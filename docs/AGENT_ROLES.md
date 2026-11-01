# Agent Roles

**Audience:** people selecting a component to build and LLMs locating its contract. This is a role index and summary; the linked [Stage Contracts](STAGE_CONTRACTS.md) are normative for triggers, inputs, required actions, outputs, statuses, and handoffs. The [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json) define machine-readable record shapes.

| Role | Plane | Purpose | Authority boundary | Normative contract |
| --- | --- | --- | --- | --- |
| Intent Creation Skill | Control | Capture and synchronize an initial product intent | Creates in `New Ideas`; does not move cards or control freeze state | [Intent Creation Skill](INTENT_CREATION_SKILL.md) and Stage Contracts: Intent Creation |
| Conductor | Control | Maintain lifecycle state and route validated work | Sole workflow-state writer; validates Git refs read-only; does not create branches, commit, push, merge, or make human decisions | Stage Contracts: Prioritized Freeze and shared rules; [Workflow](WORKFLOW.md) |
| Board Setup | Control setup | Create a canonical Trello board or bind an existing board to a product configuration | Runs only when explicitly authorized; does not create work items, set priority, or change lifecycle policy | [Board Setup](BOARD_SETUP.md) and [Architecture](ARCHITECTURE.md) |
| Spec & Design Agent | Execution | Turn frozen intent and product baselines into reviewed specification artifacts | Creates the work branch when it is the first engineering worker; commits and pushes only assigned artifacts; cannot change intent, baselines, workflow state, or accept its own work | Stage Contracts: Spec & Design |
| Coding Agent | Execution | Implement the accepted specification | Fetches the assigned handoff SHA and pushes only assigned product changes and evidence; cannot change accepted inputs, evaluate, approve, merge, or release | Stage Contracts: Execution |
| Evaluator | Execution | Independently assess the candidate and its evidence | Reads candidate code at its immutable SHA; may push only its Evaluation Package; cannot alter candidate code, approve, or change workflow state | Stage Contracts: Evaluation |
| Release and Observation Controls | Control | Execute authorized integration and deployment and record delivery and outcome facts | A separately authorized integration workflow or human may merge under the Release contract; controls cannot approve the candidate or production deployment or close work | Stage Contracts: Release, Production Observation, and Done |
| Product owner / accountable human | Control | Make product and delivery decisions | Retains priority, policy applicability, spec acceptance, release, and closure decisions | [Governance](GOVERNANCE.md) and relevant Stage Contracts |

## Cross-board execution rule

All roles are invoked with resolved product context, not tied to a particular board. The Conductor validates an incoming board event and selects the unique registered product configuration; stage agents receive that product ID, its assigned repository, and pinned artifact references. Agents must not hardcode a board, product, or repository, and must not use Trello board titles or list names to choose work.

## Build a component

1. Select the role above and follow its normative contract link.
2. Read the shared rules and relevant transition behavior in [Stage Contracts](STAGE_CONTRACTS.md) and [Workflow](WORKFLOW.md).
3. Implement the required role behavior and validate every exchanged record against the named definition in the [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json).
4. Use [Governance](GOVERNANCE.md) for decision rights and [Artifacts](ARTIFACTS.md) for provenance and publication requirements.

The component may select its runtime, model, tools, packaging, and deployment within the boundaries of its linked normative contract.
