# Governance

## Decision rights

| Decision | Authority | Required record |
| --- | --- | --- |
| Create/refine pre-freeze intent | Product owner with Intent Creation Skill | Intent version and card synchronization metadata |
| Prioritize and commit engineering capacity | Authorized product/engineering owner | Trello transition and frozen revision reference |
| Move work from Prioritized to Spec & Design | Authorized workflow coordination path | Frozen revision reference and assigned worktree |
| Move work from Spec & Design to Execution | Authorized workflow coordination path | Immutable `spec.md` reference |
| Advance a card within the current lifecycle | Conductor under the defined policy | Required artifact reference and transition record |

## Controls

- Authorize each workflow role only for its approved actions and apply least privilege.
- Validate identity, revision, and hash preconditions before an agent runs.
- Make required checks executable: schemas, contract validation, CI, structural checks, and policy checks.
- Preserve lineage from intent to spec and execution artifacts.
- Use append-only or immutable records where the platform permits.
- Do not treat agent narrative, green tests, or a successful deployment command as sufficient evidence by themselves.

## Exceptions and incidents

Any workflow exception records its scope, approver, rationale, expiry, and compensating controls. Agent failures caused by missing, malformed, mismatched, or unapproved inputs are system/input errors—not implementation passes. Security incidents, unexpected access, material execution disagreement, or production harm pause the affected workflow and require human triage.

## Transparency

Trello cards should expose a concise status summary so a nontechnical stakeholder can understand the current column, durable artifact references, unresolved decisions, and next action without entering a repository.
