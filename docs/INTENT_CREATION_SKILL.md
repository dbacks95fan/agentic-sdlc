# Intent Creation Skill

The Intent Creation Skill creates and refines `intent.md`, commits and pushes accepted revisions to the dedicated GitHub `intent-backlog` repository, and synchronizes Trello with the exact revision reference and fingerprint. This document describes required outcomes, not how the skill is built or how `intent.md` is laid out.

## Why it exists

Agentic execution makes implementation cheap; human attention and clear decisions become the scarce resources. Work that enters engineering with a vague or shifting intent wastes both. The skill exists so that, when a product owner commits engineering capacity at `Prioritized`, the intent is clear, reviewable, and stored as an exact revision that later stages can trust without asking again.

## Who uses it and where

A product owner works with the skill to create and refine intent. The skill helps the owner think the idea through and records the result; the owner makes the decisions (see [Governance](GOVERNANCE.md)).

The skill works only while a card is in `New Ideas` or `Backlog`. Refinement is work done within those columns, not a separate stage. If a card is in any other column, the skill leaves the card and intent unchanged and tells the user that the intent has been handed off.

From Backlog, a product owner may explicitly select a handoff destination. The skill synchronizes the revision and submits the handoff request to the control plane; it does not need to understand or validate the destination's name, position, or purpose. The Conductor validates and applies the board transition. If the card returns to Backlog before Spec & Design starts, the Conductor revokes the active freeze and records that state; the skill may then refine the same intent as a new version. Once Spec & Design starts, the freeze cannot be lifted for that work item, and material changes require a successor intent.

## What an intent must capture

- The product outcome to achieve, stated so a reader can tell afterwards whether it was delivered.
- Scope: what is included and what is deliberately left out.
- Acceptance criteria that a later stage can check.
- Constraints the work must respect.
- Assumptions the intent depends on.
- The one product it belongs to.
- The product Policy & Compliance Profile ID and version selected by the product owner.
- A stable identifier that includes the product, such as `INT-MF-0042`, which is never changed or reused.
- A version that increases with every accepted revision.

How these are laid out in `intent.md`, and what they are called, is the implementer's choice, as long as a person can read the file and the Spec & Design stage can work from it. No workflow control depends on the layout.

The file must not contain values that are only known after it is saved, such as the commit that stores it. Those values are recorded on the card and in the freeze tuple, so the file's fingerprint stays stable (see [Artifacts](ARTIFACTS.md)).

## Outcomes the skill must deliver

1. Every idea it captures has a card on the product's board and a matching `intent.md` in the `intent-backlog` repository, under the right product.
2. The card and `intent.md` always describe the same intent at the same version. Each accepted revision is committed to `intent-backlog`, and the card shows the product, intent ID, version, commit, and fingerprint of that revision.
3. Every revision stays retrievable; history is never rewritten.
4. When the card and the file disagree, the conflict is surfaced, reconciled with the product owner, and recorded as a new version. Neither side is silently overwritten.
5. Ambiguity the skill cannot resolve is raised with the owner, not filled in with a guess.
6. An idea that spans several products becomes a parent intent with one intent per product; no single intent spans unrelated product repositories.
7. Before handoff, the intent is synchronized, its acceptance criteria are settled, and its current revision is committed and reachable, so the freeze can record it exactly.
8. A handoff begins only when a product owner explicitly selects a destination for a Backlog card. It leaves a frozen revision and asks the Conductor to place the card in that selected destination without interpreting the downstream workflow.
9. If a card returns to Backlog before Spec & Design starts and the Conductor has recorded the freeze as revoked, the skill may revise the same intent by creating a new version and commit. After Spec & Design starts, a material change becomes a new successor intent that goes through the normal flow; the frozen intent is left untouched.
10. The skill records the policy-profile reference selected by the product owner. It may surface a defined escalation condition, but it must not decide legal applicability or claim compliance.

## What the skill does not do

- Change an intent revision once it is frozen.
- Create branches, worktrees, specifications, or any other engineering artifact.
- Infer, validate, or manage downstream board stages after handoff; that belongs to the Conductor (see [Agent roles](AGENT_ROLES.md)).

## Done when

An implementation is complete when all of these can be shown:

- Capturing a new idea produces a card and an `intent.md` with a product-prefixed ID and a first version, and the card shows the commit and fingerprint of that revision.
- Revising the intent produces a new version and a new commit, the previous revision is still retrievable, and the card shows the new values.
- For any card, recomputing the SHA-256 of `intent.md` at the commit shown on the card gives the fingerprint shown on the card.
- Editing the card and the file in different ways produces a reported conflict, not a silent overwrite.
- A handoff from Backlog happens only after a product owner explicitly selects a destination, leaves a frozen revision, and asks the Conductor to place the card in that destination without interpreting its meaning.
- Returning to Backlog before Spec & Design starts lifts the active freeze only after the Conductor records revocation; a new revision is committed without rewriting the old commit.
- An attempt to change a frozen revision after Spec & Design starts is refused and leaves both the file and the card unchanged.
- An attempt to change a card outside New Ideas or Backlog is refused and tells the user that the intent has been handed off.
- An idea spanning two products results in one intent per product.
- The frozen intent identifies the policy-profile version used by Spec & Design and Execution.

## Open decisions

- Whether a card may move back from `Backlog` to `New Ideas` for more refinement, and how that move is shown on the card.
