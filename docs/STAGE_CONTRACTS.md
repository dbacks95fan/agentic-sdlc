# Stage Contracts

This document defines the complete, minimal path from a product idea to a closed delivery record. It is the shared contract for the Intent Creation Skill, Conductor, Spec & Design Agent, Coding Agent, Evaluator, and accountable human. Components may choose their own runtime and implementation, but they must satisfy these inputs, outputs, rules, and status meanings.

```text
New Ideas / Backlog -> Prioritized -> Spec & Design -> Execution -> Evaluation
intent.md             frozen intent    spec.md              candidate code    independent assessment
                                         work-contract.yaml   + evidence

Evaluation -> Human Approval -> Release -> Production Observation -> Done
              accountable decision    release record   observation record     closure record
```

The board has no Planning column. `spec.md` is the one human-reviewed Markdown document. Its Implementation and Validation section contains the implementation plan. `work-contract.yaml` is a frozen, machine-readable execution record; it is not a second human-authored plan.

## Shared rules

- The control plane owns workflow state, intent synchronization, handoff validation, routing, and human decision records. The execution plane owns only its assigned engineering artifacts and evidence.
- Each plane may use a separately configured LLM. Provider/model/version does not change role permissions, human authority, or the required status and artifact contracts.
- Each durable input is identified by immutable revision, path, and hash where applicable.
- A worker validates every input it receives before acting. A missing, mutable, or mismatched input returns `blocked` and does not advance the card.
- A worker returns `needs_decision` for a material ambiguity. It does not invent a product, security, compliance, cost, scope, or external-contract decision.
- The Conductor alone reads or updates Trello workflow state. Agents do not move cards or grant approvals.
- An assigned worker may commit only its authorized artifacts on its assigned work branch. The Conductor publishes a validated branch when the next human or worker needs a reachable reference; publication is neither approval nor merge.
- The product owner decides intent, priority, policy applicability, and whether to accept `spec.md` and the proposed work contract. Tests and agent evidence inform that decision; they do not replace it.
- A candidate is an immutable commit plus evidence. It is not accepted, released, or compliant merely because an agent says so or a check passes.
- An independent Evaluator may inspect and report; it must not alter candidate code, frozen inputs, approvals, or workflow state.
- A release or observation process records facts and requests a human decision when an approved control or outcome cannot be established. It does not infer approval.

## 1. Intent Creation

**Board trigger:** a product owner works in `New Ideas` or `Backlog`.

**Inputs:** product context, a new or existing card, the current intent revision when one exists, and the product's Policy & Compliance Profile reference.

**Outputs:** a versioned canonical `intent.md`, synchronized Trello projection, stable intent ID, and the selected policy-profile ID and version. A conflict is reported rather than overwritten.

**Rules:** the skill may edit only `New Ideas` and `Backlog`. It must not decide that a framework applies, claim compliance, or edit an intent after handoff.

## 2. Prioritized Freeze

**Board trigger:** the authorized product owner explicitly hands off a Backlog card to `Prioritized`.

**Inputs:** synchronized intent revision, card identity, policy-profile reference, and product/intent identity.

**Outputs:** a freeze tuple containing the intent revision, exact-byte intent fingerprint, card ID, policy-profile ID and version, and freeze time. The frozen intent and profile are immutable inputs to engineering.

**Rules:** a failed synchronization or missing policy profile blocks the transition. A later material change creates a successor intent and new lifecycle.

## 3. Spec & Design

**Board trigger:** a card enters `Spec & Design`. The Conductor validates the frozen inputs, prepares the assigned isolated workspace, and invokes the Spec & Design Agent.

**Inputs:** frozen intent, frozen policy profile, target repository/base revision, assigned workspace, and repository-local instructions.

**Outputs:**

- `spec.md`, the sole human-reviewed Markdown artifact.
- A proposed `work-contract.yaml` that references the frozen intent, policy profile, `spec.md`, target repository/base revision, permitted scope, and required validation.
- A structured result: `spec_ready`, `needs_decision`, `blocked`, or `failed`.

`spec.md` must contain these sections: Outcome and scope; acceptance criteria; confirmed repository facts; design; constraints and policy controls; implementation and validation plan; risks and open decisions; and traceability to frozen inputs.

**Rules:** the agent may inspect the target repository and write only assigned artifacts. It must not write product code, alter frozen inputs, alter Trello, approve the spec, or claim that a policy or regulation is satisfied.

**Human gate:** the product owner reviews `spec.md` and the proposed work contract while the card remains in `Spec & Design`. Acceptance freezes their revisions. The owner's move of the card to `Execution` is the authorization to invoke the Coding Agent.

