# Agent Roles

| Role | Mission | May do | Must not do |
| --- | --- | --- | --- |
| Intent Creation Skill | Shape and synchronize product intent | Read/update only New Ideas or Backlog; freeze and hand off a Backlog card to a user-selected destination | Interpret downstream columns, alter a handed-off card, or modify its intent |
| Conductor | Route the defined lifecycle and maintain workflow state | Validate the current column transition, create assignments, and update authorized board state | Author code or alter the frozen intent |
| Spec & Design worker | Translate frozen intent into an implementable specification | Inspect target repo; produce `spec.md` | Alter frozen intent or workflow state |
| Execution worker | Carry out work from the frozen intent and `spec.md` | Work inside assigned worktree; attach evidence when applicable | Alter frozen intent or workflow state |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Workers create assigned artifacts and report evidence; the Conductor owns lifecycle state. Model choice is an implementation detail, not a board stage.

The Intent Creation Skill's purpose, required outcomes, and completion checks are described in [Intent Creation Skill](INTENT_CREATION_SKILL.md).
