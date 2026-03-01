---
title: Example Writeup Title
cve: CVE-XXXX-XXXX
severity: critical
port: 21/tcp
service: FTP
target: Metasploitable 2
date: 2026-02-28
description: Short one-line summary for the index card.
---

## Overview

Brief description of the vulnerability, what it affects, and why it matters. Include the software name, version, and a high-level explanation of the flaw.

The vulnerability allows an attacker to perform **unauthenticated remote code execution** by sending a specially crafted request to the target service.

## Environment Setup

This write-up was performed in an isolated homelab environment. No live or production systems were targeted.

- **Attacker:** Kali Linux (192.168.56.10)
- **Target:** Metasploitable 2 (192.168.56.11) — host-only adapter
- **Tools:** Nmap, Netcat, Metasploit Framework (optional)

## Discovery

Start with a service version scan to identify the vulnerable service:

```bash
nmap -sV -p 21 192.168.56.11
```

Expected output:

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
```

Cross-reference the identified version against known CVEs. The Nmap scripting engine can also perform targeted checks:

```bash
nmap --script ftp-vsftpd-backdoor -p 21 192.168.56.11
```

## Exploitation

Describe the exploitation process step by step. Include the exact commands used and explain what each one does.

```bash
# Step 1: Trigger the vulnerability
nc 192.168.56.11 21

# Step 2: Connect to the resulting shell or confirm the impact
nc 192.168.56.11 6200
```

Explain what happens internally when the exploit fires — what code path is triggered, what privileges are gained, etc.

## Proof of Concept

Confirm successful exploitation with verification commands:

```bash
id
# uid=0(root) gid=0(root) groups=0(root)

whoami
# root
```

Include any relevant output that demonstrates the impact.

## Remediation

List concrete, actionable remediation steps:

1. **Upgrade the software** — Replace the vulnerable version with a patched release. Verify checksums against trusted sources.
2. **Apply vendor patches** — Check the vendor advisory for official patches.
3. **Network segmentation** — Restrict service access to trusted hosts via firewall rules.
4. **Disable if unused** — If the service is not required, disable it entirely.
5. **Monitor for indicators of compromise** — Set up alerts for unusual activity on the affected port.

## References

- [NVD - CVE-XXXX-XXXX](https://nvd.nist.gov/vuln/detail/CVE-XXXX-XXXX)
- [Vendor Security Advisory](https://example.com/advisory)
- [Offensive Security - Metasploitable 2 Guide](https://docs.rapid7.com/metasploit/metasploitable-2/)
