# Agentic SDLC

Reference architecture, workflow, governance, and implementation guidance for an intent-driven Agentic SDLC. It adapts established practices to a human-friendly operating model: **Kanban for flow, XP for quality, humans for consequential judgment, and AI for bounded execution.**

This repository is the canonical home for the plan and its durable documentation. It does not replace product repositories, the dedicated `intent-backlog` repository, or component implementations.

## Building a component from this repository

An LLM or engineer building one component should start with [Agent roles](docs/AGENT_ROLES.md), then read the corresponding stage in [Stage contracts](docs/STAGE_CONTRACTS.md), the related workflow and governance rules, and the applicable definitions in [Lifecycle schemas](schemas/lifecycle-contracts.schema.json). Those documents are the normative requirements for that component's purpose, trigger, permissions, inputs, responsibilities, outputs, errors, and handoffs. The component repository may choose its runtime, model, tools, packaging, and deployment, but it must validate to the shared contracts and may not invent workflow behavior.

## The operating model

```text
Control plane: Trello <-> Intent Creation Skill + Conductor <-> intent-backlog
                                                        |
                     immutable, validated handoff contracts
                                     v
Execution plane: isolated worktree -> spec/design -> candidate -> evaluation
                                     |
                     durable status, artifact, and evidence records
                                     v
Control plane: approval -> release -> observation -> closure
```

The planes have separate responsibilities and may use different LLMs. A control-plane LLM supports workflow-facing work; an execution-plane LLM supports bounded engineering work. Neither plane is tied to a provider or model. The model assignments are independently configurable, and they do not change the documented authority, immutable-handoff, or human-decision rules.

The board flow is **New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution -> Evaluation -> Human Approval -> Release -> Production Observation -> Done**. It is deliberately lean: each column has one clear purpose, owner, and exit condition. Human review happens within `Spec & Design`; there are no separate Design Review or Planning columns.

This is a learning exercise that will build real applications. Begin with a small, low-risk feature, use the same durable artifacts and gates that a larger product would use, and improve only from observed work. [The learning workflow](docs/LEARNING_WORKFLOW.md) turns the model into a practical first-feature walkthrough. The detailed architecture begins in [docs/AGENTIC_SDLC_CONTEXT.md](docs/AGENTIC_SDLC_CONTEXT.md). Use [docs/WORKFLOW.md](docs/WORKFLOW.md) for lifecycle operations, [docs/GOVERNANCE.md](docs/GOVERNANCE.md) for decision rights, and [docs/ROADMAP.md](docs/ROADMAP.md) for implementation sequencing.

## Documentation map

| Document | Purpose |
| --- | --- |
| [Architecture](docs/ARCHITECTURE.md) | System boundaries, ownership, and execution topology |
| [Workflow](docs/WORKFLOW.md) | State transitions, gates, and lifecycle behavior |
| [Governance](docs/GOVERNANCE.md) | Authority, controls, and exceptions |
| [Artifacts](docs/ARTIFACTS.md) | Artifact contracts and provenance |
| [Product-level constitution and architecture](docs/ARTIFACTS.md#intent-content) | How Spec & Design uses durable product rules and current architecture context |
| [Stage contracts](docs/STAGE_CONTRACTS.md) | Required inputs, outputs, rules, and handoffs from intent through closure |
| [Agent roles](docs/AGENT_ROLES.md) | Build-ready responsibilities, permissions, and boundaries for each component and human role |
| [Lifecycle schemas](schemas/lifecycle-contracts.schema.json) | Machine-readable invocation, result, artifact, and decision-record contracts |
| [Intent Creation Skill](docs/INTENT_CREATION_SKILL.md) | Purpose, required outcomes, and completion checks for the intent component |
| [Metrics](docs/METRICS.md) | Flow, quality, and outcome measures |
| [Roadmap](docs/ROADMAP.md) | Incremental implementation plan |
| [References](docs/REFERENCES.md) | First-party sources informing the design |
| [Contract decisions](docs/CONTRACT_DECISIONS.md) | Canonical cross-component format, integrity, workflow, and handoff decisions |
| [Learning workflow](docs/LEARNING_WORKFLOW.md) | A safe, practical first-feature walkthrough and learning loop |

## Status

This repository is the canonical, implementation-neutral specification for the lifecycle. An LLM or engineer should be able to build one component from its role and stage contracts, then verify conformance against the shared schemas and acceptance requirements. Runtime, model, framework, packaging, and deployment choices remain in the owning component repository. The roadmap defines completion evidence and production decisions; it is not a live implementation-status report.
