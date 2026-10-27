# Agentic SDLC

Canonical, vendor-neutral documentation for an intent-driven software delivery lifecycle using Trello, Git, and bounded AI agents. The model is **Kanban for flow, XP for quality, humans for consequential judgment, and AI for bounded execution**.

## Start here

- **People:** read the [Executive Summary](docs/EXECUTIVE_SUMMARY.md), then the [Learning Workflow](docs/LEARNING_WORKFLOW.md) for a first-feature walkthrough. These explain the process; they do not add requirements.
- **Component builders:** start with [Agent Roles](docs/AGENT_ROLES.md), follow the linked [Stage Contracts](docs/STAGE_CONTRACTS.md), and validate records against the [Lifecycle Schemas](schemas/lifecycle-contracts.schema.json). The [Workflow](docs/WORKFLOW.md), [Artifacts](docs/ARTIFACTS.md), [Architecture](docs/ARCHITECTURE.md), and [Governance](docs/GOVERNANCE.md) define the other binding rules.

## The lifecycle

```text
New Ideas -> Backlog -> Prioritized -> Spec & Design -> Execution -> Evaluation
                                                            -> Human Approval -> Release
                                                            -> Production Observation -> Done
```

Trello shows the human workflow. The dedicated `intent-backlog` repository stores canonical intent; product repositories store product baselines and engineering work. The Conductor owns workflow state and routing. Narrow, stateless agents work from validated inputs. People make priority, specification acceptance, policy applicability, release, and closure decisions. Tests and agent reports provide evidence; they do not prove the intended outcome was delivered.

## Documentation authority

The Executive Summary, README, Learning Workflow, and Agent Roles index orient readers; they do not add requirements. The [Intent Creation Skill contract](docs/INTENT_CREATION_SKILL.md), [Architecture](docs/ARCHITECTURE.md), [Workflow](docs/WORKFLOW.md), [Stage Contracts](docs/STAGE_CONTRACTS.md), [Artifacts](docs/ARTIFACTS.md), [Governance](docs/GOVERNANCE.md), [Lifecycle Schemas](schemas/lifecycle-contracts.schema.json), and [Trello template](templates/trello-board-workflow.yaml) define binding requirements within their stated scope. When an explanation conflicts with a contract, follow the contract and correct the explanation.

The [Metrics](docs/METRICS.md), [Roadmap](docs/ROADMAP.md), [Contract Decisions index](docs/CONTRACT_DECISIONS.md), and [References](docs/REFERENCES.md) provide measures, adoption sequencing, navigation, and rationale. This repository defines the SDLC, not agent runtimes, models, tools, packaging, or deployment.
