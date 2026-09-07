# Governance

## Decision rights

| Decision | Authority | Required record |
| --- | --- | --- |
| Create/refine pre-freeze intent | Product owner with Intent Creation Skill | Intent version and card synchronization metadata |
| Prioritize and commit engineering capacity | Authorized product/engineering owner | Trello transition and frozen revision reference |
| Publish a reviewable lifecycle artifact | Authorized workflow coordination path | Immutable commit SHA, repository-relative path, and reachable reference |
| Advance a stage after worker completion | Conductor under the defined gate policy | Required artifact and validation evidence |
| Approve design | Human Design Reviewer | Design-review decision linked to `spec.md` |
| Change implementation plan | Authorized reviewer | Updated plan/work contract with rationale |
| Decide evaluation disposition | Human approval authority | Evaluation decision brief and disposition |
| Release to production | Release authority under product controls | Release and health evidence |

## Controls

- Authorize each workflow role only for its approved actions and apply least privilege.
- Validate identity, revision, hash, and approval preconditions before an agent runs.
- Make required checks executable: schemas, contract validation, CI, structural checks, and policy checks.
- Preserve lineage from intent to spec, plan, code, validation, evaluation, approval, release, and production observation.
- Use append-only or immutable records for approvals and evaluation evidence where the platform permits.
- Do not treat agent narrative, green tests, or a successful deployment command as sufficient evidence by themselves.

## Exceptions and incidents

Any workflow exception records its scope, approver, rationale, expiry, and compensating controls. Agent failures caused by missing, malformed, mismatched, or unapproved inputs are system/input errors—not implementation passes. Security incidents, unexpected access, material evaluation disagreement, or production harm pause the affected workflow and require human triage.

## Transparency

Trello cards should expose a concise decision brief so a nontechnical stakeholder can understand status, criteria, verified findings, risks, unresolved decisions, and the next action without entering a repository.
