# Learning Workflow

This is the practical way to use the Agentic SDLC for a first real application feature. It exercises the whole delivery loop without pretending that a personal learning project needs enterprise-scale ceremony or autonomous production deployment.

## Start with a safe feature

Choose a small, reversible outcome with no regulated data, external payment, irreversible migration, or high-impact user decision. Create a product Policy & Compliance Profile anyway and explicitly record that the relevant regulated frameworks are not applicable for this exercise. If the feature introduces one of those conditions, stop and get an accountable human decision before proceeding.

Use one product, one board, one intent, and one branch. Keep the feature small enough that a person can read the intent, `spec.md`, change set, evaluation findings, and observation results in one sitting.

## The walkthrough

| Board column | What happens in this exercise | Who decides or acts | Durable evidence before leaving |
| --- | --- | --- | --- |
| New Ideas | Capture the desired user outcome and why it matters. | Product owner, using the Intent Creation Skill | Initial versioned `intent.md` and matching card |
| Backlog | Clarify scope, exclusions, acceptance criteria, assumptions, and selected policy profile. | Product owner | Revised, synchronized intent |
| Prioritized | Decide it is worth doing now and freeze the exact intent and policy-profile revisions. Returning to Backlog or New Ideas revokes the active freeze and allows a new intent version, even after Spec & Design starts. | Product owner authorizes; Conductor records and moves | Freeze tuple, or a recorded revocation with the prior tuple retained |
| Spec & Design | The first engineering worker creates and verifies the isolated workspace. The Spec & Design Agent then creates `spec.md` plus a proposed work contract. The owner reviews and accepts both. | Worker prepares; agent investigates; human accepts | Spec Acceptance Record binding the accepted artifacts |
| Execution | The Coding Agent builds only the accepted scope in an isolated workspace. It runs the defined checks and produces evidence tied to its candidate commit. | Coding Agent | Immutable candidate SHA and Evidence Package |
| Evaluation | A separate Evaluator checks the candidate against the frozen inputs and evidence. | Evaluator | Evaluation Package with pass, fail, or escalation |
| Human Approval | Review the candidate and evaluation. Approve integration/release, request bounded rework, or stop. | Accountable human | Decision tied to candidate SHA |
| Release | Merge the approved candidate through the authorized integration path, release the resulting revision, and verify basic health. | Authorized integration and release controls with human oversight | Release Record with candidate and integration SHA |
| Production Observation | Watch the agreed signals for a short, defined period and compare them with the intended outcome. | Observation control gathers facts; human interprets | Observation Record and follow-up links |
| Done | Close delivered work after observation, or stopped/rolled-back work with a clear closure reason. | Accountable human | Observation Record or Closure Record |

## What to learn from the first feature

At the end, ask five questions:

1. Did the intent make the desired outcome and exclusions clear before implementation started?
2. Did `spec.md` give the Coding Agent enough confirmed repository context and validation direction?
3. Did deterministic checks and independent evaluation find useful issues?
4. Could the release and observation records show what actually happened, rather than only that a command completed?
5. What one documentation rule, template field, check, or board practice would make the next feature easier or safer?

Capture the answer as a small, versioned improvement to this repository before changing a component implementation. Do not add a board column merely because a problem occurred; first establish its owner, purpose, and exit condition.

## Lightweight quality bar

For an initial feature, the minimum is a clear intent, frozen inputs, an accepted `spec.md`, a branch-scoped candidate, relevant deterministic checks, separate evaluation, human release approval, a verified release fact, and a short observation record. The learning value comes from completing this loop honestly, including failures and rework, not from maximizing automation on the first run.
