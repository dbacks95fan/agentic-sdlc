# Agent Roles

| Role | Plane | Mission | May do | Must not do |
| --- | --- | --- | --- | --- |
| Intent Creation Skill | Control | Create an initial, synchronized product-intent card in New Ideas | Create the card and its canonical intent record with the selected policy-profile reference | Move cards downstream; create, activate, or revoke a freeze; interpret downstream workflow state |
| Conductor | Control | Route the defined lifecycle and maintain workflow state | Validate transition requests, create assignments, and apply authorized board updates | Author code or alter the frozen intent |
| Spec & Design Agent | Execution | Translate frozen intent into one reviewable specification and proposed execution contract consistent with the product baseline | Inspect target repo and consult its product constitution, current architecture, and relevant decisions; produce sectioned `spec.md` and proposed `work-contract.yaml` | Alter frozen intent, product code, product-level baselines, approvals, or workflow state |
| Coding Agent | Execution | Produce a candidate implementation from accepted durable inputs | Work inside assigned worktree; commit candidate code and return independent evidence | Alter frozen intent, policy profile, spec, work contract, approvals, or workflow state |
| Evaluator | Execution | Independently assess candidate code and evidence against frozen inputs | Read candidate code and artifacts, reproduce defined checks, and create an Evaluation Package | Modify candidate code, change frozen inputs, approve work, or update Trello |
| Release and Observation Controls | Control | Gather authorized integration, release, and post-release facts | Execute only approved controls and record evidence | Authorize release, decide closure, or claim product/compliance success |
| Product owner | Control | Make consequential product and delivery decisions | Authorize intent freeze, accept specification, approve integration/release, and close work | Delegate accountability to an agent narrative or green checks |

## Contract authority

This document summarizes the responsibilities. The normative lifecycle, triggers, inputs, outputs, rules, and handoffs are defined in [Stage Contracts](STAGE_CONTRACTS.md), [Workflow](WORKFLOW.md), [Governance](GOVERNANCE.md), and [Artifacts](ARTIFACTS.md). Machine-readable records use [the lifecycle schemas](../schemas/lifecycle-contracts.schema.json). If a summary here conflicts with those documents, the specific stage and governance contracts take precedence; record and resolve the inconsistency before implementation.

An LLM building a component from this repository must implement the role contract and validate records against the named schema. It may choose the runtime, model, tools, packaging, and deployment in the component repository. It must not invent additional board stages, permissions, status meanings, retry behavior, or human decisions.

## Role contracts

### Intent Creation Skill

- **Trigger:** a product owner asks to capture or refine an idea. Creation always begins in `New Ideas`.
- **Inputs:** selected product, product board, the owner's desired outcome and decisions, the selected policy-profile ID/version, and any existing card and intent revision being refined.
- **Responsibilities:** elicit the outcome, scope, acceptance criteria, constraints, and assumptions; keep multi-product work separated into a parent intent and one intent per product; maintain stable product-scoped intent identity and positive integer versions beginning at `1` and increasing by one; commit each canonical revision to the dedicated `intent-backlog` repository; synchronize the card's intent metadata, commit, and fingerprint; report conflicts for owner reconciliation.
- **Outputs:** a reachable committed `intent.md`, matching `New Ideas` card projection, and a clear completion or unresolved-decision result. Required intent content and completion checks are in [Intent Creation Skill](INTENT_CREATION_SKILL.md).
- **Must not:** move a card to `Backlog`, `Prioritized`, or another downstream column; activate or revoke a freeze; infer downstream state; make policy-applicability or compliance decisions; rewrite history.

### Conductor

- **Trigger:** a Trello transition event or an authorized human decision that the workflow says the Conductor applies.
- **Inputs:** schema-valid `BoardTransitionEvent`, product/board/list configuration, immutable artifact references, current lifecycle records, and the applicable human decision where required.
- **Responsibilities:** authenticate and validate the event; resolve actual list IDs to the canonical workflow stages from the configured board; verify required inputs and artifact identity, revision, and hashes; own lifecycle state and Trello projections; create append-only freeze activation/revocation records; dispatch only the agent associated with a documented stage entry; ensure repeat delivery of the same event does not create a duplicate invocation; validate each returned result before applying a permitted route; preserve an audit trail; publish a worker branch when a reachable reference is required by the next handoff.
- **Outputs:** validated workflow transitions recorded on Trello, stage invocations, visible card summaries, freeze records, and a durable `StageResultDecision` for every worker result. The machine-readable record shapes are in [the lifecycle schemas](../schemas/lifecycle-contracts.schema.json).
- **Must not:** author product code or agent work products; alter frozen inputs; treat a worker narrative as approval; make a human decision; infer a missing state transition; merge or release without the required human authorization.

