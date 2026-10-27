# Implementation Roadmap

This roadmap defines the order and completion evidence for implementing the documented lifecycle. It is not a live status report for component repositories, Trello, or deployments. Recheck those systems when starting implementation work; never infer current implementation status from this document.

Implement one coherent stage at a time on a feature branch in the owning repository. Update and review the canonical SDLC contract before changing a component contract or implementation. A component is complete only when its behavior and handoffs pass the conformance requirements in [Stage Contracts](STAGE_CONTRACTS.md).

## Phase 0 — Complete and adopt the canonical contract

- Keep the lifecycle, decision rights, artifact ownership, agent responsibilities, stage results, exception boundaries, and schemas consistent across the canonical documents.
- Define and validate the machine-readable schemas for shared invocation, result, freeze, acceptance, evidence, evaluation, human decision, release, observation, and closure records.
- Confirm the versioning and compatibility rule for every shared record. A consumer rejects an unsupported version rather than silently converting it.
- Validate each product entry against `ProductConfig`; record the product/intent identity invariants, authorized product-owner identities, canonical Trello stage mapping, and versioned policy-profile references.
- Maintain one product-level constitution and architecture per product; Spec & Design consults them when producing a feature specification.

**Complete when:** a builder can identify a component's trigger, authority, inputs, required actions, outputs, errors, schema, and conformance checks from this repository without assuming a runtime or deployment model.

## Phase 1 — Intent intake and Prioritized commitment

- Verify the Intent Creation Skill creates only the initial `New Ideas` card and synchronizes a versioned canonical `intent.md` to `intent-backlog`.
- Verify product, intent, and version identity; profile selection; conflict handling; history preservation; and multi-product decomposition.
- Verify the Conductor validates the Backlog-to-Prioritized transition, pins the exact intent and policy-profile revisions, records the freeze tuple, and records revocation when the card returns to Backlog or New Ideas.
- Verify Trello projections show the recorded identity, revision, fingerprints, and freeze state, with the Conductor as the sole workflow-state writer.

**Complete when:** controlled new, revision, prioritization, and revocation cases produce synchronized artifacts and inspectable, hash-verifiable records.

## Phase 2 — Spec & Design and human acceptance

- Verify a valid `Spec & Design` entry causes the Conductor to validate the freeze and invoke the Spec & Design Agent with exact inputs and isolated workspace permissions.
- Verify the agent consults the product constitution and architecture, produces the required `spec.md` sections and a schema-valid proposed `work-contract.yaml`, and returns only a defined status.
- Verify the product owner can accept both artifacts and that the Conductor records a Spec Acceptance Record binding their immutable references and hashes before Execution.

**Complete when:** a human-accepted spec and work contract are reachable and their references exactly match the acceptance record and frozen intent/profile.

## Phase 3 — Execution and independent evaluation

- Migrate the Conductor, Coding Agent, and Evaluator from any legacy Work Contract shape to canonical Work Contract v3; the accepted `spec.md` contains the implementation/validation plan, and no separate `plan.md` is used. A v2-only component is not conformant and must fail closed rather than reinterpret fields. The current Coding Agent documentation still describes v2 and a proposed v3 containing `plan.md`; that proposal must be replaced with this canonical contract before the adapter is integrated.
- Align the Conductor's Coding Agent adapter with the accepted Spec Acceptance Record and `work-contract.yaml`; it must not derive scope or requirements from Trello text or card-derived legacy fields.
- Verify the Coding Agent validates inputs, stays within assigned scope, runs repository-declared deterministic checks, creates a candidate commit, and returns a schema-valid Evidence Package.
- Verify the independent Evaluator receives the same frozen inputs and candidate, has read-only access to code, reproduces applicable checks, and returns a schema-valid Evaluation Package.
- Verify only the documented routes apply: Evaluation `pass` to Human Approval; implementation `fail` to Execution; a non-material specification finding to Spec & Design; unresolved or invalid states remain blocked for human resolution.

**Complete when:** candidate and evaluation evidence refer to the same immutable SHA and the Conductor rejects malformed, missing, or mismatched records.

## Phase 4 — Human approval, release, observation, and closure

- Verify human approval names the immutable candidate SHA and records an allowed decision.
- Verify only the authorized integration/release path merges the approved candidate, records the integration SHA, performs required checks, records delivery and rollback facts, and verifies the running service.
- Verify Production Observation records the agreed time window, signals, observed outcome, deviations, and follow-up references.
- Verify a human closes released work only after observation, and closes stopped or rolled-back work only with a Closure Record.

**Complete when:** one low-risk product feature has a traceable chain from intent through a human closure decision, with observed release and outcome evidence or a deliberate stop record.

## Deferred exception flows

The initial implementation supports the documented in-order path and the two specified rework routes: implementation findings return to Execution, and human-approved non-material specification corrections return to Spec & Design. A direct rerun request while a card remains in the same stage, and out-of-order or unexpected board moves, require a separate documented contract before implementation. Define their trigger, state reconciliation, authorization, audit record, and human recovery path first. The bounded transient-operation retry rule in [Governance](GOVERNANCE.md) is separate. Do not add a board column merely to accommodate exceptions.

## Before production autonomy

Resolve product-specific webhook authentication, secret storage, retention, workspace lifecycle, access control, rollback, observation-window, and operational-signal requirements before enabling autonomous production actions. The documented human decision gates and allowed Conductor transitions are canonical; runtime credentials and permissions are configured in the owning implementation. A learning feature may proceed with the approved human gates and a manually authorized release path while these broader operating policies remain explicit.
