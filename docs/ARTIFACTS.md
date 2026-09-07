# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Canonical schema, identity, revision, and integrity metadata |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit, normalized content hash, and exact-byte artifact hash |
| `spec.md` | Product worktree | Spec & Design Agent | Frozen intent reference, spec version, review status |
| `plan.md` | Product worktree | Planning Agent | Spec reference, dependencies, validation plan |
| Work contract | Product worktree | Conductor/planning stage | Intent/spec/plan references, gates, approvals |
| Coding evidence | Product worktree | Coding Agent and deterministic tools | Command/result, environment, commit, timestamp |
| Evaluation result | Product worktree or designated evidence store | Evaluator | Candidate/base commits, facts, findings, disposition |
| Decision brief | Trello card | Conductor from durable result | Links to source evidence and next action |
| Release/observation record | Delivery and observability systems | Release process | Release identity, health, outcome signals |

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

Every engineering-stage artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact required for review, evaluation, approval, release, or later audit.

Before Human Design Review or independent Evaluation starts, the workflow records the artifact's immutable commit SHA, repository-relative path, and reachable review or evaluation reference. The reference and provenance tuple make the reviewable input unambiguous without changing the frozen intent.

Publication is a workflow control that makes a candidate available to the next stage. It does not confer authority to approve the design, accept the evaluation result, or release the product.
