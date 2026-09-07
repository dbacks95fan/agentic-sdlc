# Agentic SDLC

Reference architecture, workflow, governance, and implementation guidance for an intent-driven Agentic SDLC. It adapts established practices to a human-friendly operating model: **Kanban for flow, XP for quality, humans for consequential judgment, and AI for bounded execution.**

This repository is the canonical home for the plan and its durable documentation. It does not replace product repositories, the dedicated `intent-backlog` repository, or the coding-agent, evaluator-agent, and conductor implementations.

## The operating model

```text
Trello (human workflow) <-> intent-backlog (product intent)
                                      |
                      Ready for Planning: freeze a revision
                                      |
Product repository worktree: intent -> spec -> plan -> code -> evidence
                                      |
                      independent evaluation -> human approval -> release
                                      |
                         production observation and feedback
```

The detailed architecture begins in [docs/AGENTIC_SDLC_CONTEXT.md](docs/AGENTIC_SDLC_CONTEXT.md). Use [docs/WORKFLOW.md](docs/WORKFLOW.md) for lifecycle operations, [docs/GOVERNANCE.md](docs/GOVERNANCE.md) for decision rights, and [docs/ROADMAP.md](docs/ROADMAP.md) for implementation sequencing.

## Documentation map

| Document | Purpose |
| --- | --- |
| [Architecture](docs/ARCHITECTURE.md) | System boundaries, ownership, and execution topology |
| [Workflow](docs/WORKFLOW.md) | State transitions, gates, and lifecycle behavior |
| [Governance](docs/GOVERNANCE.md) | Authority, controls, and exceptions |
| [Artifacts](docs/ARTIFACTS.md) | Artifact contracts and provenance |
| [Agent roles](docs/AGENT_ROLES.md) | Narrow, independent agent responsibilities |
| [Metrics](docs/METRICS.md) | Flow, quality, and outcome measures |
| [Roadmap](docs/ROADMAP.md) | Incremental implementation plan |
| [References](docs/REFERENCES.md) | First-party sources informing the design |
| [Contract decisions](docs/CONTRACT_DECISIONS.md) | Canonical cross-component format, integrity, workflow, and deployment decisions |

## Status

This is an architectural baseline, not a claim that every component is implemented. The roadmap identifies the remaining implementation decisions and validation needed before autonomous production use.
