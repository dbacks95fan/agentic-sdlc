# Agent Instructions

This is a vendor-neutral architectural repository. Before proposing or changing the Agentic SDLC, read `docs/AGENTIC_SDLC_CONTEXT.md`, `docs/ARCHITECTURE.md`, `docs/WORKFLOW.md`, `docs/GOVERNANCE.md`, and `docs/REFERENCES.md`.

Preserve these decisions unless a documented deviation has concrete evidence and an explicit tradeoff:

- Trello is the human workflow surface; Git is the durable artifact record.
- The Intent Creation Skill maintains synchronization between a Trello card and its canonical `intent.md`.
- An accepted intent revision is immutable once execution starts; a material change creates a new work item and lifecycle.
- Agents are narrow, stateless workers. The Conductor owns workflow state and routing.
- Coding and evaluation are independent. Tests are evidence, not proof of outcome delivery.
- Humans retain consequential judgment and approval; deterministic controls belong in tooling, not prompts alone.

Do not silently redesign the workflow or assert implementation, test, deployment, or reference facts without verification.
