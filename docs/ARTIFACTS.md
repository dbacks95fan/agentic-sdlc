# Artifact Model

| Artifact | Authoritative location | Created/updated by | Required provenance |
| --- | --- | --- | --- |
| `intent.md` | `intent-backlog` | Intent Creation Skill with product owner | ID, product, version, card ID, sync commit |
| Frozen `intent.md` | Product worktree | First engineering agent | Source commit and SHA-256 hash |
| `spec.md` | Product worktree | Spec & Design Agent | Frozen intent reference, spec version, review status |
| `plan.md` | Product worktree | Planning Agent | Spec reference, dependencies, validation plan |
| Work contract | Product worktree | Conductor/planning stage | Intent/spec/plan references, gates, approvals |
| Coding evidence | Product worktree | Coding Agent and deterministic tools | Command/result, environment, commit, timestamp |
| Evaluation result | Product worktree or designated evidence store | Evaluator | Candidate/base commits, facts, findings, disposition |
| Decision brief | Trello card | Conductor from durable result | Links to source evidence and next action |
| Release/observation record | Delivery and observability systems | Release process | Release identity, health, outcome signals |

## Minimal intent metadata

```yaml
intent_id: INT-MF-0042
product_id: MF
version: 5
status: refinement
trello_card_id: "..."
last_synced_commit: "..."
```

## Integrity rules

Before execution, validate that product ID, intent ID, intent version, source commit, and content hash agree across the Conductor record, Trello projection, backlog artifact, and worktree copy. Never overwrite a conflict casually: surface it, reconcile it with the authorized owner, and record the resulting version.

Artifacts become durable inputs to later stages rather than disposable prompts. The precise schemas are an implementation-roadmap deliverable; this document defines the required lineage and ownership now.
