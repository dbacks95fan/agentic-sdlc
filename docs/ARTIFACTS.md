# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Canonical schema, identity, revision, and integrity metadata |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit, normalized content hash, and exact-byte artifact hash |
| Policy & Compliance Profile | Product intent backlog | Product owner | Profile ID, version, applicability decisions, and control IDs |
| `spec.md` | Product worktree | Spec & Design Agent | Frozen intent/profile references; design, implementation, validation, and decision sections |
| `work-contract.yaml` | Product worktree | Spec & Design Agent; frozen by product owner | Frozen intent/profile and accepted `spec.md` references; target, scope, and required validation |
| Evidence Package | Product worktree | Coding Agent | Candidate commit, validation results, control evidence, and status |
| Evaluation Package | Product worktree or authorized versioned record | Evaluator | Candidate commit, checks reproduced, findings, and outcome status |
| Human Approval Record | Authorized workflow record | Accountable human | Candidate SHA, decision, rationale, and time |
| Release Record | Product worktree or authorized versioned record | Authorized release control | Candidate SHA, environment, release/health facts, and rollback facts |
| Observation Record | Product worktree or authorized versioned record | Observation control with human closure | Observation window, signals, outcome, findings, and follow-up links |
| Execution artifacts | Product worktree | Coding Agent | Frozen intent, profile, `spec.md`, and work-contract references |

## Canonical intent schema

The sole canonical `intent.md` format is maintained by the [Intent Creation
Skill](https://github.com/dbacks95fan/intent-creation-skill/blob/main/references/output-format.md).
It defines the frontmatter names, permitted statuses, canonical rendering, and
required handoff fields. Other repositories must reference that format rather
than restating or extending its field names informally.

In particular, use `intent_version`, `intent_commit`, and the status value
`Refining`; do not use local aliases such as `version`, `last_synced_commit`, or
lowercase `refinement`.

## Integrity and freeze tuple

Two hashes serve distinct purposes and must never be conflated:

| Field | Definition | Used for |
| --- | --- | --- |
| `intent_content_sha256` | SHA-256 of the canonical normalized rendering defined by the Intent Creation Skill, excluding generated commit metadata | Product-side synchronization, semantic intent identity, and the Trello projection |
| `frozen_artifact_sha256` | SHA-256 of the raw bytes of the exact `intent.md` snapshot copied into the engineering worktree | Byte-for-byte engineering chain of custody |

At Prioritized, the freeze tuple records intent ID, product ID, intent
version, intent repository commit SHA, both hashes, Trello card ID, and freeze
time. The first engineering-stage worker verifies the raw-byte artifact hash
after copying the frozen file; later agents verify the same frozen artifact and
retain the normalized content hash as provenance.

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source
commit, normalized content hash, and frozen-artifact hash agree across the
Conductor record, Trello projection, backlog artifact, and worktree copy. Never
overwrite a conflict casually: surface it, reconcile it with the authorized
owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. The precise schemas are an implementation-roadmap deliverable; this document defines the required lineage and ownership now.

## Durability and publication

Every artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact needed by Spec & Design or Execution.

Before Execution starts, the workflow records the `spec.md` artifact's immutable commit SHA, repository-relative path, and reachable reference. The reference and provenance tuple make the execution input unambiguous without changing the frozen intent.

## Publication authority

An assigned worker may commit only its authorized artifacts on its assigned work branch. The Conductor publishes a validated branch when a human or later worker needs a reachable reference, and records that reference with the stage handoff. Publication is not an approval or merge. A worker without remote access remains correct; a required publication failure blocks the handoff and is surfaced to the authorized owner.

`spec.md` is the one human-reviewed Markdown document. It includes implementation and validation planning as required by [Stage Contracts](STAGE_CONTRACTS.md); do not create a separate `plan.md`. The machine-readable `work-contract.yaml` is frozen only after the product owner accepts the associated `spec.md`.

Publication is a workflow control that makes an artifact available to the next defined stage. It does not change the frozen intent or create an undeclared board stage. The candidate, Evaluation Package, Human Approval Record, Release Record, and Observation Record retain the same immutable candidate reference so the work can be traced from intent to observed outcome.
