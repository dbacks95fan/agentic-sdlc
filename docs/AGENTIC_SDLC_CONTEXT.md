# Agentic SDLC Context

## Purpose

The goal is not merely to automate coding. Agentic execution makes implementation cheaper and faster; human attention, prioritization, and judgment become the limiting resources. This SDLC therefore optimizes intent quality, flow, traceability, and bounded execution.

## Core thesis

**Kanban for flow. XP for quality. Humans for consequential judgment. AI for execution.**

We adapt rather than reinvent: durable specifications, isolated workspaces, and enforceable controls. Repository-local knowledge and mechanical checks should replace fragile chat-only or prompt-only rules.

In this SDLC, **XP for quality** means practical engineering habits within each small slice of work: make acceptance criteria executable where feasible, write and run focused tests, integrate frequently, refactor only with evidence, and keep a working change small enough to understand. It does not mean adding ceremony, mandatory pair programming, or a separate board stage. Tests are evidence about the candidate; independent evaluation, human judgment, release checks, and observation still matter.

## Non-negotiable decisions

- The **Intent Creation Skill** creates the initial card in `New Ideas` and synchronizes the canonical, versioned `intent.md` and selected policy-profile ID/version. It does not own downstream transitions or freeze state.
- Trello is the human-facing workflow. Git is the versioned record for durable artifacts.
- Product intent lives in a dedicated `intent-backlog` repository, separate from product source repositories.
- Each intent belongs to one product context and has a stable product-prefixed ID, such as `INT-MF-0042`.
- A Trello move to **Prioritized** records and freezes the exact intent and policy-profile references. If the card returns to **Backlog or New Ideas**, the Conductor revokes the active freeze while preserving its history; the intent can then be revised as a new Git version, even if Spec & Design has started.
- The first engineering agent creates an isolated branch/worktree in the target product repository and copies the exact frozen intent and policy profile into it before it starts Spec & Design.
- **Spec & Design** produces `spec.md` from the frozen intent.
- **Execution** begins from the frozen intent, accepted `spec.md`, and frozen work contract. The Coding Agent creates a candidate implementation and evidence.
- **Evaluation** is independent of implementation. An Evaluator reviews the candidate and evidence against the frozen inputs.
- **Human Approval** is the consequential decision to accept the evaluated candidate for release, request rework, or stop the work.
- **Release**, **Production Observation**, and **Done** complete the delivery loop with a release record, outcome evidence, and a human closure decision.

## Change rule

Clarification and versioning are allowed before the Prioritized commitment boundary. A return from Prioritized to Backlog or New Ideas revokes the active freeze and allows a new intent revision while retaining the previous Git history and freeze record, even if Spec & Design has started. A subsequent move to Prioritized creates a new freeze tuple for the new revision. Do not rewrite prior Git revisions or freeze records.

## Terminology

- **Intent:** the product outcome, boundaries, and acceptance criteria to be achieved.
- **Product context:** the single product namespace owning an intent, its Trello location, and target repository information.
- **Work item:** the execution lifecycle created from a frozen intent revision.
- **Conductor:** the workflow state machine and router; not an execution worker.
- **Evidence:** reproducible validation outputs, review facts, and links to artifacts—not a declaration of success.
- **Candidate:** the immutable commit and evidence produced by Execution; it is not approved or released merely because checks passed.
- **Evaluation Package:** an independent assessment of the candidate against the frozen inputs and evidence.
- **Release Record:** the durable record of the approved candidate's delivery, health checks, and rollback facts.
- **Observation Record:** the defined post-release evidence, findings, and follow-up decision.
