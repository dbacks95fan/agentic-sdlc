# Agent Roles

**Audience:** people selecting a component to build and LLMs locating its contract. This is a role index and summary; the linked [Stage Contracts](STAGE_CONTRACTS.md) are normative for triggers, inputs, required actions, outputs, statuses, and handoffs. The [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json) define machine-readable record shapes.

| Role | Plane | Purpose | Authority boundary | Normative contract |
| --- | --- | --- | --- | --- |
| Intent Creation Skill | Control | Capture and synchronize an initial product intent | Creates in `New Ideas`; does not move cards or control freeze state | [Intent Creation Skill](INTENT_CREATION_SKILL.md) and Stage Contracts: Intent Creation |
| Conductor | Control | Maintain lifecycle state and route validated work | Sole workflow-state writer; does not author product code or make human decisions | Stage Contracts: Prioritized Freeze and shared rules; [Workflow](WORKFLOW.md) |
| Spec & Design Agent | Execution | Turn frozen intent and product baselines into reviewed specification artifacts | Writes assigned artifacts; cannot change intent, baselines, workflow state, or accept its own work | Stage Contracts: Spec & Design |
| Coding Agent | Execution | Implement the accepted specification | Writes only assigned product changes and evidence; cannot change accepted inputs, evaluate, approve, merge, or release | Stage Contracts: Execution |
| Evaluator | Execution | Independently assess the candidate and its evidence | Read-only on candidate code; cannot edit, approve, or change workflow state | Stage Contracts: Evaluation |
| Release and Observation Controls | Control | Record authorized integration, delivery, and outcome facts | Performs only authorized controls; cannot approve release or close work | Stage Contracts: Release, Production Observation, and Done |
| Product owner / accountable human | Control | Make product and delivery decisions | Retains priority, policy applicability, spec acceptance, release, and closure decisions | [Governance](GOVERNANCE.md) and relevant Stage Contracts |

## Build a component

1. Select the role above and follow its normative contract link.
2. Read the shared rules and relevant transition behavior in [Stage Contracts](STAGE_CONTRACTS.md) and [Workflow](WORKFLOW.md).
3. Implement the required role behavior and validate every exchanged record against the named definition in the [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json).
4. Use [Governance](GOVERNANCE.md) for decision rights and [Artifacts](ARTIFACTS.md) for provenance and publication requirements.

The component may select its runtime, model, tools, packaging, and deployment within the boundaries of its linked normative contract.
