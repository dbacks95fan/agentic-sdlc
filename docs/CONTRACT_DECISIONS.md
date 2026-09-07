# Cross-Component Contract Decisions

## Canonical intent representation

The Intent Creation Skill owns the sole canonical `intent.md` schema at
`references/output-format.md`. Agent repositories and the Conductor reference
that contract rather than create local metadata variants. Legacy consumers must
migrate through an explicit compatibility plan; they may not silently parse a
different format as if it were canonical.

## Freeze integrity

The freeze tuple records both a normalized content hash and an exact-byte
engineering artifact hash, as defined in [Artifacts](ARTIFACTS.md). The two
values have different purposes and field names. A worker verifies the exact
bytes it receives; cross-system product synchronization uses the normalized
content hash.

## Lifecycle vocabulary

The canonical workflow stages are defined in [Workflow](WORKFLOW.md). Generic
labels `Agent Working` and `Agent Review` are retired in favor of `Coding` and
`Evaluation`. `Ready for Build` and `Ready for Release` are explicit Conductor
gates; `Production Observation` continues after the card reaches `Done`.

`Prioritized` is the product-to-engineering commitment and intent-freeze boundary. The exact freeze protocol and material-change rule are defined in [Workflow](WORKFLOW.md) and [Artifacts](ARTIFACTS.md).

## Publication

Required lifecycle artifacts must be durable and reviewable before the next independent gate begins. The authorized workflow coordination path records their immutable provenance and reachable reference; publication is not design approval, evaluation approval, or release authorization. See [Artifacts](ARTIFACTS.md), [Workflow](WORKFLOW.md), and [Governance](GOVERNANCE.md).

## Scope boundary

This repository defines SDLC outcomes and controls, not agent or skill implementation. Component repositories own their runtime and deployment design while remaining compatible with these contracts. See [Architecture](ARCHITECTURE.md).
