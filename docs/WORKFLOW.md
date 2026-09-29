# Workflow

## Product flow

```text
New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution
                                                        ^         |
                                                        |         v
Done <- Production Observation <- Release <- Human Approval <- Evaluation
```

| Column | Primary action | Exit condition |
| --- | --- | --- |
| New Ideas | Capture an idea and refine its product context and intent | A coherent intent is ready for future consideration |
| Backlog | Hold a valid, non-committed intent | Product owner explicitly queues it for engineering |
| Prioritized | Commit engineering capacity and capture the freeze tuple | Exact frozen intent is available to Spec & Design |
| Spec & Design | Produce and review `spec.md` and the proposed work contract | Owner accepts the specification and moves the work to Execution |
| Execution | Build the candidate from the accepted durable inputs | Candidate commit and Evidence Package are ready for independent evaluation |
| Evaluation | Independently compare candidate and evidence to the frozen inputs | Pass goes to Human Approval; failure returns to Execution; uncertainty is escalated |
| Human Approval | Make the accountable release decision | Approve for Release, request rework, or stop the work |
| Release | Deliver the approved candidate using the product's release controls | Release Record has verified delivery and health facts |
| Production Observation | Observe the defined outcome and operational signals | Observation Record is complete and follow-up is decided |
| Done | Close the completed work item | Human confirms the observation outcome and closure |

The required stage inputs, outputs, decision gates, and agent statuses are defined in [Stage Contracts](STAGE_CONTRACTS.md). The human review of `spec.md` occurs while a card remains in `Spec & Design`; a move into `Execution` triggers the Coding Agent only after the Conductor validates the accepted work contract. Entries into `Spec & Design`, `Execution`, and `Evaluation` are the planned agent-trigger points. `Human Approval` is always a human decision.

## Intent Creation boundary

The Intent Creation Skill may create or revise an intent only while its card is in `New Ideas` or `Backlog`. If the card is in any other column, it must leave both the card and intent unchanged and tell the user that the intent has already been handed off.

From Backlog, a product owner may explicitly select a handoff destination. The skill must freeze the synchronized intent and place the card in that selected destination. It must not infer meaning from a destination's name, visual position, or any other downstream board detail. After that handoff, routing belongs to the Conductor and the skill does not change the card or intent.

## Intent Creation boundary

The Intent Creation Skill may create or revise an intent only while its card is in `New Ideas` or `Backlog`. If the card is in any other column, it must leave both the card and intent unchanged and tell the user that the intent has already been handed off.

From Backlog, a user may explicitly select a handoff destination. The skill must freeze the synchronized intent and place the card in that selected destination. It must not infer meaning from a destination's name, visual position, or any other downstream board detail. After that handoff, routing belongs to the Conductor and the skill does not change the card or intent.

## Prioritized freeze protocol

1. Confirm card/intent identity and synchronization.
2. Resolve material ambiguity and acceptance criteria before commitment.
3. Record intent ID, product ID, intent version, intent commit SHA, intent fingerprint, card ID, freeze time, and acceptance metadata.
4. Mark the revision immutable and establish the assigned product-repository branch/worktree.
5. Copy the frozen bytes into `.agent/work/<intent-id>/intent.md` before Spec & Design begins.

If a material change is requested after execution begins, create a successor intent and route it through normal product flow. Do not mutate the frozen artifact or work beneath active agents.

## Delivery and rework rules

- Evaluation `pass` moves to `Human Approval`. Evaluation `fail` returns to `Execution` with a durable finding; it never changes frozen inputs.
- A human may approve the candidate for Release, return it to Execution with bounded feedback, or stop it. A material outcome, scope, acceptance-criterion, constraint, or assumption change creates a successor intent instead of reworking the frozen intent.
- Release records what occurred, including the candidate revision, environment, verification and health-check facts, and any rollback. A release command completing is not proof that the product was delivered correctly.
- Production Observation compares actual signals to the intent's expected outcome for a defined observation window. A defect, incident, unmet outcome, or new idea becomes a distinct follow-up item; it does not silently mutate the completed lifecycle.
- `Done` means the accountable human has reviewed the Observation Record and decided either that the outcome is sufficiently understood or that the necessary follow-up is captured.

## Artifact handoff

Before a card moves from Spec & Design to Execution, record the durable `spec.md` artifact, its immutable commit SHA, repository-relative path, and a reachable reference. This makes the execution input unambiguous without changing the frozen intent.

The ten lifecycle names in this document are the canonical Trello workflow vocabulary. Do not create an additional board stage until real work has established its purpose, ownership, and exit condition.
