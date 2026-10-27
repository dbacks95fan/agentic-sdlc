# Agentic SDLC: Executive Summary

**Audience:** product owners, engineers, and stakeholders. This is an orientation, not an implementation contract. Follow the linked normative documents for exact rules.

## What it is

An intent-driven process for delivering small software changes with AI support. It makes work visible in Trello, preserves decisions and artifacts in Git, and uses bounded agents for repeatable engineering work. **Kanban manages flow; XP practices support quality; people retain consequential judgment.**

## How work moves

```text
Idea -> Backlog -> Prioritized -> Spec & Design -> Execution -> Evaluation
                                                        -> Human Approval -> Release
                                                        -> Production Observation -> Done
```

The product owner creates and refines an intent. `Prioritized` commits the work and freezes the selected intent and policy-profile revisions. Spec & Design consults the product’s constitution and architecture, then produces one human-reviewed `spec.md` and a machine-readable work contract. After the owner accepts them, the Coding Agent builds a candidate. A separate Evaluator checks it. A person decides whether to approve release, request defined rework, or stop. Release and observation records capture what happened; a person closes the work.

## Who owns what

- A configured Trello board shows one product's work state; **Git** preserves intents, specifications, code, and evidence. The Conductor is configured with board and list IDs; it is not tied to a named product or board. A setup capability can create a board from the canonical template or bind an existing board.
- The **Conductor** validates board events, owns workflow state, and routes work.
- **Agents** are narrow workers: they use assigned, validated inputs and return durable results. They do not approve their own work or control the board.
- **People** decide priority, policy applicability, specification acceptance, release, and closure.

Passing checks and agent reports are evidence, not proof of business success or regulatory compliance. Production observation compares the delivered result with the intended outcome.

## Where to go next

- Follow the [Learning Workflow](LEARNING_WORKFLOW.md) to try the process on one low-risk feature.
- Read the [Workflow](WORKFLOW.md) for board states and transitions.
- If building a component, use [Agent Roles](AGENT_ROLES.md), its linked [Stage Contracts](STAGE_CONTRACTS.md), and the [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json). These contracts, not this summary, are authoritative.
