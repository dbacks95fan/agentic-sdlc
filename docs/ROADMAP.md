# Implementation Roadmap

**Audience:** people sequencing adoption and component implementation. This roadmap defines phases and completion evidence; it is not a live status report or a duplicate of the operating contracts. Follow [Stage Contracts](STAGE_CONTRACTS.md) for exact component requirements and [Workflow](WORKFLOW.md) for allowed transitions.

Implement one coherent stage at a time in its owning repository. Update and review the canonical SDLC contract before changing a component contract or implementation. Never infer an implementation's current state from this roadmap; verify the component and its handoffs directly.

## Phase 0 - Validate the canonical contract

Confirm each lifecycle role has a trigger, authority, inputs, required actions, outputs, error/status behavior, schema, and conformance checks. Confirm every shared record has an explicit version and every product entry validates against `ProductConfig`. Verify Board Setup can create a greenfield board from the canonical template or bind an existing board with an explicit and valid stage-to-list-ID map. Ensure product constitution and architecture baselines are consulted during Spec & Design.

**Complete when:** a builder can implement a component from this repository without guessing required behavior or assuming a particular runtime, and valid and invalid handoffs can be checked against the published contracts.

The board setup contract and `ProductConfig` 1.1 define board creation and binding. Existing product records and Conductor consumers must be migrated to this contract before a Conductor implementation that requires the explicit list-ID map is enabled.

## Phase 1 - Intent and commitment

Verify new intent creation, versioned revisions, multi-product separation, policy-profile selection, conflict handling, Prioritized freeze, and freeze revocation on return to Backlog or New Ideas.

**Complete when:** these cases preserve synchronized, reachable artifacts and inspectable, hash-verifiable freeze history.

## Phase 2 - Spec & Design and acceptance

Verify a valid Spec & Design entry checks the active freeze, provides the exact inputs and isolated workspace, consults product baselines, and produces `spec.md` plus a schema-valid proposed Work Contract v3. Verify human acceptance records immutable references and hashes before Execution.

**Complete when:** the acceptance record, accepted artifacts, frozen intent/profile, and target revision agree exactly.

## Phase 3 - Execution and independent evaluation

Verify components follow the accepted Work Contract v3 and its validation requirements, Coding stays within assigned scope, and the separate Evaluator assesses the same candidate and evidence with read-only access to candidate code. The exact compatibility and artifact rules are in [Artifacts](ARTIFACTS.md) and [Stage Contracts](STAGE_CONTRACTS.md).

**Complete when:** malformed, missing, or mismatched records are rejected, and candidate and evaluation evidence identify the same immutable SHA.

## Phase 4 - Human decision, release, and observation

Verify the accountable human's decision names the candidate, the authorized release path records integration and release facts, required health checks are observed, and Production Observation records the agreed period and signals.

**Complete when:** one low-risk feature has a traceable chain from intent through observation and human closure, or a deliberate stop/rollback with a Closure Record.

## Deferred exception flows

Direct same-stage reruns and out-of-order board moves are not defined for the initial path. Specify their trigger, reconciliation, authorization, audit record, and human recovery before implementing them. The Conductor's bounded retry for a known idempotent, transient operation is separate and is defined in [Governance](GOVERNANCE.md). Do not add a board column merely to accommodate an exception.

## Before production autonomy

Resolve product-specific webhook authentication, secret storage, retention, workspace lifecycle, access control, rollback, observation-window, and operational-signal requirements before enabling autonomous production actions. A learning feature may use the documented human gates and a manually authorized release path while these broader operating policies remain explicit.
