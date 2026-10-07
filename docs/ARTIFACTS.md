# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Product, intent identity, version, and the commit that stores each revision |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit and exact-byte intent fingerprint |
| Policy & Compliance Profile | Product intent backlog | Product owner | Profile ID, version, source commit, exact-byte fingerprint, applicability decisions, and control IDs |
| `spec.md` | Product worktree | Spec & Design Agent | Frozen intent/profile references; design, implementation, validation, and decision sections |
| `work-contract.yaml` | Product worktree | Spec & Design Agent; frozen by product owner | Frozen intent/profile references; target, scope, validation, and release/observation requirements |
| Spec Acceptance Record | Authorized workflow record | Conductor after product-owner acceptance | Immutable references, paths, and content hashes for the accepted `spec.md` and `work-contract.yaml` |
| Evidence Package | Product worktree | Coding Agent | Candidate commit, validation results, control evidence, and status |
| Evaluation Package | Product worktree or authorized versioned record | Evaluator | Candidate commit, checks reproduced, findings, and outcome status |
| Human Approval Record | Authorized workflow record | Accountable human | Candidate SHA, decision, rationale, and time |
| Release Record | Product worktree or authorized versioned record | Release and Observation Controls | Candidate SHA, integration SHA, environment, release/health facts, and rollback facts |
| Observation Record | Product worktree or authorized versioned record | Observation control with human closure | Observation window, signals, outcome, findings, and follow-up links |
| Closure Record | Authorized workflow record | Accountable human | Stopped or rolled-back reason, latest immutable artifact, decision, and follow-up |
| Execution artifacts | Product worktree | Coding Agent | Frozen intent, profile, `spec.md`, and work-contract references |

## Intent content

What an intent must capture, and the outcomes the Intent Creation Skill must deliver, are described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). This repository describes outcomes only; the layout of `intent.md` and the names inside it are the implementer's choice. No workflow control depends on that layout: every value the workflow checks is held in the freeze tuple and verified by fingerprint.

## Integrity and freeze tuple

The intent fingerprint is the SHA-256 of the exact bytes of `intent.md` as stored at the frozen commit in `intent-backlog`. Anyone with read access can recompute it with Git and a standard SHA-256 tool, without knowing how the file was produced. Because the fingerprint covers the whole file, `intent.md` must not contain values that are only known after it is saved, such as the commit that stores it; those values live on the card and in the freeze tuple. The policy profile uses the same source-commit and exact-byte-fingerprint rule.

At Prioritized, the freeze tuple records product ID, intent ID, intent version, intent repository commit SHA, intent fingerprint, policy-profile ID, policy-profile version, policy-profile repository commit SHA, policy-profile fingerprint, Trello card ID, authorizing owner, and freeze time. If the card returns to Backlog before Spec & Design starts, the Conductor revokes the active freeze and preserves the prior tuple and revision in workflow history; the Intent Creation Skill may then commit a new version. Re-prioritization records a new tuple for the new revision. Once Spec & Design starts, the active freeze cannot be revoked for that work item. The first engineering-stage worker obtains byte-preserving copies directly from the recorded source revisions, places them in the worktree, and confirms their fingerprints; later agents verify the same frozen artifacts. A copy whose bytes differ for any reason, including line-ending conversion on checkout, fails the check.

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source commit, and intent fingerprint agree between the Conductor record and the Trello card; validate the policy-profile ID, version, source commit, and fingerprint against that same record; and confirm both backlog artifacts and worktree copies match their fingerprints. Never overwrite a conflict casually: surface it, reconcile it with the authorized owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. The precise schemas are an implementation-roadmap deliverable; this document defines the required lineage and ownership now.

## Durability and publication

Every artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact needed by Spec & Design or Execution.

Before Execution starts, the Spec Acceptance Record binds both `spec.md` and `work-contract.yaml` with immutable repository references, repository-relative paths, and content hashes. It makes the execution input unambiguous without requiring either file to contain the commit that stores itself.

An assigned worker may commit only its authorized artifacts on its assigned work branch. The Conductor publishes a validated branch when a human or later worker needs a reachable reference, and records that reference with the stage handoff. Publication is neither approval nor merge. A worker without remote access remains correct; a required publication failure blocks the handoff and is surfaced to the authorized owner.

Publication is a workflow control that makes an artifact available to the next defined stage. It does not change the frozen intent or create an undeclared board stage. The candidate, Evaluation Package, Human Approval Record, Release Record, Observation Record, and Closure Record retain the relevant immutable candidate or integration reference so the work can be traced from intent to outcome or deliberate closure.
