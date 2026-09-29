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
| Trello | Human-visible flow and summaries | Canonical artifact contents |
| Intent Creation Skill | Refinement and synchronization contract | Engineering execution after freeze |
| `intent-backlog` | Canonical evolving product intent and revision history | Product code or execution evidence |
| Conductor | State transitions, routing, retries, and board updates | Execution conclusions |
| Target product repository | Branch-scoped engineering artifacts, code, candidate, and evidence | Product intent or workflow state |
| Spec & Design worker | Produce `spec.md` from a frozen intent | Alter the frozen intent |
| Coding Agent | Carry out assigned work from durable inputs | Mutate frozen inputs or workflow state |
| Evaluator | Independently assess an immutable candidate and evidence | Modify code, approve, or release work |
| Release and observation controls | Record authorized delivery and outcome facts | Decide product intent, approval, or compliance applicability |
| Humans | Priority and consequential judgment | Routine deterministic work |

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
  work-contract.yaml
  evidence-package/
  evaluation-package/
  release-record/
  observation-record/
```

The frozen intent, accepted `spec.md`, frozen work contract, candidate, and later evidence form one traceable delivery chain. Artifact details are defined in [Artifacts](ARTIFACTS.md) and [Stage Contracts](STAGE_CONTRACTS.md).

## Design constraints

Agents remain stateless between runs; durable state belongs in versioned artifacts and Conductor-managed workflow records. Grant least privilege by role. Enforce critical architecture, validation, and policy invariants mechanically where possible. Record facts, inferences, and unresolved decisions separately.

## Scope boundary

This repository defines lifecycle stages, role boundaries, artifact controls, and governance outcomes. Agent and skill runtime, packaging, deployment, credential mechanics, and implementation-specific recovery belong in the responsible component repository. Those choices must satisfy the lifecycle controls defined here without changing them.
