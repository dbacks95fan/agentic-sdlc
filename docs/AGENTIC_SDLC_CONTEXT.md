# Agentic SDLC Context

## Purpose

The goal is not merely to automate coding. Agentic execution makes implementation cheaper and faster; human attention, prioritization, and judgment become the limiting resources. This SDLC therefore optimizes intent quality, flow, traceability, independent evaluation, governance, and outcome learning.

## Core thesis

**Kanban for flow. XP for quality. Humans for consequential judgment. AI for execution.**

We adapt rather than reinvent: durable specifications, isolated workspaces, automated validation, independent review, observability, and explicit approval gates. Repository-local knowledge and enforceable controls should replace fragile chat-only or prompt-only rules.

## Non-negotiable decisions

- The **Intent Creation Skill** turns ideas into reviewable, versioned intent and keeps Trello and `intent.md` synchronized.
- Trello is the human-facing workflow. Git is the versioned record for durable artifacts.
- Product intent lives in a dedicated `intent-backlog` repository, separate from product source repositories.
- Each intent belongs to one product context and has a stable product-prefixed ID, such as `INT-MF-0042`.
- A Trello move to **Prioritized** records the exact intent commit and hash. That revision is immutable once the first engineering agent begins.
- The first engineering agent creates an isolated branch/worktree in the target product repository and copies that exact frozen intent into it.
- A separate Spec & Design stage produces `spec.md`, followed by human Design Review. A separate planning stage produces `plan.md` and a work contract.
- A Claude-oriented Coding Agent implements the approved plan and produces evidence. Deterministic validation runs independently of the agent’s self-report.
- A separate Codex-oriented Evaluator assesses the implementation against the frozen intent, specification, contract, and evidence. It is read-only and does not control board transitions.
- Human Approval precedes Release. Production observation feeds learning back to product refinement.

## Change rule

Clarification and versioning are allowed before the commitment boundary. After execution begins, any material change in outcome, scope, acceptance criteria, constraints, or assumptions creates a new intent/work item and follows a new lifecycle. Do not mutate work beneath active agents.

## Terminology

- **Intent:** the product outcome, boundaries, and acceptance criteria to be achieved.
- **Product context:** the single product namespace owning an intent, its Trello location, and target repository information.
- **Work item:** the execution lifecycle created from a frozen intent revision.
- **Conductor:** the workflow state machine and router; not an implementation or evaluation agent.
- **Evidence:** reproducible validation outputs, review facts, and links to artifacts—not a declaration of success.
