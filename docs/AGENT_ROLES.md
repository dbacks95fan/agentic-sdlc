# Agent Roles

| Role | Mission | May do | Must not do |
| --- | --- | --- | --- |
| Intent Creation Skill | Shape and synchronize product intent | Read/update pre-freeze intent and Trello projection | Modify an immutable/executing intent |
| Conductor | Route lifecycle work and maintain workflow state | Validate gates, create assignments, update authorized board state | Author code or self-certify outcomes |
| Spec & Design Agent | Translate frozen intent into an implementable specification | Inspect target repo; produce `spec.md` | Alter frozen intent or skip human design review |
| Planning Agent | Produce an executable implementation plan | Create `plan.md` and contract inputs | Implement production changes |
| Coding Agent | Make scoped product changes | Work inside assigned worktree; run required checks; attach evidence | Approve its own work or alter workflow state |
| Validation tooling | Execute deterministic checks | Run schemas, builds, tests, linters, and policy checks | Interpret business intent beyond encoded rules |
| Evaluator | Independently assess delivery against original intent | Read artifacts/code/evidence; produce structured result | Write code, mutate Trello, or waive a criterion |
| Release/Observation automation | Deliver approved changes and surface signals | Deploy through authorized paths; capture health/telemetry | Bypass human approval or hide failed health checks |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Model choice is an implementation detail: the current design intentionally uses Claude for coding and Codex for independent evaluation, while preserving vendor-neutral artifacts and contracts.
