# Implementation Roadmap

The documentation defines the intended lifecycle. Implement it in small, observable increments and do not claim the automation exists before its contract checks prove it. Start with a low-risk application feature and keep humans in each consequential gate while the workflow is learned.

## Phase 0 — Establish the contract

- Adopt this documentation baseline and assign decision owners.
- Define independently configurable control-plane and execution-plane LLM assignments, including how run metadata will be recorded. Do not make model selection a substitute for role permissions or validation controls.
- Define product registry and ID conventions.
- Define the required intent invariants, Trello/freeze-field mapping, and synchronization conflict policy without prescribing an `intent.md` layout.
- Adopt the [stage contracts](STAGE_CONTRACTS.md) and a lean, versioned Policy & Compliance Profile for each product.

## Phase 1 — Product intake and commitment

- Implement the Intent Creation Skill’s product/intent context handling.
- Create the dedicated `intent-backlog` repository and product namespaces.
- Version the canonical Trello board template and configure lists: New Ideas, Backlog, Prioritized, Spec & Design, Execution, Evaluation, Human Approval, Release, Production Observation, and Done.
- Provide an idempotent Conductor board-bootstrap command that creates and validates the template without moving or deleting existing work.
- Implement bidirectional synchronization checks and the immutable freeze record at Prioritized.

## Phase 2 - Spec & Design

- Implement Conductor preflight and worker assignment for isolated worktree creation.
- Copy and verify frozen intent and policy-profile bytes in the target repository without checkout line-ending conversion.
- Implement sectioned `spec.md` production, proposed `work-contract.yaml`, and the human acceptance gate within Spec & Design.

## Phase 3 - Execution and evaluation

- Align the Coding Agent with the accepted work contract; require independently observed validation and an Evidence Package before reporting a candidate result.
- Implement the independent Evaluator contract and Evaluation Package; route failures back to Execution without changing frozen inputs.
- Configure Conductor column triggers for Spec & Design, Execution, and Evaluation only after it validates the canonical contracts.

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
