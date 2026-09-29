# Agentic SDLC

Reference architecture, workflow, governance, and implementation guidance for an intent-driven Agentic SDLC. It adapts established practices to a human-friendly operating model: **Kanban for flow, XP for quality, humans for consequential judgment, and AI for bounded execution.**

This repository is the canonical home for the plan and its durable documentation. It does not replace product repositories, the dedicated `intent-backlog` repository, or component implementations.

## The operating model

```text
Trello (human workflow) <-> intent-backlog (product intent)
                                      |
                            Prioritized: freeze a revision
                                      |
Product repository worktree: frozen intent -> spec and design -> execution
                                      |
                        independent evaluation -> human decision
                                      |
                           release -> observe outcome -> close
```

The board flow is **New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution -> Evaluation -> Human Approval -> Release -> Production Observation -> Done**. It is deliberately lean: each column has one clear purpose, owner, and exit condition. Human review happens within `Spec & Design`; there are no separate Design Review or Planning columns.

This is a learning exercise that will build real applications. Begin with a small, low-risk feature, use the same durable artifacts and gates that a larger product would use, and improve only from observed work. [The learning workflow](docs/LEARNING_WORKFLOW.md) turns the model into a practical first-feature walkthrough. The detailed architecture begins in [docs/AGENTIC_SDLC_CONTEXT.md](docs/AGENTIC_SDLC_CONTEXT.md). Use [docs/WORKFLOW.md](docs/WORKFLOW.md) for lifecycle operations, [docs/GOVERNANCE.md](docs/GOVERNANCE.md) for decision rights, and [docs/ROADMAP.md](docs/ROADMAP.md) for implementation sequencing.

## Documentation map

| Document | Purpose |
| --- | --- |
| [Architecture](docs/ARCHITECTURE.md) | System boundaries, ownership, and execution topology |
| [Workflow](docs/WORKFLOW.md) | State transitions, gates, and lifecycle behavior |
| [Governance](docs/GOVERNANCE.md) | Authority, controls, and exceptions |
| [Artifacts](docs/ARTIFACTS.md) | Artifact contracts and provenance |
| [Stage contracts](docs/STAGE_CONTRACTS.md) | Required inputs, outputs, rules, and handoffs from intent through closure |
| [Agent roles](docs/AGENT_ROLES.md) | Narrow, independent agent responsibilities |
| [Metrics](docs/METRICS.md) | Flow, quality, and outcome measures |
| [Roadmap](docs/ROADMAP.md) | Incremental implementation plan |
| [References](docs/REFERENCES.md) | First-party sources informing the design |
| [Contract decisions](docs/CONTRACT_DECISIONS.md) | Canonical cross-component format, integrity, workflow, and deployment decisions |
| [Learning workflow](docs/LEARNING_WORKFLOW.md) | A safe, practical first-feature walkthrough and learning loop |

## Status

This is an architectural baseline, not a claim that every component is implemented. The roadmap identifies the remaining implementation decisions and validation needed before autonomous production use.
