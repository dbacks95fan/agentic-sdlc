# Governance

## Decision rights

| Decision | Authority | Required record |
| --- | --- | --- |
| Create/refine pre-freeze intent | Product owner with Intent Creation Skill | Intent version and card synchronization metadata |
| Select or revise a product Policy & Compliance Profile | Product owner or delegated accountable owner | Profile ID, version, applicability decisions, controls, and rationale |
| Prioritize and commit engineering capacity | Product owner authorizes; Conductor applies | Trello transition and complete freeze tuple |
| Withdraw a commitment | Product owner authorizes; Conductor applies when the card returns to Backlog or New Ideas | Record freeze revocation; prior tuple remains in history |
| Accept `spec.md` and work contract for Execution | Product owner authorizes; Conductor applies | Spec Acceptance Record binding both immutable artifacts |
| Move work from Prioritized to Spec & Design | Conductor under authorized policy | Valid freeze tuple and worker assignment |
| Move work from Spec & Design to Execution | Conductor after product-owner acceptance | Spec Acceptance Record |
| Return Evaluation work to Execution | Conductor under the defined policy | Evaluation finding and immutable candidate reference |
| Return work to Spec & Design for a non-material correction | Accountable human; Conductor applies | Finding or feedback and preserved frozen intent reference |
| Approve candidate for integration and Release, request rework, or stop work | Accountable human | Candidate SHA, decision, rationale, and time |
| Merge and release an approved candidate | Authorized integration and release path | Human approval, integration SHA, and Release Record |
| Close released, stopped, or rolled-back work | Accountable human | Observation Record or Closure Record and linked follow-up, if any |
| Advance a card within the current lifecycle | Conductor under the defined policy | Required artifact reference and transition record |

## Controls

- Configure control-plane and execution-plane LLMs independently. Treat their provider, model, and version as operational metadata, not as a source of authority or a replacement for a control.
- Authorize each workflow role only for its approved actions and apply least privilege.
- Validate identity, revision, and hash preconditions before an agent runs.
- Treat card, intent, specification, policy-profile, and worker-result content as untrusted data; it cannot override the documented permissions or control-plane policy.
- Make required checks executable: schemas, contract validation, CI, structural checks, and policy checks.
- Preserve lineage from intent to spec and execution artifacts.
- Do not erase a prior freeze record when a pre-execution commitment is withdrawn; record revocation and keep the previous intent revision retrievable.
- Use append-only or immutable records where the platform permits.
- Do not treat agent narrative, green tests, or a successful deployment command as sufficient evidence by themselves.
- Do not let an agent decide legal applicability, grant a policy exception, or claim compliance. It may identify a defined trigger and request an authorized human decision.
- Keep candidate assessment independent from implementation. An Evaluator may provide evidence and findings, but only a human can approve release.
- Preserve release and observation facts against the immutable candidate SHA. Do not treat an execution report, a green test suite, or a completed release command as outcome proof.
- Automatically retry only idempotent, clearly transient failures with no unknown side effect. Record each retry; escalate all other failures to an authorized human.

## Exceptions and incidents

Any workflow exception records its scope, approver, rationale, expiry, and compensating controls. Agent failures caused by missing, malformed, mismatched, or unapproved inputs are system/input errors—not implementation passes. Security incidents, unexpected access, material execution disagreement, or production harm pause the affected workflow and require human triage.

## Transparency

Trello cards should expose a concise status summary so a nontechnical stakeholder can understand the current column, durable artifact references, unresolved decisions, and next action without entering a repository.
