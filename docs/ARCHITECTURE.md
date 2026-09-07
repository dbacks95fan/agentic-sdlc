# Architecture

## System boundaries

```text
People <-> Trello <-> Conductor <-> agent runners
                 |                    |
                 |                    +-- isolated product-repo worktrees
                 v
         intent-backlog repository
                 |
                 +-- frozen revision copied into worktree
```

| Component | Owns | Does not own |
| --- | --- | --- |
| Trello | Human-visible flow, summaries, approvals, escalation | Canonical artifact contents |
| Intent Creation Skill | Refinement and synchronization contract | Engineering execution after freeze |
| `intent-backlog` | Canonical evolving product intent and revision history | Product code or execution evidence |
| Conductor | State transitions, routing, retries, and board updates | Coding or evaluation conclusions |
| Target product repository | Branch-scoped execution artifacts, code, and evidence | Uncommitted product discovery |
| Coding Agent | Scoped implementation and declared evidence | Final acceptance of its own work |
| Evaluator | Independent, read-only evaluation result | Trello transitions or code mutation |
| Humans | Priority, design, consequential approval, release authority | Routine deterministic work |

## Product and intent identity

The intent backlog is namespaced by product. A lightweight product registry supplies product ID, board reference, target repository, and ownership. Example:

```text
products/mealflow/product.yaml
products/mealflow/intents/INT-MF-0042/intent.md
products/agentic-sdlc/intents/INT-AS-0017/intent.md
```

`intent.md` and its Trello card carry the same `product_id`, `intent_id`,
`intent_version`, `intent_commit`, and canonical content hash. The field names
and permitted values are owned by the Intent Creation Skill's canonical schema;
this repository does not define alternatives. Cross-product initiatives are
parent/portfolio intents decomposed into one execution intent per product; one
execution intent never spans unrelated product repositories.

## Execution workspace

The worktree is created only from a frozen input. A suggested durable layout is:

```text
.agent/work/INT-MF-0042/
  intent.md
  spec.md
  plan.md
  work-contract.json
  evidence/
  evaluation/
```

The work contract includes intent path, source commit, normalized content hash,
frozen-artifact hash, approval metadata, acceptance criteria, constraints,
non-goals, dependencies, validation gates, and escalation conditions. Every
downstream artifact links back to it.

## Design constraints

Agents remain stateless between runs; durable state belongs in versioned artifacts and Conductor-managed workflow records. Grant least privilege by role. Enforce critical architecture, validation, and policy invariants mechanically where possible. Record facts, inferences, and unresolved decisions separately.

## Deployment boundary

Containerization is an agent-specific deployment decision, not a current
system-wide mandate. Every agent must be bounded, observable, and deployable
through an approved runtime, but an existing local-process worker is not made
noncompliant merely because it is not containerized. A container requirement
must be adopted explicitly with its operational, secret-management, and health
verification controls.
