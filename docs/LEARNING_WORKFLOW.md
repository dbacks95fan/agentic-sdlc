# Learning Workflow

**Audience:** a product owner and engineer trying the process on a first real feature. This is an example walkthrough, not a source of new workflow rules; use [Workflow](WORKFLOW.md) and [Stage Contracts](STAGE_CONTRACTS.md) for exact requirements.

## Choose a safe first feature

Use one product, one board, one intent, and one small, reversible change. Avoid regulated data, payments, irreversible migrations, and high-impact decisions. Create a Policy & Compliance Profile and record any considered frameworks as applicable or not applicable through an accountable human decision. If the feature raises a new security, compliance, or safety question, pause for human direction.

## Walk the work through

1. **Capture:** describe the desired outcome and why it matters; create a synchronized, versioned intent in `New Ideas`.
2. **Commit:** refine its scope and acceptance criteria in `Backlog`. When the product owner moves it to `Prioritized`, the Conductor freezes the exact intent and policy-profile revisions.
3. **Specify:** on entry to `Spec & Design`, the worker prepares an isolated workspace. The Spec & Design Agent consults the product constitution and architecture, then writes `spec.md` and a proposed work contract. The product owner reviews and accepts both before Execution starts.
4. **Build and evaluate:** the Coding Agent implements only the accepted scope and records check results with its candidate. A separate Evaluator reviews that candidate and evidence.
5. **Decide and learn:** a person approves release, requests an allowed rework, or stops. Record release and health facts, observe whether the outcome was achieved, then make the human closure decision.

## Review the first run

After one feature, ask: Was the intent clear before commitment? Did the spec resolve repository facts and identify open decisions? Did checks and independent evaluation find useful issues? Can the records show what actually reached production and what happened afterward? Choose one improvement supported by observed work and update the canonical contract before changing component implementations.

Keep the evidence small enough for a person to review: intent, accepted spec and work contract, candidate change, evaluation findings, release facts, and observation. Completing the loop honestly, including a stop or rework, is more useful than maximizing automation on the first run.
