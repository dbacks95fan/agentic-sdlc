# Workflow

**Audience:** product owners, Conductor builders, and lifecycle component builders. This document is the normative source for board stages, transition rules, and rework routes. Stage-specific inputs, outputs, and worker requirements are in [Stage Contracts](STAGE_CONTRACTS.md).

## Product flow

```text
New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution
                                                        ^         |
                                                        |         v
Done <- Production Observation <- Release <- Human Approval <- Evaluation
```

| Column | Primary action | Exit condition |
| --- | --- | --- |
| New Ideas | Capture an idea and refine its product context and intent. | A coherent intent is ready for future consideration. |
| Backlog | Hold a valid intent that may be refined. A card returned from Prioritized is editable again after the Conductor revokes its active freeze. | Product owner authorizes its handoff to Prioritized. |
| Prioritized | Commit engineering capacity and record the freeze tuple. | Frozen intent and policy-profile references are valid; the control plane queues Spec & Design. A return to Backlog or New Ideas revokes the active freeze. |
| Spec & Design | Consult product-level constitution and architecture; produce and review `spec.md` and the proposed work contract for the intent. | Owner accepts both artifacts; the acceptance record freezes their revisions and authorizes Execution. Any conflict with the product baseline or required baseline change is surfaced for human decision, not silently resolved by the agent. |
| Execution | Build the candidate from accepted durable inputs. | Candidate commit and Evidence Package are ready for independent evaluation. |
| Evaluation | Independently compare candidate and evidence to frozen inputs. | Pass goes to Human Approval; implementation findings return to Execution; specification findings return to Spec & Design. |
| Human Approval | Make the accountable integration and release decision. | Approve integration/release, request bounded rework, or stop the work. |
| Release | Integrate the approved candidate and deliver the approved integration revision. | Release Record has integration, delivery, and health-check facts. |
| Production Observation | Observe the agreed outcome and operational signals. | Observation Record and human closure decision are complete. |
| Done | Close delivered or deliberately stopped work. | Human closure record explains the outcome and any follow-up. |

The required stage inputs, outputs, decision gates, and agent statuses are defined in [Stage Contracts](STAGE_CONTRACTS.md). Entry into `Spec & Design` triggers the Spec & Design stage: the Conductor validates the frozen inputs, prepares the isolated workspace handoff, and invokes the Spec & Design Agent. Entry into `Execution` and `Evaluation` triggers their respective agents under the same stage contracts. `Human Approval` is always a human decision. The Conductor is the sole control-plane identity that writes Trello workflow state; a human card move is a transition request that it validates and records.

## Conductor event and trigger contract

The Trello adapter maps a board move to a `BoardTransitionEvent` as defined by the [lifecycle schemas](../schemas/lifecycle-contracts.schema.json). The product configuration identifies the expected board. The Conductor verifies the event source, board and card identity, and current card/list state against Trello before applying a workflow action; it does not treat webhook payload text as authority. Actual Trello list IDs are resolved to the canonical stage names in this document using that board's configured lists. A missing, duplicate, or ambiguous required list mapping blocks processing rather than guessing from visual order.

| Validated transition into | Conductor action |
| --- | --- |
| `Prioritized` from `Backlog` | Validate intent/profile references, append the freeze activation event, and update the freeze projection. Do not invoke an engineering agent yet. |
| `Backlog` or `New Ideas` from a frozen stage | Revoke the active freeze and preserve the activation record. |
| `Spec & Design` | Validate the active freeze and invoke Spec & Design with a `StageInvocation`. |
| `Execution` | Require the accepted Spec Acceptance Record and invoke Coding with its immutable inputs. |
| `Evaluation` | Require the candidate and Evidence Package and invoke the independent Evaluator. |
| `Human Approval`, `Release`, `Production Observation`, or `Done` | Apply only the human decision and artifact conditions defined for that stage; do not substitute an agent result for the required human decision. |

The same board event may be delivered more than once. The Conductor uses the event ID to make processing idempotent and must not create duplicate stage invocations from duplicate delivery. If the current board state no longer matches the event or the move is outside the documented in-order lifecycle, the Conductor records a blocked result and asks for human resolution; the detailed out-of-order recovery flow remains deferred. A direct after-review rerun without one of the documented board transitions is not inferred from repeated webhook delivery.

### Allowed in-order transitions

| From | To | Authorized cause and required condition |
| --- | --- | --- |
| New Ideas | Backlog | Product owner moves a coherent, synchronized intent for future consideration. No agent is invoked. |
| Backlog | Prioritized | Product owner commits capacity; Conductor validates and records the active freeze before accepting the transition. |
| Prioritized | Spec & Design | Conductor routes only after the freeze is valid; entry invokes Spec & Design. |
| Prioritized or a later pre-execution stage | Backlog or New Ideas | Product owner withdraws commitment; Conductor records freeze revocation before allowing further intent edits. |
| Spec & Design | Execution | Product owner accepts both artifacts; Conductor records the Spec Acceptance Record and then invokes Coding on Execution entry. |
| Execution | Evaluation | Conductor accepts a valid `candidate_complete` result and reachable Evidence Package; entry invokes the Evaluator. |
| Evaluation | Human Approval | Conductor accepts a valid `pass` Evaluation Package. |
| Evaluation | Execution | Conductor routes a valid implementation `fail` with durable findings as bounded rework input. |
| Evaluation | Spec & Design | Accountable human authorizes a non-material specification correction; Conductor preserves the frozen intent and routes the recorded finding. |
| Human Approval | Release | Accountable human approves the named candidate for integration and release. |
| Human Approval | Execution or Spec & Design | Accountable human authorizes bounded rework; a Spec & Design return is non-material. Material intent changes after Execution begins require a successor intent and lifecycle. |
| Human Approval | Done | Accountable human stops work and records a Closure Record. |
| Release | Production Observation | Release Record status is `released`. |
| Release | Done | Release status is `rolled_back`, or an accountable human stops failed/blocked release work; a human Closure Record is required. |
| Production Observation | Done | Observation Record exists and the accountable human records closure. |

