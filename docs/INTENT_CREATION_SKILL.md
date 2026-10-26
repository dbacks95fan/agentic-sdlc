# Intent Creation Skill

The Intent Creation Skill creates the initial Trello card in `New Ideas` and synchronizes it with the canonical, versioned `intent.md` in the dedicated GitHub `intent-backlog` repository. It records the selected policy-profile ID and version with the card and intent. The product owner moves the card through later columns; the Conductor alone records workflow transitions and freeze state. This document describes required outcomes, not how the skill is built or how `intent.md` is laid out.

## Why it exists

Agentic execution makes implementation cheap; human attention and clear decisions become the scarce resources. Work that enters engineering with a vague or shifting intent wastes both. The skill exists so that, when a product owner commits engineering capacity at `Prioritized`, the intent is clear, reviewable, and stored as an exact revision that later stages can trust without asking again.

## Who uses it and where

A product owner works with the skill to create and refine intent. The skill helps the owner think the idea through and records the result; the owner makes the decisions (see [Governance](GOVERNANCE.md)).

The Skill creates cards in `New Ideas`. After creation, the product owner and Conductor own all board transitions. The Skill does not move a card to `Backlog` or any downstream column and does not write or interpret freeze state.

The product owner moves a synchronized card from `Backlog` to `Prioritized`. The Conductor validates the card and its pinned artifacts, then records or revokes the freeze as the card moves. The Skill has no role in that transition.

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
8. The Skill creates the initial card in `New Ideas` and synchronizes its intent and selected policy-profile reference. A product owner, not the Skill, moves the card through later columns.
9. The Skill does not set, clear, or interpret freeze state. The Conductor records activation or revocation and preserves prior freeze history.
10. The skill records the policy-profile reference selected by the product owner. It may surface a defined escalation condition, but it must not decide legal applicability or claim compliance.

The Trello projection includes `Policy Profile ID` and `Policy Profile Version`. The Skill does not calculate or record the policy-profile source commit or fingerprint; the Conductor resolves those from the pinned intent-backlog revision when freezing the card.

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
- Creating an idea creates a Trello card in `New Ideas` and a synchronized, versioned intent record.
- The card's selected profile ID/version resolves through the product's `product.yaml` to a profile file in the same pinned intent-backlog commit.
- Moving a card to `Prioritized` causes the Conductor, not the Skill, to resolve and record the complete freeze tuple.
- Returning a card to `Backlog` or `New Ideas` causes the Conductor, not the Skill, to record revocation while preserving prior history.
- An idea spanning two products results in one intent per product.
- The frozen intent identifies the policy-profile version used by Spec & Design and Execution.

## Open decisions

No workflow decision is currently open in this contract. Reruns and out-of-order moves are documented as deferred exceptions in [Roadmap](ROADMAP.md).
