# Agent Roles

| Role | Mission | May do | Must not do |
| --- | --- | --- | --- |
| Intent Creation Skill | Shape and synchronize product intent | Read/update only New Ideas or Backlog; freeze and move a Backlog card to an explicitly selected handoff destination | Interpret downstream columns, move an already handed-off card, or modify its intent |
| Conductor | Route the defined lifecycle and maintain workflow state | Validate the current column transition, create assignments, and update authorized board state | Author code or alter the frozen intent |
| Spec & Design Agent | Translate frozen intent into one reviewable specification and proposed execution contract | Inspect target repo; produce sectioned `spec.md` and proposed `work-contract.yaml` | Alter frozen intent, product code, approvals, or workflow state |
| Coding Agent | Produce a candidate implementation from accepted durable inputs | Work inside assigned worktree; commit candidate code and return independent evidence | Alter frozen intent, policy profile, spec, work contract, approvals, or workflow state |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Workers create assigned artifacts and report evidence; the Conductor owns lifecycle state. The required interfaces are defined in [Stage Contracts](STAGE_CONTRACTS.md). Model choice is an implementation detail, not a board stage.
