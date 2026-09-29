# Cross-Component Contract Decisions

## Intent content

This repository describes what an intent must capture and what the Intent Creation Skill must achieve, in [Intent Creation Skill](INTENT_CREATION_SKILL.md). It does not depend on, link to, or defer to any implementation of the skill. The layout of `intent.md` is the implementer's choice, and no workflow control depends on it.

## Freeze integrity

The freeze tuple records a single intent fingerprint: the SHA-256 of the exact bytes of `intent.md` at the frozen commit, as defined in [Artifacts](ARTIFACTS.md). Anyone can recompute it, and every stage verifies the frozen intent against it.

## Lifecycle vocabulary

The canonical workflow stages are defined in [Workflow](WORKFLOW.md): `New Ideas`, `Backlog`, `Prioritized`, `Spec & Design`, and `Execution`. New stages are not implied by an agent capability or a possible future delivery concern; add one only after real work establishes its purpose, owner, and exit condition.

`Prioritized` is the product-to-engineering commitment and intent-freeze boundary. The exact freeze protocol and material-change rule are defined in [Workflow](WORKFLOW.md) and [Artifacts](ARTIFACTS.md).

Product refinement is work performed within `New Ideas` and `Backlog`; it is not a separate Trello list. The versioned [Trello board template](../templates/trello-board-workflow.yaml) is the canonical list order used for board provisioning and validation.

## Publication

Required lifecycle artifacts must be durable and reachable before the next defined stage begins. The authorized workflow coordination path records their immutable provenance and reachable reference. See [Artifacts](ARTIFACTS.md), [Workflow](WORKFLOW.md), and [Governance](GOVERNANCE.md).

## Scope boundary

This repository defines SDLC outcomes and controls, not agent or skill implementation. Component repositories own their runtime and deployment design while remaining compatible with these contracts. See [Architecture](ARCHITECTURE.md).