All other moves are not authorized by this happy-path contract. The Conductor holds them for human resolution and does not guess a route. A material intent change before Execution begins may return the item to Backlog or New Ideas after freeze revocation; after Execution begins, use a successor intent. A non-material specification correction follows the explicit human-approved route above and does not alter the frozen intent.

The product constitution and architecture are maintained once for the whole product in its product repository. They provide durable constraints and current-system context to each Spec & Design run; they are not recreated, copied, or separately frozen for every intent. A change to either baseline is a product-level documentation/architecture change handled through the repository's normal review process. If feature work reveals that a baseline change is needed, the Spec & Design Agent records the issue and options and waits for human direction before treating the new direction as agreed.

## Intent Creation boundary

The Intent Creation Skill creates the initial card in `New Ideas` and records the canonical intent revision and selected policy-profile ID/version. It has no authority over later board transitions or freeze state. The Conductor owns workflow metadata, including activation and revocation of freezes. Trello card projections include `Policy Profile ID`, `Policy Profile Version`, and a `Freeze State` line managed only by the Conductor.

The product owner moves the synchronized card from Backlog to `Prioritized`. The Conductor validates the handoff and records the freeze; the Intent Creation Skill does not select or perform the downstream transition.

## Prioritized freeze protocol

1. The product owner authorizes the Backlog-to-Prioritized transition.
2. The Conductor confirms card/intent identity, synchronization, material clarity, acceptance criteria, and agreement between the card and intent on policy-profile ID/version. It resolves the profile path from the product's `product.yaml` at the pinned intent commit and reads the profile from that same commit.
3. The Conductor records the complete, append-only freeze tuple defined in [Artifacts](ARTIFACTS.md) and the `FreezeActivationEvent` schema in a visible Trello comment. It updates the card's current Freeze State projection to Active.
4. A successful Prioritized freeze makes the intent and selected policy profile immutable engineering inputs. If required freeze inputs are missing or disagree, the Conductor returns the card to Backlog with a visible explanation and does not create an active freeze. If the owner returns a frozen card to Backlog or New Ideas, the Conductor updates the current Freeze State projection to Revoked and appends a visible revocation event; the prior freeze comment and Git revision remain in history. A later move to Prioritized creates a new freeze tuple for the newly synchronized revision. `Acceptance metadata` means the authorizing owner, time, and recorded decision; it does not include an unspecified second approval.
5. When `Spec & Design` starts, the first engineering worker creates the isolated branch/worktree from the assigned base revision, obtains exact byte-preserving copies of the frozen intent and policy profile, verifies their fingerprints, and reports the workspace and artifact references to the Conductor.
6. Before accepting any worker result, the Conductor verifies that the invocation's freeze remains active and its frozen input references are unchanged. A result produced after revocation or supersession cannot advance the card, even if the worker reports success; the Conductor records it as blocked/superseded and preserves the output references for audit.

After Prioritized, a material change in outcome, scope, acceptance criteria, constraints, or assumptions may be incorporated as a new version only after the card returns to Backlog or New Ideas and the Conductor revokes the freeze. This applies even if Spec & Design has started; preserve the previous Git revision and record the revocation. A non-material specification correction may return to `Spec & Design` under the rework rules below.

## Delivery and rework rules

- Evaluation `pass` moves to `Human Approval`. Evaluation `fail` returns to `Execution` with durable findings as inputs. A finding that the accepted specification is incomplete or inconsistent returns to `Spec & Design`; the frozen intent remains unchanged.
- A human may approve the candidate for integration and Release, return it to Execution with bounded feedback, return it to Spec & Design for a non-material correction, or stop it. A material product change creates a successor intent instead of reworking the frozen intent.
- After Human Approval, the authorized integration path merges the approved candidate into the product's protected integration branch, normally `main`, and records the resulting immutable integration SHA. Required integration checks must pass before that revision is released. A merge conflict or failed integration check returns the work to Execution or blocks it for human resolution.
- Release records the candidate SHA, integration SHA, environment, verification and health-check facts, and any rollback. A release command completing is not proof that the product was delivered correctly.
- `blocked` and `failed` statuses keep the card in its current column until an authorized human chooses retry, rework, stop, or another documented exception. An automatic retry is limited to a known idempotent, clearly transient operation with no unknown side effect. That operational retry repeats the same operation; it is not a new stage run. Only the listed board transitions may start documented rework. A direct rerun while a card remains in the same stage and out-of-order moves remain deferred and must not be inferred.
- Stopped work and rolled-back work move to Done only with a human Closure Record that names the reason, latest immutable artifact, decision, and follow-up if needed. A released work item moves to Done only after its Observation Record and human closure decision are complete.
- Production Observation compares actual signals to the accepted release and observation plan. A defect, incident, unmet outcome, or new idea becomes a distinct follow-up card and intent; it does not silently mutate completed work.

## Artifact handoff

Before a card moves from Spec & Design to Execution, the acceptance record binds both `spec.md` and `work-contract.yaml` by immutable repository reference, path, and content hash. The acceptance record, rather than either file referring to its own commit, establishes the frozen pair and makes the execution input unambiguous.

The ten lifecycle names in this document are the canonical Trello workflow vocabulary. Do not create an additional board stage until real work has established its purpose, ownership, and exit condition.
