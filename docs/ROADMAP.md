# Implementation Roadmap

## Phase 0 — Establish the contract

- Adopt this documentation baseline and assign decision owners.
- Define product registry and ID conventions.
- Define the canonical intent schema, Trello field mapping, and synchronization conflict policy.
- Define work-contract and evaluator-result schemas plus validation fixtures.

## Phase 1 — Product intake and commitment

- Implement the Intent Creation Skill’s product/intent context handling.
- Create the dedicated `intent-backlog` repository and product namespaces.
- Configure Trello lists: New Ideas, Product Refinement, Backlog, Prioritized.
- Implement bidirectional synchronization checks and the immutable freeze record at Prioritized.

## Phase 2 — Engineering artifact chain

- Implement Conductor preflight and isolated worktree creation.
- Copy and verify frozen intent bytes in the target repository.
- Implement templates and human Design Review for `spec.md`.
- Implement planning output, work contract, and deterministic validation gates.

## Phase 3 — Independent execution and evaluation

- Connect the Claude-oriented Coding Agent to contracted worktrees.
- Connect the Codex-oriented Evaluator in read-only mode.
- Persist structured evidence, evaluation results, and card decision briefs.
- Pilot with low-risk work and measure gate failures, rework, and cycle time.

## Phase 4 — Release and learning loop

- Integrate human approval and existing release controls.
- Verify health after deployment; capture release and production-observation evidence.
- Feed measured outcomes, incidents, and recurring evaluator findings into backlog refinement and mechanical controls.

## Decisions still required before production autonomy

- Exact canonical schemas and storage/retention requirements.
- Trello authorization model and which transitions the Conductor may execute automatically.
- Human approval roles, risk thresholds, and exception policy.
- Artifact retention, worktree lifecycle, and access-control requirements.
- Release environments, rollback authority, production-success measures, and observability ownership.
