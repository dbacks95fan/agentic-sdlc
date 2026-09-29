# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Product, intent identity, version, and the commit that stores each revision |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit and intent fingerprint |
| Policy & Compliance Profile | Product intent backlog | Product owner | Profile ID, version, applicability decisions, and control IDs |
| `spec.md` | Product worktree | Spec & Design Agent | Frozen intent/profile references; design, implementation, validation, and decision sections |
| `work-contract.yaml` | Product worktree | Spec & Design Agent; frozen by product owner | Frozen intent/profile and accepted `spec.md` references; target, scope, and required validation |
| Evidence Package | Product worktree | Coding Agent | Candidate commit, validation results, control evidence, and status |
| Evaluation Package | Product worktree or authorized versioned record | Evaluator | Candidate commit, checks reproduced, findings, and outcome status |
| Human Approval Record | Authorized workflow record | Accountable human | Candidate SHA, decision, rationale, and time |
| Release Record | Product worktree or authorized versioned record | Authorized release control | Candidate SHA, environment, release/health facts, and rollback facts |
| Observation Record | Product worktree or authorized versioned record | Observation control with human closure | Observation window, signals, outcome, findings, and follow-up links |
| Execution artifacts | Product worktree | Coding Agent | Frozen intent, profile, `spec.md`, and work-contract references |

## Intent content

What an intent must capture, and the outcomes the Intent Creation Skill must deliver, are described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). This repository describes outcomes only; the layout of `intent.md` and the names inside it are the implementer's choice. No workflow control depends on that layout: every value the workflow checks is held in the freeze tuple and verified by fingerprint.

## Integrity and freeze tuple

The intent fingerprint is the SHA-256 of the exact bytes of `intent.md` as stored at the frozen commit in `intent-backlog`. Anyone with read access can recompute it with Git and a standard SHA-256 tool, without knowing how the file was produced. Because the fingerprint covers the whole file, `intent.md` must not contain values that are only known after it is saved, such as the commit that stores it; those values live on the card and in the freeze tuple. The frozen policy profile is identified by its own immutable version reference.

At Prioritized, the freeze tuple records intent ID, product ID, intent version, intent repository commit SHA, intent fingerprint, Trello card ID, and freeze time. The first engineering-stage worker copies the file from that commit into the worktree and confirms the copy has the same fingerprint; later agents verify the same frozen artifact. A copy whose bytes differ for any reason, including line-ending conversion on checkout, fails the check.

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source commit, and intent fingerprint agree between the Conductor record and the Trello card, and that both the backlog file at the source commit and the worktree copy match the fingerprint. Never overwrite a conflict casually: surface it, reconcile it with the authorized owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. The precise schemas are an implementation-roadmap deliverable; this document defines the required lineage and ownership now.

## Durability and publication

Every artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact needed by Spec & Design or Execution.

Before Execution starts, the workflow records the `spec.md` artifact's immutable commit SHA, repository-relative path, and reachable reference. The reference and provenance tuple make the execution input unambiguous without changing the frozen intent.

An assigned worker may commit only its authorized artifacts on its assigned work branch. The Conductor publishes a validated branch when a human or later worker needs a reachable reference, and records that reference with the stage handoff. Publication is neither approval nor merge. A worker without remote access remains correct; a required publication failure blocks the handoff and is surfaced to the authorized owner.

Publication is a workflow control that makes an artifact available to the next defined stage. It does not change the frozen intent or create an undeclared board stage. The candidate, Evaluation Package, Human Approval Record, Release Record, and Observation Record retain the same immutable candidate reference so the work can be traced from intent to observed outcome.
