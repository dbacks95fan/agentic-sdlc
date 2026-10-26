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
| New Ideas | Capture an idea and refine its product context and intent. | A coherent intent is ready for future consideration. |
| Backlog | Hold a valid intent that may be refined. A card returned from Prioritized is editable again after the Conductor revokes its active freeze. | Product owner authorizes its handoff to Prioritized. |
| Prioritized | Commit engineering capacity and record the freeze tuple. | Frozen intent and policy-profile references are valid; the control plane queues Spec & Design. A return to Backlog or New Ideas revokes the active freeze. |
| Spec & Design | Produce and review `spec.md` and the proposed work contract. | Owner accepts both artifacts; the acceptance record freezes their revisions and authorizes Execution. |
| Execution | Build the candidate from accepted durable inputs. | Candidate commit and Evidence Package are ready for independent evaluation. |
| Evaluation | Independently compare candidate and evidence to frozen inputs. | Pass goes to Human Approval; implementation findings return to Execution; specification findings return to Spec & Design. |
| Human Approval | Make the accountable integration and release decision. | Approve integration/release, request bounded rework, or stop the work. |
| Release | Integrate the approved candidate and deliver the approved integration revision. | Release Record has integration, delivery, and health-check facts. |
| Production Observation | Observe the agreed outcome and operational signals. | Observation Record and human closure decision are complete. |
| Done | Close delivered or deliberately stopped work. | Human closure record explains the outcome and any follow-up. |

The required stage inputs, outputs, decision gates, and agent statuses are defined in [Stage Contracts](STAGE_CONTRACTS.md). Entry into `Spec & Design` triggers the Spec & Design stage: the Conductor validates the frozen inputs, prepares the isolated workspace handoff, and invokes the Spec & Design Agent. Entry into `Execution` and `Evaluation` triggers their respective agents under the same stage contracts. `Human Approval` is always a human decision. The Conductor is the sole control-plane identity that writes Trello workflow state; a human card move is a transition request that it validates and records.

## Intent Creation boundary

The Intent Creation Skill creates the initial card in `New Ideas` and records the canonical intent revision and selected policy-profile ID/version. It has no authority over later board transitions or freeze state. The Conductor owns workflow metadata, including activation and revocation of freezes. Trello card projections include `Policy Profile ID`, `Policy Profile Version`, and a `Freeze State` line managed only by the Conductor.

The product owner moves the synchronized card from Backlog to `Prioritized`. The Conductor validates the handoff and records the freeze; the Intent Creation Skill does not select or perform the downstream transition.

## Prioritized freeze protocol

1. The product owner authorizes the Backlog-to-Prioritized transition.
2. The Conductor confirms card/intent identity, synchronization, material clarity, acceptance criteria, and agreement between the card and intent on policy-profile ID/version. It resolves the profile path from the product's `product.yaml` at the pinned intent commit and reads the profile from that same commit.
3. The Conductor records one freeze tuple in a visible Trello comment: product ID, intent ID, intent version, intent repository commit SHA, exact-byte intent fingerprint, policy-profile ID, policy-profile version, policy-profile repository commit SHA, policy-profile fingerprint, Trello card ID, authorizing owner, and freeze time. It updates the card's current Freeze State projection to Active.
4. A successful Prioritized freeze makes the intent and selected policy profile immutable engineering inputs. If required freeze inputs are missing or disagree, the Conductor returns the card to Backlog with a visible explanation and does not create an active freeze. If the owner returns a frozen card to Backlog or New Ideas, the Conductor updates the current Freeze State projection to Revoked and appends a visible revocation event; the prior freeze comment and Git revision remain in history. A later move to Prioritized creates a new freeze tuple for the newly synchronized revision. `Acceptance metadata` means the authorizing owner, time, and recorded decision; it does not include an unspecified second approval.
5. When `Spec & Design` starts, the first engineering worker creates the isolated branch/worktree from the assigned base revision, obtains exact byte-preserving copies of the frozen intent and policy profile, verifies their fingerprints, and reports the workspace and artifact references to the Conductor.

After Prioritized, a material change in outcome, scope, acceptance criteria, constraints, or assumptions may be incorporated as a new version only after the card returns to Backlog or New Ideas and the Conductor revokes the freeze. This applies even if Spec & Design has started; preserve the previous Git revision and record the revocation. A non-material specification correction may return to `Spec & Design` under the rework rules below.

## Delivery and rework rules

- Evaluation `pass` moves to `Human Approval`. Evaluation `fail` returns to `Execution` with durable findings as inputs. A finding that the accepted specification is incomplete or inconsistent returns to `Spec & Design`; the frozen intent remains unchanged.
- A human may approve the candidate for integration and Release, return it to Execution with bounded feedback, return it to Spec & Design for a non-material correction, or stop it. A material product change creates a successor intent instead of reworking the frozen intent.
- After Human Approval, the authorized integration path merges the approved candidate into the product's protected integration branch, normally `main`, and records the resulting immutable integration SHA. Required integration checks must pass before that revision is released. A merge conflict or failed integration check returns the work to Execution or blocks it for human resolution.
- Release records the candidate SHA, integration SHA, environment, verification and health-check facts, and any rollback. A release command completing is not proof that the product was delivered correctly.
- `blocked` and `failed` statuses keep the card in its current column until an authorized human chooses retry, rework, stop, or another documented exception. Automatic retry is limited to an idempotent, clearly transient failure with no unknown side effect.
- Stopped work and rolled-back work move to Done only with a human Closure Record that names the reason, latest immutable artifact, decision, and follow-up if needed. A released work item moves to Done only after its Observation Record and human closure decision are complete.
- Production Observation compares actual signals to the accepted release and observation plan. A defect, incident, unmet outcome, or new idea becomes a distinct follow-up card and intent; it does not silently mutate completed work.

## Artifact handoff

Before a card moves from Spec & Design to Execution, the acceptance record binds both `spec.md` and `work-contract.yaml` by immutable repository reference, path, and content hash. The acceptance record, rather than either file referring to its own commit, establishes the frozen pair and makes the execution input unambiguous.

The ten lifecycle names in this document are the canonical Trello workflow vocabulary. Do not create an additional board stage until real work has established its purpose, ownership, and exit condition.
