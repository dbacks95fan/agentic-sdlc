# Cross-Component Contract Decisions

## Intent content

This repository describes what an intent must capture and what the Intent Creation Skill must achieve, in [Intent Creation Skill](INTENT_CREATION_SKILL.md). It does not depend on, link to, or defer to any implementation of the skill. The layout of `intent.md` is the implementer's choice, and no workflow control depends on it.

## Freeze integrity

The freeze tuple records the SHA-256 fingerprint of the exact bytes of `intent.md` at the frozen commit and the same source-commit/fingerprint pair for the selected policy profile, as defined in [Artifacts](ARTIFACTS.md). Anyone can recompute them, and every engineering stage verifies the frozen inputs against them.

## Lifecycle vocabulary

The canonical workflow stages are defined in [Workflow](WORKFLOW.md): `New Ideas`, `Backlog`, `Prioritized`, `Spec & Design`, `Execution`, `Evaluation`, `Human Approval`, `Release`, `Production Observation`, and `Done`. The stages are intentionally few: human review stays within `Spec & Design`, and the implementation plan stays in `spec.md`; neither needs a separate board column. New stages are not implied by an agent capability or a possible future delivery concern; add one only after real work establishes its purpose, owner, and exit condition.

`Prioritized` is the product-to-engineering commitment and intent-freeze boundary. The exact freeze protocol and material-change rule are defined in [Workflow](WORKFLOW.md) and [Artifacts](ARTIFACTS.md).

Product refinement is work performed within `New Ideas` and `Backlog`; it is not a separate Trello list. The versioned [Trello board template](../templates/trello-board-workflow.yaml) is the canonical list order used for board provisioning and validation.

## Publication

Required lifecycle artifacts must be durable and reachable before the next defined stage begins. The Conductor, acting under authorized policy, records their immutable provenance and reachable reference. See [Artifacts](ARTIFACTS.md), [Workflow](WORKFLOW.md), and [Governance](GOVERNANCE.md).

## Specification and execution contract

`spec.md` is the single human-reviewed Markdown artifact. It contains its own implementation and validation plan; a separate `plan.md` is not part of this lifecycle. The proposed `work-contract.yaml` is a machine-readable execution record created alongside the spec and frozen only after human acceptance. Its required lineage is defined in [Stage Contracts](STAGE_CONTRACTS.md).

## Candidate, evaluation, and delivery

Execution creates a candidate commit and Evidence Package; neither is an approval. A separate Evaluator produces an Evaluation Package against the frozen inputs. Only an accountable human may approve the named candidate for integration and Release. The authorized integration path records the resulting protected-branch SHA before release. Release and Production Observation create durable fact records tied to the candidate and integration SHA, and Done requires a human closure decision. Evaluation failure returns to Execution with findings; a non-material specification defect returns to Spec & Design; a material product change creates a successor intent instead.

## Policy controls

Each frozen intent names a versioned product Policy & Compliance Profile by ID, version, source commit, and fingerprint. Its applicable controls and escalation conditions travel through the specification, work contract, and Evidence Package. The profile makes a consideration visible; it does not permit an agent to make a legal or compliance conclusion.

## Scope boundary

This repository defines SDLC outcomes and controls, not agent or skill implementation. Component repositories own their runtime and deployment design while remaining compatible with these contracts. See [Architecture](ARCHITECTURE.md).
