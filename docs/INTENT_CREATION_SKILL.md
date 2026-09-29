# Intent Creation Skill

The Intent Creation Skill turns a product idea into a clear, versioned intent and keeps the human-facing Trello card and durable `intent.md` in agreement. This document defines the outcomes and rules a person or agent must meet when building the skill. It does not prescribe a repository for the skill, a runtime, tools, APIs, or file layout.

## Why it exists

Agentic execution makes implementation cheap; human attention and clear decisions become the scarce resources. Work that enters engineering with a vague or shifting intent wastes both. The skill ensures that, when work is handed off, the intent is clear, reviewable, and stored as an exact revision that later stages can trust.

## Where it operates

A product owner uses the skill to create and refine intent. The skill records and clarifies the owner's decisions; it does not make consequential product decisions.

The skill may work only while a card is in `New Ideas` or `Backlog`. Refinement happens within those columns, not in a separate stage. A card in any other column is outside the skill's write authority: it must leave the card and intent unchanged and inform the user that handoff has already occurred.

From Backlog, a product owner may explicitly select a handoff destination. The skill freezes the synchronized revision and places the card in that selected destination. It does not need to understand, validate, or infer the destination's name, position, or purpose.

## What an intent must capture

- The product outcome to achieve, stated so a reader can later tell whether it was delivered.
- Scope: what is included and deliberately excluded.
- Acceptance criteria a later stage can check.
- Constraints and assumptions the work must respect.
- The one product the intent belongs to.
- The product Policy & Compliance Profile ID and version selected by the product owner.
- A stable, product-prefixed intent ID that is never changed or reused.
- A version that increases with every accepted revision.

The Intent Creation Skill's canonical schema owns the exact format and field names. This document does not restate or alter that schema: a builder must use the canonical schema so people, Trello synchronization, and later stages identify the same revision consistently.

## Required outcomes

1. Every captured idea has a card on its product board and a matching durable `intent.md` under that product's intent backlog.
2. The card and intent describe the same intent at the same version. Each accepted revision is durable, retrievable, and identified by its product, intent ID, version, commit, and fingerprint.
3. History is never rewritten. Revising an intent creates a new version without losing earlier revisions.
4. If the card and intent disagree, the conflict is surfaced to the product owner and reconciled as a new version. Neither side is silently overwritten.
5. Ambiguity the skill cannot resolve is escalated to the product owner rather than filled in with a guess.
6. An idea spanning multiple products creates a parent intent plus one product-specific intent per affected product. One intent does not span unrelated product repositories.
7. Before handoff, the intent is synchronized, acceptance criteria are settled, and the current revision is durable and reachable so the freeze can identify it exactly.
8. Handoff begins only when a product owner explicitly selects a destination for a Backlog card. It leaves a frozen revision and places the card in that selected destination without interpreting the downstream workflow.
9. After handoff, a material change creates a new successor intent that follows the normal flow. The frozen intent remains untouched.
10. The skill records the policy-profile reference selected by the product owner. It may surface a defined escalation condition, but it must not decide legal applicability or claim compliance.

## Out of scope

- Changing a frozen intent revision.
- Creating engineering branches, worktrees, specifications, plans, or implementation artifacts.
- Inferring, validating, or managing downstream board stages after handoff. That belongs to the Conductor.

## Done when

- Creating an idea yields a product-prefixed ID, first intent version, matching card, and durable intent revision.
- Revising an intent yields a new version while keeping the prior revision retrievable.
- A card and its intent can be verified as the same revision using the recorded fingerprint.
- A card/intent conflict is reported rather than silently overwritten.
- Handoff occurs only after an explicit product-owner destination selection, produces a frozen revision, and places the card in the selected destination without interpreting it.
- Attempts to change a frozen intent, or any card outside `New Ideas` and `Backlog`, are refused without changing either artifact.
- A multi-product idea results in one intent per product.
- The frozen intent identifies the product policy-profile version used by Spec & Design and Execution.

## Open decision

- Whether a card may move from `Backlog` back to `New Ideas` for additional refinement, and how that should be shown.
