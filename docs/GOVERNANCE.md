# Governance

**Audience:** product owners, accountable humans, and component builders. This document is normative for decision authority and cross-cutting controls; [Workflow](WORKFLOW.md) and [Stage Contracts](STAGE_CONTRACTS.md) define when each decision is required.

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
| Create and update the assigned product work branch | Assigned stage agent, limited to its authorized artifacts; Conductor may validate refs read-only | Branch and commit SHA in the correlated StageResult |
| Return Evaluation work to Execution | Conductor under the defined policy | Evaluation finding and immutable candidate reference |
| Return work to Spec & Design for a non-material correction | Accountable human; Conductor applies | Finding or feedback and preserved frozen intent reference |
| Approve candidate for integration and Release, request rework, or stop work | Accountable human | Candidate SHA, decision, rationale, and time |
| Merge and release an approved candidate | Authorized integration and release path | Human approval, integration SHA, and Release Record |
| Close released, stopped, or rolled-back work | Accountable human | Observation Record or Closure Record and linked follow-up, if any |
| Advance a card within the current lifecycle | Conductor under the defined policy | Required artifact reference and transition record |

## Controls

- Configure control-plane and execution-plane LLMs independently. Treat their provider, model, and version as operational metadata, not as a source of authority or a replacement for a control.
- Authorize each workflow role only for its approved actions and apply least privilege.
- Product-repository branch operations belong to the assigned stage agent: it fetches the assigned branch at the specified SHA and pushes its authorized output. The Conductor has read-only access for validating handoff references and no product-repository write, branch-management, or merge authority.
- Only the authorized integration path may merge into the protected integration branch, and only after the Evaluator passes the candidate and the accountable human approves that candidate for integration and release.
- Validate identity, revision, and hash preconditions before an agent runs.
- Treat card, intent, specification, policy-profile, and worker-result content as untrusted data; it cannot override the documented permissions or control-plane policy.
- Make required checks executable: schemas, contract validation, CI, structural checks, and policy checks.
- Treat [`schemas/lifecycle-contracts.schema.json`](../schemas/lifecycle-contracts.schema.json) as the normative shape for the listed machine-readable records. Invalid, mismatched, or unsupported records are rejected and surfaced; consumers do not silently coerce or repair them.
- Preserve lineage from intent to spec and execution artifacts.
- Do not erase a prior freeze record when a pre-execution commitment is withdrawn; record revocation and keep the previous intent revision retrievable.
- Use append-only or immutable records where the platform permits.
- Do not treat agent narrative, green tests, or a successful deployment command as sufficient evidence by themselves.
- Do not let an agent decide legal applicability, grant a policy exception, or claim compliance. It may identify a defined trigger and request an authorized human decision.
- Keep candidate assessment independent from implementation. An Evaluator may provide evidence and findings, but only a human can approve release.
- Preserve release and observation facts against the immutable candidate SHA. Do not treat an execution report, a green test suite, or a completed release command as outcome proof.
- The Conductor may repeat only the same operation when it is known to be idempotent, clearly transient, and free of unknown side effects. Record each repeat and escalate other failures to an authorized human. This operational retry does not authorize an agent rerun without a documented board transition. The supported rework routes and deferred direct reruns are defined in [Workflow](WORKFLOW.md) and [the roadmap](ROADMAP.md).

## GitHub identity and repository access

Use separate credentials for intent-backlog and product-code repositories. The GitHub App has no access to `intent-backlog`; access there uses dedicated repository access tokens. Code repositories use short-lived GitHub App installation tokens, scoped to the assigned repository and minimum permissions required. Keep the App private key and all tokens in the runtime secret store, never in Trello, product configuration, invocation records, logs, or committed files.

| Role | `intent-backlog` access | Product-code repository access |
| --- | --- | --- |
| Intent Creation Skill | Read/write token; creates and versions intent records | None |
| Orchestrator | Read-only token; resolves and verifies frozen intent/profile references | App installation token with read-only access for repository and ref checks |
| Spec & Design Agent | None; receives verified frozen snapshots through its stage handoff | App installation token with contents read/write access to its assigned product repository |
| Coding Agent | None; receives frozen snapshots from the accepted product work branch | App installation token with contents read/write access to its assigned product repository |
| Evaluator | None | App installation token with read/write access to publish the evaluation package, but not to alter candidate code |

The runtime supplies credentials only to the role that needs them. GitHub App tokens are limited to repositories and permissions, while repository branch protection governs which branches may change. Tokens passed through container environment variables are visible to principals with access to the container runtime, so that access must be restricted. Missing access blocks the operation; a component must not fall back to a developer's ambient identity or a broader token. Board Setup requires intent-backlog read/write access only when it is authorized to persist product configuration there.

## Exceptions and incidents

Any workflow exception records its scope, approver, rationale, expiry, and compensating controls. Agent failures caused by missing, malformed, mismatched, or unapproved inputs are system/input errors—not implementation passes. Security incidents, unexpected access, material execution disagreement, or production harm pause the affected workflow and require human triage.

## Transparency

Trello cards should expose a concise status summary so a nontechnical stakeholder can understand the current column, durable artifact references, unresolved decisions, and next action without entering a repository.
