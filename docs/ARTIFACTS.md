# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | Product, intent identity, version, and the commit that stores each revision |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit and intent fingerprint |
| `spec.md` | Product worktree | Spec & Design worker | Frozen intent reference and source commit |
| Execution artifacts | Product worktree | Assigned execution worker | Frozen intent and `spec.md` references, command/result where applicable |

## Intent content

What an intent must capture, and the outcomes the Intent Creation Skill must deliver, are described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). This repository describes outcomes only; the layout of `intent.md` and the names inside it are the implementer's choice. No workflow control depends on that layout: every value the workflow checks is held in the freeze tuple and verified by fingerprint.

## Integrity and freeze tuple

The intent fingerprint is the SHA-256 of the exact bytes of `intent.md` as stored at the frozen commit in `intent-backlog`. Anyone with read access can recompute it with Git and a standard SHA-256 tool, without knowing how the file was produced. Because the fingerprint covers the whole file, `intent.md` must not contain values that are only known after it is saved, such as the commit that stores it; those values live on the card and in the freeze tuple.

At Prioritized, the freeze tuple records intent ID, product ID, intent version, intent repository commit SHA, intent fingerprint, Trello card ID, and freeze time. The first engineering-stage worker copies the file from that commit into the worktree and confirms the copy has the same fingerprint; later agents verify the same frozen artifact. A copy whose bytes differ for any reason, including line-ending conversion on checkout, fails the check.

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source commit, and intent fingerprint agree between the Conductor record and the Trello card, and that both the backlog file at the source commit and the worktree copy match the fingerprint. Never overwrite a conflict casually: surface it, reconcile it with the authorized owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. The precise schemas are an implementation-roadmap deliverable; this document defines the required lineage and ownership now.

## Durability and publication

Every artifact in this document is retained in the assigned work branch or another versioned record authorized by the lifecycle. Repository ignore rules must not cause loss of an artifact needed by Spec & Design or Execution.

Before Execution starts, the workflow records the `spec.md` artifact's immutable commit SHA, repository-relative path, and reachable reference. The reference and provenance tuple make the execution input unambiguous without changing the frozen intent.

Publication is a workflow control that makes an artifact available to the next defined stage. It does not change the frozen intent or create an undeclared board stage.
