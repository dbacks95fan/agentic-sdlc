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
