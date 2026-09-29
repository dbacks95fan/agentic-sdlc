# Agent Roles

| Role | Plane | Mission | May do | Must not do |
| --- | --- | --- | --- | --- |
| Intent Creation Skill | Control | Shape and synchronize product intent | Read/update only New Ideas or Backlog; synchronize an authorized handoff request with the Conductor | Interpret downstream columns, independently mutate downstream board state, or modify a handed-off intent |
| Conductor | Control | Route the defined lifecycle and maintain workflow state | Validate transition requests, create assignments, and apply authorized board updates | Author code or alter the frozen intent |
| Spec & Design Agent | Execution | Translate frozen intent into one reviewable specification and proposed execution contract | Inspect target repo; produce sectioned `spec.md` and proposed `work-contract.yaml` | Alter frozen intent, product code, approvals, or workflow state |
| Coding Agent | Execution | Produce a candidate implementation from accepted durable inputs | Work inside assigned worktree; commit candidate code and return independent evidence | Alter frozen intent, policy profile, spec, work contract, approvals, or workflow state |
| Evaluator | Execution | Independently assess candidate code and evidence against frozen inputs | Read candidate code and artifacts, reproduce defined checks, and create an Evaluation Package | Modify candidate code, change frozen inputs, approve work, or update Trello |
| Release and Observation Controls | Control | Gather authorized integration, release, and post-release facts | Execute only approved controls and record evidence | Authorize release, decide closure, or claim product/compliance success |
| Product owner | Control | Make consequential product and delivery decisions | Authorize intent freeze, accept specification, approve integration/release, and close work | Delegate accountability to an agent narrative or green checks |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Workers create assigned artifacts and report evidence; the Conductor owns lifecycle state. A model configured for a plane supports these roles but does not merge their authority or eliminate their required separation. The required interfaces are defined in [Stage Contracts](STAGE_CONTRACTS.md). The Intent Creation Skill's required outcomes are defined in [Intent Creation Skill](INTENT_CREATION_SKILL.md). Model choice is an implementation detail, not a board stage.