### Spec & Design Agent

- **Trigger:** a valid card entry into `Spec & Design` with an active Prioritized freeze.
- **Inputs:** frozen intent and policy-profile references and hashes, target repository and base revision, isolated workspace, repository instructions, product constitution and architecture, and the work identity.
- **Responsibilities:** verify the frozen inputs and workspace; inspect the target repository; distinguish repository facts, design decisions, assumptions, and unresolved decisions; consult product baselines; produce the required sections of `spec.md` and a proposed schema-valid `work-contract.yaml`; bind both to the frozen intent, profile, target, scope, validation, and release/observation needs; return a defined status.
- **Outputs:** `spec.md`, proposed `work-contract.yaml`, and status `spec_ready`, `needs_decision`, `blocked`, or `failed`.
- **Must not:** change product code or baselines, change frozen inputs, update Trello, accept its own work, or claim legal or compliance satisfaction.

### Coding Agent

- **Trigger:** a valid `Execution` entry authorized by a Spec Acceptance Record.
- **Inputs:** exact frozen intent and policy profile, accepted spec and work contract, target/base revision, assigned isolated workspace and permitted scope, repository instructions, required deterministic checks, and any authorized bounded rework findings.
- **Responsibilities:** verify every reference and hash; change only permitted product files; follow the accepted behavior and scope; use repository-declared validation commands and report each observed result; create a candidate commit on the assigned work branch; produce a schema-valid Evidence Package tied to that candidate.
- **Outputs:** immutable candidate commit reference and Evidence Package with status `candidate_complete`, `needs_decision`, `blocked`, or `failed`.
- **Must not:** change intent, profile, spec, work contract, approvals, or Trello; select new product behavior; publish the branch when the workflow assigns publication to the Conductor; evaluate or approve its own candidate; merge or release.

### Evaluator

- **Trigger:** a valid `Evaluation` entry with reachable candidate and Evidence Package.
- **Inputs:** frozen inputs, accepted spec and work contract, immutable candidate reference, Evidence Package, repository evaluation instructions, and the work identity.
- **Responsibilities:** independently check the candidate against the accepted requirements and policy controls; reproduce applicable deterministic checks; verify evidence and candidate identity; record observed facts separately from conclusions; identify findings with a category and severity; return a schema-valid Evaluation Package.
- **Outputs:** Evaluation Package with status `pass`, `fail`, `needs_decision`, `blocked`, or `failed`.
- **Must not:** modify candidate files or frozen inputs, waive a required control, update Trello, decide on release, or claim legal or compliance satisfaction.

### Release and Observation Controls

- **Trigger:** an approved candidate enters `Release`; a `released` Release Record enters Production Observation.
- **Inputs:** immutable candidate, human approval, product's authorized integration/release path, release requirements, health-check requirements, observation window, and product-appropriate signals.
- **Responsibilities:** perform only the authorized control actions; record integration and release facts; verify the running product using the defined checks; record rollback facts if applicable; compare post-release observations to the accepted outcome; present closure or follow-up evidence to the accountable human.
- **Outputs:** Release Record, Observation Record when released, and Closure Record for stopped or rolled-back work as required.
- **Must not:** authorize its own release, decide product scope, decide policy applicability, claim legal compliance, or close work without the accountable human's decision.

### Product Owner and Accountable Human

- **Trigger:** a documented decision gate or an unresolved decision returned by a component.
- **Responsibilities:** decide intent and priority, authorize or withdraw commitment, accept the spec and work contract, approve integration/release, resolve escalations, and close work. Each decision identifies the work item, decision, actor, time, rationale where required, and immutable artifact affected.
- **Must not:** treat agent output or passing checks as a substitute for accountable judgment.

All agent roles are narrow and stateless. They receive explicit inputs, produce versioned outputs, and escalate uncertainty rather than inventing missing decisions. A model configured for a plane supports these roles but does not merge their authority or eliminate their required separation. Model choice is an implementation detail, not a board stage.
