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

## Deployment

The architecture does not impose a blanket container mandate. Runtime selection
is an agent-specific decision subject to bounded execution, least privilege,
observability, and verified operational health.
