# Workflow

## Product flow

```text
New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution
```

| Column | Primary action | Exit condition |
| --- | --- | --- |
| New Ideas | Capture an idea and refine its product context and intent | A coherent intent is ready for future consideration |
| Backlog | Hold a valid, non-committed intent | Priority decision |
| Prioritized | Queue work for execution and capture the full freeze tuple | Exact frozen intent is available to Spec & Design |
| Spec & Design | Produce `spec.md` from the frozen intent | A specification and design artifact is ready for Execution |
| Execution | Carry out the work from the frozen intent and `spec.md` | Define further workflow only when real work establishes its need |

## Intent Creation boundary

The Intent Creation skill recognizes only `New Ideas` and `Backlog` as editable locations. It may create or revise an intent only while its card is in one of those columns.

When a user or user interface explicitly selects a handoff destination, the skill may freeze a synchronized Backlog intent and move its card to that selected destination. It uses the selected list identifier and does not infer meaning from a downstream list's name or visual position. On the current board, `Prioritized` is the usual handoff destination; the skill does not depend on that name.

If a card is already outside `New Ideas` or `Backlog`, the skill must not update the intent or move the card. It reports the card's current column and that the intent has been handed off.

## Prioritized freeze protocol

1. Confirm card/intent identity and synchronization.
2. Resolve material ambiguity and acceptance criteria before commitment.
3. Record intent ID, product ID, intent version, intent commit SHA, normalized content hash, frozen-artifact hash, card ID, freeze time, and acceptance metadata.
4. Mark the revision immutable and establish the assigned product-repository branch/worktree.
5. Copy the frozen bytes into `.agent/work/<intent-id>/intent.md` before Spec & Design begins.

If a material change is requested after execution begins, create a successor intent and route it through normal product flow. Do not mutate the frozen artifact or work beneath active agents.

## Artifact handoff

Before a card moves from Spec & Design to Execution, record the durable `spec.md` artifact, its immutable commit SHA, repository-relative path, and a reachable reference. This makes the execution input unambiguous without changing the frozen intent.

The five lifecycle names in this document are the canonical Trello workflow vocabulary. Do not create an additional board stage until real work has established its purpose, ownership, and exit condition.
