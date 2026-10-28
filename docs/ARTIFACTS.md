# Artifact Model

**Audience:** component builders and maintainers. This document is normative for artifact ownership, location, provenance, integrity, and publication. The [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json) are authoritative for machine-readable field shapes; this document explains how those records are used and validated.

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| Product Constitution | Product repository, maintained with product documentation | Product owner; changes reviewed through the normal repository workflow | Product/repository identity and Git revision |
| Product Architecture | Product repository, maintained with product documentation | Product owner and engineering contributors; changes reviewed through the normal repository workflow | Product/repository identity and Git revision; links to accepted architecture decisions where useful |
| `product.yaml` | `products/<product-slug>/` in `intent-backlog` | Product owner; updated through the repository's normal review process | Product identity, Trello board and explicit lifecycle-stage-to-list-ID mapping, target repository, authorized owner identities, and profile-ID/path mapping |
| `BoardTransitionEvent` | Authenticated Trello event plus current board state | Trello adapter; validated and correlated by Conductor | Event ID, board/card/list IDs, actor, and event time |
| `StageInvocation` / `StageResult` | Authorized control-plane record and assigned worker result | Conductor / invoked worker | Invocation ID, triggering board event, work identity, immutable inputs, target revision, permissions, status, and output references |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Product, intent identity, version, and the commit that stores each revision |
| Frozen `intent.md` | Product work branch | First engineering agent | Source commit and exact-byte intent fingerprint |
| Policy & Compliance Profile | `products/<product>/policy-profiles/` in `intent-backlog`, with its path mapped by `product.yaml` | Product owner | Profile ID, version, source commit, exact-byte fingerprint, applicability decisions, and control IDs |
| `spec.md` | Product work branch | Spec & Design Agent | Frozen intent/profile references; design, implementation, validation, and decision sections |
| `work-contract.yaml` | Product work branch | Spec & Design Agent; frozen by product owner | Frozen intent/profile references; target, scope, validation, policy-control, and release/observation requirements |
| Spec Acceptance Record | Authorized workflow record | Conductor after product-owner acceptance | Immutable references, paths, and content hashes for the accepted `spec.md` and `work-contract.yaml` |
| Evidence Package | Product work branch | Coding Agent | Candidate commit, validation results, control evidence, and status |
| Evaluation Package | Assigned artifact path on the product work branch | Evaluator | Candidate commit, checks reproduced, findings, and outcome status; may not alter candidate code |
| Human Approval Record | Authorized workflow record | Accountable human | Candidate SHA, decision, rationale, and time |
| Release Record | Product worktree or authorized versioned record | Release and Observation Controls | Candidate SHA, approval reference, integration SHA when produced, environment, release/health facts, and rollback facts |
| Observation Record | Product worktree or authorized versioned record | Observation control with human closure | Observation window, signals, outcome, findings, and follow-up links |
| Closure Record | Authorized workflow record | Accountable human | Stopped or rolled-back reason, latest immutable artifact, decision, and follow-up |
| Execution artifacts | Product work branch | Coding Agent | Frozen intent, profile, `spec.md`, and work-contract references |

## Intent content

The constitution and architecture are product-level references, not per-intent artifacts. Spec & Design consults the current product constitution and architecture when producing a feature's `spec.md` and implementation plan. The spec records relevant constraints and architectural assumptions and links to the files or decisions it relied on when that context materially affects the design. These baseline documents are not copied into each work item or frozen as part of the intent tuple. If a proposed feature conflicts with a constitution rule or requires an architectural change, Spec & Design makes the conflict and tradeoff explicit for human decision; it does not silently revise the baseline. An accepted baseline change is maintained in the product repository through its normal review process.

Each product's `product.yaml` contains a `policy_profiles` mapping from profile ID to a repository-relative profile path, for example `MEALFLOW-DEFAULT: policy-profiles/MEALFLOW-DEFAULT.md`. The selected profile ID and version are identified by the intent and projected on the Trello card as `Policy Profile ID` and `Policy Profile Version`. The profile file exposes `Profile ID` and `Version` in its human-readable metadata. The Conductor resolves `product.yaml` and the profile at the exact commit pinned by the intent, verifies identity/version, and computes the raw-byte SHA-256; it never guesses a path or substitutes the latest profile. What an intent must capture, and the outcomes the Intent Creation Skill must deliver, are described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). The layout of `intent.md` and the names of its sections remain implementation-defined; the required information and identity/version invariants do not.

Intent versions are positive integers starting at `1` and increase by exactly one for each accepted revision. Policy-profile versions are preserved as non-empty strings from the profile metadata so a product can use its established version label; the Conductor compares that label exactly and never substitutes a newer profile.

## Integrity and freeze tuple

The intent fingerprint is the SHA-256 of the exact bytes of `intent.md` as stored at the frozen commit in `intent-backlog`. Anyone with read access can recompute it with Git and a standard SHA-256 tool, without knowing how the file was produced. Because the fingerprint covers the whole file, `intent.md` must not contain values that are only known after it is saved, such as the commit that stores it; those values live on the card and in the freeze tuple. The policy profile uses the same source-commit and exact-byte-fingerprint rule.

