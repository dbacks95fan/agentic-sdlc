# Agent Roles

| Role | Mission | May do | Must not do |
| --- | --- | --- | --- |
| Intent Creation Skill | Shape and synchronize product intent | Read/update pre-freeze intent and Trello projection | Modify an immutable/executing intent |
| Conductor | Route the defined lifecycle and maintain workflow state | Validate the current column transition, create assignments, and update authorized board state | Author code or alter the frozen intent |
| Spec & Design worker | Translate frozen intent into an implementable specification | Inspect target repo; produce `spec.md` | Alter frozen intent or workflow state |
| Execution worker | Carry out work from the frozen intent and `spec.md` | Work inside assigned worktree; attach evidence when applicable | Alter frozen intent or workflow state |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Workers create assigned artifacts and report evidence; the Conductor owns lifecycle state. Model choice is an implementation detail, not a board stage.