## 4. Execution

**Board trigger:** an accepted card enters `Execution`. The Conductor verifies the approved `spec.md` and frozen work contract before invoking the Coding Agent.

**Inputs:** frozen intent, frozen policy profile, accepted `spec.md`, accepted `work-contract.yaml`, isolated product workspace, and repository-local instructions.

**Outputs:** candidate implementation committed on the assigned branch, immutable candidate commit SHA, and an Evidence Package containing changed artifacts, independently observed validation results, policy-control evidence, and status: `candidate_complete`, `needs_decision`, `blocked`, or `failed`.

**Rules:** the Coding Agent may change only the assigned workspace. It must not change the intent, policy profile, spec, work contract, Trello state, approvals, or release state. It must run required deterministic validation and report observed evidence, not self-certify product delivery or compliance.

## 5. Evaluation

**Board trigger:** a candidate enters `Evaluation`. The Conductor verifies that the candidate SHA, Evidence Package, and all frozen inputs are reachable and consistent before invoking the independent Evaluator.

**Inputs:** frozen intent, frozen policy profile, accepted `spec.md`, frozen work contract, candidate commit SHA, Evidence Package, and repository-local evaluation instructions.

**Outputs:** an Evaluation Package with the candidate SHA, checked artifacts, observed validation facts, findings, and one status: `pass`, `fail`, `needs_decision`, `blocked`, or `failed`.

**Rules:** the Evaluator is separate from the Coding Agent and has read-only authority over candidate code. It may reproduce deterministic checks or inspect the candidate, but it may not modify code, frozen inputs, policy decisions, approvals, Trello, or release state. It must distinguish observed facts from conclusions, never waive a required control, and never claim legal compliance.

**Routing:** `pass` advances to `Human Approval`; `fail` returns to `Execution` with durable, bounded findings; `needs_decision` and `blocked` remain visible for an authorized human resolution. A material request changes the lifecycle by creating a successor intent.

## 6. Human Approval

**Board trigger:** an Evaluation Package with `pass` enters `Human Approval`.

**Inputs:** the frozen inputs, candidate SHA, Evidence Package, Evaluation Package, unresolved decisions, and release requirements.

**Output:** a durable human decision identifying the candidate SHA and one outcome: `approved_for_release`, `return_to_execution`, or `stopped`.

**Rules:** this is an accountable human decision, not an agent action. Approval means the named candidate may enter the product's release process; it is not a claim that every production outcome is guaranteed. Rework preserves the freeze boundary. A material change creates a successor intent.

## 7. Release

**Board trigger:** an approved candidate enters `Release`.

**Inputs:** human approval tied to the immutable candidate SHA, applicable release controls, and the product's authorized release path.

**Outputs:** a Release Record containing the candidate SHA, target environment, time, release actions, required verification and health-check facts, rollback facts if used, and one status: `released`, `rolled_back`, `blocked`, or `failed`.

**Rules:** release is performed only through the product's authorized controls. A successful command is not sufficient evidence; the record must include the required observed verification. A release failure, rollback, or material incident is surfaced for human triage and does not move automatically to Done.

## 8. Production Observation

**Board trigger:** a Release Record with `released` enters `Production Observation`.

**Inputs:** release record, expected outcome and acceptance criteria, defined observation window, and product-appropriate operational signals.

**Outputs:** an Observation Record with the period observed, signals considered, observed outcome, incidents or deviations, follow-up references, and a recommendation to close or create follow-up work.

**Rules:** observation validates what happened after release; it does not retroactively rewrite the intent or candidate. A new request, defect, or material outcome gap is a new card and intent. The process may collect facts automatically, but a human retains the closure decision.

## 9. Done

**Board trigger:** the Observation Record is complete.

**Inputs:** Observation Record, linked follow-up work where needed, and the human closure decision.

**Output:** a closed work item with its full chain of durable references from intent through observation.

**Rules:** close only when the outcome is sufficiently understood and any necessary follow-up is captured. Do not use Done to hide unresolved risks, blocked release work, or an unreviewed observation.

## Policy & Compliance Profile

Every product has a versioned profile following [the template](../templates/policy-compliance-profile.md). An intent references the profile version selected by its product owner. A profile may say that no external framework is currently applicable, but that is an explicit, reviewable decision rather than missing information.

The profile supplies applicable control IDs, required agent actions, evidence expectations, and escalation conditions. A rule can require an agent to stop for human review when work introduces payment-card data, health data, financial-reporting controls, customer assurance commitments, or another defined trigger. It cannot authorize an agent to make a legal applicability or compliance conclusion.
