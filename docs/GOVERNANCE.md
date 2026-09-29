# Governance

## Decision rights

| Decision | Authority | Required record |
| --- | --- | --- |
| Create/refine pre-freeze intent | Product owner with Intent Creation Skill | Intent version and card synchronization metadata |
| Select or revise a product Policy & Compliance Profile | Product owner or delegated accountable owner | Profile ID, version, applicability decisions, controls, and rationale |
| Prioritize and commit engineering capacity | Authorized product/engineering owner | Trello transition and frozen revision reference |
| Accept `spec.md` and work contract for Execution | Authorized product owner | Immutable artifact references and approval record |
| Move work from Prioritized to Spec & Design | Authorized workflow coordination path | Frozen revision reference and assigned worktree |
| Move work from Spec & Design to Execution | Authorized workflow coordination path | Immutable `spec.md` reference |
| Return Evaluation work to Execution | Conductor under the defined policy | Evaluation finding and immutable candidate reference |
| Approve candidate for Release, request rework, or stop work | Accountable human | Candidate SHA, decision, rationale, and time |
| Release an approved candidate | Authorized release path | Human approval and Release Record |
| Close work after observation | Accountable human | Observation Record and linked follow-up, if any |
| Advance a card within the current lifecycle | Conductor under the defined policy | Required artifact reference and transition record |

## Controls

- Configure control-plane and execution-plane LLMs independently. Treat their provider, model, and version as operational metadata, not as a source of authority or a replacement for a control.
- Authorize each workflow role only for its approved actions and apply least privilege.
- Validate identity, revision, and hash preconditions before an agent runs.
- Make required checks executable: schemas, contract validation, CI, structural checks, and policy checks.
- Preserve lineage from intent to spec and execution artifacts.
- Use append-only or immutable records where the platform permits.
- Do not treat agent narrative, green tests, or a successful deployment command as sufficient evidence by themselves.
- Do not let an agent decide legal applicability, grant a policy exception, or claim compliance. It may identify a defined trigger and request an authorized human decision.
- Keep candidate assessment independent from implementation. An Evaluator may provide evidence and findings, but only a human can approve release.
- Preserve release and observation facts against the immutable candidate SHA. Do not treat an execution report, a green test suite, or a completed release command as outcome proof.

## Exceptions and incidents

Any workflow exception records its scope, approver, rationale, expiry, and compensating controls. Agent failures caused by missing, malformed, mismatched, or unapproved inputs are system/input errors—not implementation passes. Security incidents, unexpected access, material execution disagreement, or production harm pause the affected workflow and require human triage.

## Transparency

Trello cards should expose a concise status summary so a nontechnical stakeholder can understand the current column, durable artifact references, unresolved decisions, and next action without entering a repository.
