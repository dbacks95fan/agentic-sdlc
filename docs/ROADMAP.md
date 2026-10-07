# Implementation Roadmap

The documentation defines the intended lifecycle. Implement it in small, observable increments and do not claim the automation exists before its contract checks prove it. Start with a low-risk application feature and keep humans in each consequential gate while the workflow is learned.

## Phase 0 — Establish the contract

- Adopt this documentation baseline and assign decision owners.
- Define independently configurable control-plane and execution-plane LLM assignments, including how run metadata will be recorded. Do not make model selection a substitute for role permissions or validation controls.
- Define product registry and ID conventions.
- Define the required intent invariants, Trello/freeze-field mapping, and synchronization conflict policy without prescribing an `intent.md` layout.
- Adopt the [stage contracts](STAGE_CONTRACTS.md) and a lean, versioned Policy & Compliance Profile for each product.

## Phase 1 — Product intake and commitment

- Completed: the separately packaged Intent Creation Skill creates the initial New Ideas card and synchronizes its versioned `intent.md` to the dedicated GitHub `intent-backlog` repository. It does not move cards or control freeze state. Its contract validation is text-level; authenticated create verification remains an integration check.
- Completed: the dedicated `intent-backlog` repository and product namespaces are configured.
- Completed: version the canonical Trello board template with New Ideas, Backlog, Prioritized, Spec & Design, Execution, Evaluation, Human Approval, Release, Production Observation, and Done.
- Completed: the PerioParrot board was verified against the ten-list template; its order matches the documented flow.
- In progress: Conductor feature branch implements the Prioritized freeze projection and visible activation/revocation events. The Spec & Design handoff verifies the same frozen intent and policy-profile bytes.
- Pending: merge and deploy that Conductor change, then verify it against the live Trello webhook and a controlled test card.

## Phase 2 - Spec & Design

- In progress: Spec & Design request contract carries Prioritized authorization and the pinned policy-profile reference; its worker validates raw bytes and stages both frozen inputs in an isolated workspace.
- Pending: verify the published `spec.md` and proposed `work-contract.yaml` against the human acceptance gate and record a durable Spec Acceptance Record before Execution.

## Phase 3 - Execution and evaluation

- Align the Coding Agent with the accepted work contract; require independently observed validation and an Evidence Package before reporting a candidate result.
- Implement the independent Evaluator contract and Evaluation Package; route failures back to Execution without changing frozen inputs.
- In progress: Conductor recognizes Spec & Design, Execution, and Evaluation list entry as stage triggers. Its existing Coding Agent adapter still consumes a legacy card-derived contract and must be reconciled with the accepted `spec.md` and `work-contract.yaml` before end-to-end deployment.

## Phase 4 - Accountable delivery and learning

- Implement the Human Approval decision record tied to an immutable candidate SHA.
- Integrate the approved candidate into the protected product branch, record the immutable integration SHA, and produce a Release Record with post-release verification and rollback facts.
- Define a short observation window and signals for the first real feature; produce an Observation Record and capture any follow-up as new intent.
- Review flow, rework, release, and outcome evidence. Improve a documented rule or control only when the evidence supports it.
- Add automated checks for local documentation links, lifecycle vocabulary/template consistency, and required freeze and handoff fields.

## Decisions still required before production autonomy

- Trello authorization model and which transitions the Conductor may execute automatically.
- Artifact retention, worktree lifecycle, and access-control requirements.
- The concrete release authorization, observation window, and operational signals for each product.
- The next automation capability, based on a completed learning feature and its evidence.

## Deferred exception handling

The initial implementation and validation focus on the documented happy path. Define reruns of an agent stage after review and out-of-order or unexpected Trello card moves separately before supporting them. Their triggers, Conductor behavior, state reconciliation, and audit records are not implied by the happy-path contracts.
