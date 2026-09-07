# Workflow

## Product flow

```text
New Ideas -> Product Refinement -> Backlog -> Prioritized
                                                    |
                                                 freeze
                                                              v
Spec & Design -> Design Review -> Implementation Planning -> Ready for Build
                                                              |
                                                              v
Coding -> Evaluation -> Human Approval -> Ready for Release -> Release
                                                              |
                                                              v
                                              Production Observation -> Done
```

| State or stage | Primary action | Exit condition |
| --- | --- | --- |
| New Ideas | Capture an idea with product context | A product owner begins refinement |
| Product Refinement | Intent Creation Skill reconciles card and `intent.md`; version intent | Outcome, criteria, constraints, and open questions are reviewable |
| Backlog | Hold a valid, non-committed intent | Priority decision |
| Prioritized | Commit engineering capacity and capture the full freeze tuple | Exact frozen intent is available to Spec & Design |
| Spec & Design | Spec & Design Agent creates `spec.md` | Design Review approves or returns it |
| Design Review | Human reviews the specification and design | Approved spec or documented return for revision |
| Implementation Planning | Planning Agent creates `plan.md` and work contract | Plan is approved and executable |
| Ready for Build | Conductor verifies approved plan and work contract | Coding Agent is assigned bounded implementation work |
| Coding | Coding Agent implements only the contracted scope | Required deterministic checks and evidence complete |
| Evaluation | Evaluator independently assesses alignment and evidence | Decision brief: pass, needs work, blocked, or system/input error |
| Human Approval | Authorized person reviews consequential result | Approval to release or return path |
| Ready for Release | Conductor verifies candidate, evaluator disposition, and approval | Release process is authorized to act |
| Release | Standard delivery process deploys the approved change | Health and release evidence verified |
| Production Observation | Monitor outcomes and incidents | Feedback becomes a new or refined pre-freeze intent |
| Done | Delivery lifecycle is complete | Retain evidence; production observation remains ongoing |

## Prioritized freeze protocol

1. Confirm card/intent identity and synchronization.
2. Resolve material ambiguity and acceptance criteria before commitment.
3. Record intent ID, product ID, intent version, intent commit SHA, normalized content hash, frozen-artifact hash, card ID, freeze time, and acceptance metadata.
4. Mark the revision immutable and establish the assigned product-repository branch/worktree.
5. Copy the frozen bytes into `.agent/work/<intent-id>/intent.md`.

If a material change is requested after step 4, create a successor intent and route it through normal product flow. A bug or omission in the current contracted scope may be handled in the current work item only when the approved authority classifies it as non-material and records that decision.

## Artifact publication gates

Before Design Review begins, the workflow records the durable `spec.md` artifact, its immutable commit SHA, repository-relative path, and a reachable review reference. Before independent Evaluation begins, it records equivalent provenance for the implementation candidate and validation outputs. This lets a reviewer or evaluator inspect the same immutable inputs that produced the stage outcome.

Publication makes an artifact available for its next lifecycle gate. It is not design approval, evaluation approval, or release authorization.

## Evaluation outcomes

An evaluator result is durable, structured, and human-readable. It must distinguish verified facts from inference and never report a pass unless every applicable contract criterion is satisfied. The Conductor uses that result to propose or perform only pre-authorized workflow transitions; the evaluator itself remains read-only.

The lifecycle names in this document are the canonical Trello workflow
vocabulary. `Agent Working` and `Agent Review` are retired generic names; use
the specialized `Coding` and `Evaluation` stages instead.
