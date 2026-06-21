# bytes question banks

Rapid-fire practice question banks for the shelnet site's `/bytes` feature. Each
file here is one cert's bank, fetched lazily by the site when a user opens that
cert's quiz. **The site is the source of truth for this format**; if you change
the shape, change it on the site too.

## File: `<slug>.json`

One file per cert, named by the cert's manifest slug (e.g. `security-plus.json`):

```json
{
  "cert": "security-plus",
  "version": 1,
  "questions": [
    {
      "id": "secplus-0001",
      "domain": "General Security Concepts",
      "stem": "Which control type is a security guard?",
      "choices": ["Technical", "Managerial", "Operational", "Physical"],
      "correct": [3],
      "explanation": "A guard is a physical control; it protects the facility by physical means."
    }
  ]
}
```

### Field rules

- `cert` - the cert slug; must match the manifest key and the filename.
- `version` - bank schema version; currently `1`.
- `questions` - array of question objects. Required (the site rejects a bank
  without a `questions` array).

Per question:
- `id` - stable, unique within the bank. Scheme: `secplus-NNNN`, zero-padded to
  4 digits (`secplus-0001`, `secplus-0002`, ...). Never renumber an existing id.
- `domain` - optional but expected; one value from the controlled vocabulary
  below. Rendered uppercased on the card.
- `stem` - the question text.
- `choices` - array of answer strings. Max 9 (the runner answers keys `1-9`).
- `correct` - array of indices into `choices`. Length 1 today; the schema
  supports multi-answer ("choose two") without migration.
- `explanation` - required. This is the study payoff shown after answering.

## Controlled domain vocabulary

Tag each `domain` with one of these. Use the official CompTIA names verbatim.

### Security+ (SY0-701)

- `General Security Concepts`
- `Threats, Vulnerabilities, and Mitigations`
- `Security Architecture`
- `Security Operations`
- `Security Program Management and Oversight`

## Going live with a cert

A bank file alone does **not** make a cert appear in `/bytes`. The site only
shows a cert whose `manifest.json` entry has a `bytes` field. So, in one commit:

1. Populate this cert's `questions`.
2. Add `"bytes": { "count": <n> }` to the cert's entry in `manifest.json`, where
   `<n>` equals the number of questions in the bank.
3. Deploy resources (GitHub Pages) first; the site push happens separately.

A cert with an empty `questions` array and no manifest `bytes` field (the current
`security-plus.json` state) is inert: it ships nothing and the site shows its
"No question banks yet." state.
