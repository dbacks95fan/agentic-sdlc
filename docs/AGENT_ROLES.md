# Agent Roles

| Role | Mission | May do | Must not do |
| --- | --- | --- | --- |
| Intent Creation Skill | Shape and synchronize product intent | Read/update pre-freeze intent and Trello projection | Modify an immutable/executing intent |
| Conductor | Route lifecycle work and maintain workflow state | Validate gates, create assignments, update authorized board state, and coordinate authorized publication | Author code, self-certify outcomes, or approve release |
| Spec & Design Agent | Translate frozen intent into an implementable specification | Inspect target repo; produce `spec.md` | Alter frozen intent, skip human design review, or authorize publication |
| Planning Agent | Produce an executable implementation plan | Create `plan.md` and contract inputs | Implement production changes or authorize publication |
| Coding Agent | Make scoped product changes | Work inside assigned worktree; run required checks; attach evidence | Approve its own work, alter workflow state, or authorize publication |
| Validation tooling | Execute deterministic checks | Run schemas, builds, tests, linters, and policy checks | Interpret business intent beyond encoded rules |
| Evaluator | Independently assess delivery against original intent | Read artifacts/code/evidence; produce structured result | Write code, mutate Trello, waive a criterion, or authorize publication |
| Release/Observation automation | Deliver approved changes and surface signals | Deploy through authorized paths; capture health/telemetry | Bypass human approval or hide failed health checks |

All roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. Workers create assigned artifacts and report evidence; the Conductor owns lifecycle state, while the authorized workflow coordination path makes reviewable candidates available only after their publication gates. Publication is not approval or release authority. Model choice is an implementation detail: the current design intentionally uses Claude for coding and Codex for independent evaluation, while preserving vendor-neutral artifacts and contracts.
