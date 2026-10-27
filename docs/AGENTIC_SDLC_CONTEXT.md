# Agentic SDLC Context

**Audience:** people learning or reviewing the design. This document records the rationale and principles; it does not restate operational requirements. Follow the linked normative documents for exact behavior.

## Purpose

Agentic execution can reduce the cost of implementation, which makes clear intent, prioritization, and human judgment more important. The lifecycle is designed to make work traceable, keep execution bounded, and direct human attention to decisions that need it.

## Design principles

- **Kanban for flow:** make work visible and improve the system from observed queues and outcomes.
- **XP for quality:** keep changes small, write and run focused tests, integrate frequently, and refactor with evidence. This does not add mandatory pairing or a board stage.
- **Humans for consequential judgment:** people decide priority, policy applicability, specification acceptance, release, and closure.
- **AI for bounded execution:** agents have narrow roles and act only on explicit, validated inputs.
- **Durable, checkable controls:** Trello presents workflow state; Git stores artifacts; mechanical checks enforce requirements where practical. Tests are evidence, not proof of outcome delivery.

## Authoritative documents

Use [Architecture](ARCHITECTURE.md) for system boundaries; [Workflow](WORKFLOW.md) for stages and transitions; [Stage Contracts](STAGE_CONTRACTS.md) for component inputs, responsibilities, outputs, and statuses; [Artifacts](ARTIFACTS.md) and the [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json) for artifact provenance and record shapes; and [Governance](GOVERNANCE.md) for authority and controls. The [Executive Summary](EXECUTIVE_SUMMARY.md) gives a short human overview.

## Key terms

- **Intent:** the product outcome, boundaries, and acceptance criteria to be achieved.
- **Work item:** the lifecycle created from a frozen intent revision.
- **Conductor:** the stateful workflow coordinator and router, not an execution worker.
- **Candidate:** the immutable implementation revision plus its evidence; it is not approved or released merely because checks passed.
- **Evidence:** reproducible validation results, review facts, and artifact references, not a declaration of success.