At Prioritized, the Conductor appends a human-readable Trello card comment containing a machine-readable freeze event with product ID, intent ID, intent version, intent repository commit SHA, intent fingerprint, policy-profile ID, policy-profile version, policy-profile repository commit SHA, policy-profile fingerprint, Trello card ID, authorizing owner, and freeze time. It also updates the card's current `Freeze State` projection to `Active`. A return to Backlog or New Ideas causes the Conductor to update the current projection to `Revoked` and append a revocation event; prior comments and Git revisions are retained. A later prioritization creates a new event for the newly synchronized revision. The first engineering-stage worker obtains byte-preserving copies directly from the recorded source revisions, places them in the worktree, and confirms their fingerprints; later agents verify the same frozen artifacts. A copy whose bytes differ for any reason, including line-ending conversion on checkout, fails the check.

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source commit, and intent fingerprint agree between the Conductor record and the Trello card; validate the policy-profile ID, version, source commit, and fingerprint against that same record; and confirm both backlog artifacts and worktree copies match their fingerprints. Every receiving worker independently verifies its frozen input bytes against the recorded source references. The Conductor may perform these checks through read-only repository access; it must not write repository content or refs. Never overwrite a conflict casually: surface it, reconcile it with the authorized owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. Machine-readable record shapes are defined in [`schemas/lifecycle-contracts.schema.json`](../schemas/lifecycle-contracts.schema.json). Its `$defs` are normative and are selected by JSON Pointer (for example `#/$defs/WorkContract`). YAML files are parsed as YAML, converted to JSON-compatible data, and validated against the same schema. Validators must enforce `format` constraints as well as types, required fields, enums, and `additionalProperties: false`. The following table is a navigation aid; the schema is authoritative for fields and types.

| Schema definition | Use |
| --- | --- |
| `ProductConfig` | Product/board/repository identity, explicit board list mapping, authorized product-owner identities, and profile mapping |
| `BoardTransitionEvent` | Validated input from the Trello adapter; the Conductor must also verify current board state |
| `WorkItem` | Stable product, intent, version, and Trello-card identity carried through lifecycle records |
| `ArtifactRef`, `CommitRef` | Immutable Git provenance; file artifacts include exact-byte SHA-256, while commit references identify a repository revision |
| `StageInvocation`, `StageResult`, `StageResultDecision` | Runtime-neutral request/result boundary; StageInvocation 1.1 includes the typed Prioritized freeze record so workers can identify frozen intent and policy-profile inputs; StageResultDecision records the Conductor's durable acceptance/rejection |
| `FreezeActivationEvent`, `FreezeRevocationEvent` | Append-only records for Prioritized freeze activation and authorized revocation |
| `WorkContract` | Accepted machine-readable scope, frozen input references, validation, policy controls, and release/observation requirements |
| `SpecAcceptanceRecord` | Human acceptance and immutable binding of `spec.md` and `work-contract.yaml` before Execution |
| `EvidencePackage`, `EvaluationPackage` | Coding and independent evaluation results tied to the immutable candidate |
| `HumanApprovalRecord` | Accountable human decision tied to a candidate SHA |
| `ReleaseRecord`, `ObservationRecord`, `ClosureRecord` | Integration/release facts, production observations, and deliberate stop/rollback closure |

The `schema_version` is the record contract version. `work-contract.yaml` uses schema version `3`; earlier versions are not valid inputs to this canonical workflow. A consumer rejects an unsupported version or invalid record and reports `blocked`; it must not silently coerce, drop, or invent fields. A backward-incompatible field or meaning change requires a new schema version and an explicit compatibility/migration note. Schema validity proves shape only, not authorization, hash correctness, legal compliance, or business truth; consumers must still verify those facts against their authoritative source. Version 3 does not include a separate `plan.md`: the accepted `spec.md` contains the human-readable implementation and validation plan, while the work contract is the machine-readable execution record.

`WorkContract.required_validation` names repository-declared deterministic checks. A worker must verify each command against the target repository's trusted instructions or configured check manifest before execution. It must not execute an arbitrary command merely because it appears in agent-generated or card-supplied text.

## Durability and publication

Every artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact needed by Spec & Design or Execution.

Before Execution starts, the Spec Acceptance Record binds both `spec.md` and `work-contract.yaml` with immutable repository references, repository-relative paths, and content hashes. It makes the execution input unambiguous without requiring either file to contain the commit that stores itself.

The first engineering worker creates the work branch from the assigned base commit. Each later worker starts with a fresh checkout at the exact branch commit supplied by the preceding accepted handoff. A worker commits and pushes only its authorized artifacts to that assigned branch; it does not force-push, write outside its assigned paths, or merge into the protected integration branch. Only one worker may write to the work branch at a time. A push failure blocks the handoff and is surfaced for human resolution.

The Conductor verifies branch, commit, and artifact references through read-only access; it does not create, check out, commit to, push, or merge product-repository refs. A worker's push makes a handoff reachable but is neither approval nor a workflow transition. The candidate, Evaluation Package, Human Approval Record, Release Record, Observation Record, and Closure Record retain the relevant immutable candidate or integration reference so the work can be traced from intent to outcome or deliberate closure. The authorized release path merges the approved candidate only after a passing Evaluation Package and human approval.
