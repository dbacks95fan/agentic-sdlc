# Policy & Compliance Profile

Profile ID: `<PRODUCT>-DEFAULT`

Version: `1`

Owner: `<authorized product owner>`

Data classification: `None identified | Public | Internal | Confidential | Regulated`

## Applicability decisions

| Consideration | Status | Decision rationale | Human owner |
| --- | --- | --- | --- |
| Baseline secure development | Applies | Required for every product | `<owner>` |
| PCI DSS / payment-card data | Not applicable | `<reason>` | `<owner>` |
| HIPAA / ePHI | Not applicable | `<reason>` | `<owner>` |
| SOX / financial-reporting controls | Not applicable | `<reason>` | `<owner>` |
| SOC 2 / customer assurance commitment | Not applicable | `<reason>` | `<owner>` |
| Other | Not applicable | `<reason>` | `<owner>` |

## Controls

| Control ID | Required agent action | Required evidence | Escalation trigger |
| --- | --- | --- | --- |
| SEC-BASE-001 | Do not expose or hardcode secrets. | Secret scan or equivalent deterministic check. | A secret, credential, or unapproved secret store is needed. |
| SEC-BASE-002 | Identify relevant input, access, and data-handling risks. | Tests, validation result, or documented rationale in `spec.md`. | A material risk cannot be addressed within the frozen scope. |

## Approval and history

- Approved by: `<owner>`
- Approved at: `<timestamp>`
- Change rationale: `<reason>`
- Supersedes: `<profile ID and version, if any>`
