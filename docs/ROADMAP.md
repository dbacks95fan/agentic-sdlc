# Implementation Roadmap

## Phase 0 — Establish the contract

- Adopt this documentation baseline and assign decision owners.
- Define product registry and ID conventions.
- Define the canonical intent schema, Trello field mapping, and synchronization conflict policy.

## Phase 1 — Product intake and commitment

- Implement the Intent Creation Skill’s product/intent context handling.
- Create the dedicated `intent-backlog` repository and product namespaces.
- Version the canonical Trello board template and configure lists: New Ideas, Backlog, Prioritized.
- Provide an idempotent Conductor board-bootstrap command that creates and validates the template without moving or deleting existing work.
- Implement bidirectional synchronization checks and the immutable freeze record at Prioritized.

## Phase 2 - Spec & Design

- Implement Conductor preflight and isolated worktree creation.
- Copy and verify frozen intent bytes in the target repository.
- Implement a durable `spec.md` artifact and its transition to Execution.

## Phase 3 - Execution

- Build the first execution capability only when a specific work item establishes the required behavior, artifacts, and controls.
- Measure flow, rework, and failures; add a lifecycle stage only when those findings justify it.

## Decisions still required before production autonomy

- Trello authorization model and which transitions the Conductor may execute automatically.
- Artifact retention, worktree lifecycle, and access-control requirements.
- The next needed Execution capability, based on work that has reached Spec & Design.
