# Architecture

**Audience:** SDLC maintainers and component builders. This document is normative for system boundaries, ownership, and plane interfaces; detailed stage behavior is in [Stage Contracts](STAGE_CONTRACTS.md), workflow transitions in [Workflow](WORKFLOW.md), and record shapes in the [Lifecycle Schemas](../schemas/lifecycle-contracts.schema.json).

## System boundaries

```text
                         CONTROL PLANE
People <-> Trello <-> Intent Creation Skill + Conductor <-> intent-backlog
                                              |              |
                                              +-- durable workflow and decision records
                                          |
                          immutable, validated handoff contract
                                          v
                         EXECUTION PLANE
              assigned agent runners <-> isolated product-repo worktrees
                                          |
                                          +-- spec, candidate, and evidence
```

## Two-plane operating model

The **control plane** makes the lifecycle legible and controlled. It owns the Trello projection, intent synchronization and freeze validation, stage routing, durable handoff records, and presentation of decisions to accountable humans. The Conductor is its stateful coordinator; it has read-only product-repository access for handoff validation and does not create, check out, commit to, push, or merge product-repository branches. Humans retain authority for priority, specification acceptance, release approval, policy applicability, and closure.

The **execution plane** consumes only validated, frozen inputs and produces bounded engineering artifacts. It contains the Spec & Design Agent, Coding Agent, and independent Evaluator working in assigned isolated workspaces. It cannot move cards, revise an intent, grant approval, or alter a release decision.

The boundary is a durable contract rather than a chat handoff. Control-plane-to-execution-plane messages identify immutable input revisions, the assigned work branch, the expected commit, and permissions. Execution-plane-to-control-plane messages identify status, pushed commit and artifact references, and observed evidence. The Conductor validates those records before any route changes.

The shared message and record shapes are in [`schemas/lifecycle-contracts.schema.json`](../schemas/lifecycle-contracts.schema.json). The Trello adapter validates a `BoardTransitionEvent`; the Conductor then emits a `StageInvocation` containing the work identity, immutable inputs, target revision, assigned branch, and explicit read/write paths. The worker returns a correlated `StageResult` and the stage-specific durable artifact. The Conductor verifies schema version, invocation identity, pushed commit and artifact references, reachability, and hashes using read-only repository access, then writes a `StageResultDecision` before applying any route. It does not mutate repository refs. A component may use a different internal request format, but its adapter must preserve this logical contract at the boundary.

## LLM assignment

Use a separately configurable LLM assignment for each plane: one control-plane LLM and one execution-plane LLM. Any capable provider or model may fill either assignment. The assignments are operational configuration, not Trello columns, identity, or authority. Record the provider/model/version used for a run in the appropriate audit or evidence record when available, but do not let that metadata substitute for artifact lineage.

Using different LLMs supports separation of concerns and makes it easier to compare behavior, but it is not a safety control by itself. The execution plane has one configurable default LLM; the Evaluator may use an explicitly configured override when risk and evidence justify it. The Evaluator remains independent through its role, separate invocation, frozen inputs, and read-only access to candidate code; it may write only its assigned Evaluation Package.

| Component | Owns | Does not own |
| --- | --- | --- |
| Configured Trello board | Human-visible flow and summaries for one product | Canonical artifact contents or Conductor policy |
| Intent Creation Skill | Refinement and synchronization contract | Engineering execution while a freeze is active |
| `intent-backlog` | Canonical evolving product intent and revision history | Product code or execution evidence |
| Conductor | State transitions, routing, bounded retries, board updates, and read-only handoff validation | Product-repository writes, branch management, and merges |
| Target product repository | Branch-scoped engineering artifacts, code, candidate, and evidence; assigned agents commit and push to the work branch | Product intent or workflow state |
| Spec & Design Agent | Produce `spec.md` from a frozen intent | Alter the frozen intent |
| Coding Agent | Carry out assigned work from durable inputs | Mutate frozen inputs or workflow state |
| Evaluator | Independently assess an immutable candidate and evidence | Modify code, approve, or release work |
| Release and Observation Controls | Perform authorized integration after approval and record delivery and outcome facts | Decide product intent, approval, or compliance applicability |
| Humans | Priority and consequential judgment | Routine deterministic work |

## Product, board, and intent identity

The intent backlog is namespaced by product. A lightweight product registry supplies product ID/name, the assigned Trello board ID, the board's explicit mapping from canonical lifecycle stages to Trello list IDs, target repository, authorized product-owner identities, and policy-profile paths. Product IDs are unique and stable within the registry. An intent ID is stable, unique within its product, includes that product's identifying prefix, and is never reused; its precise textual format is configured rather than parsed as workflow state. Intent versions are positive integers beginning at `1` and increasing by one. Each product entry validates against `ProductConfig` in the [lifecycle schemas](../schemas/lifecycle-contracts.schema.json). Example:

```text
products/mealflow/product.yaml
products/mealflow/policy-profiles/MEALFLOW-DEFAULT.md
products/mealflow/intents/INT-MF-0042/intent.md
products/agentic-sdlc/intents/INT-AS-0017/intent.md
```

`product.yaml` identifies the product and its repository and maps every selectable profile ID to an exact path:

```yaml
schema_version: "1.1"
product_id: MF
product_name: MealFlow
trello_board_id: <board-id>
trello_lists:
  new_ideas: <list-id>
  backlog: <list-id>
  prioritized: <list-id>
  spec_design: <list-id>
  execution: <list-id>
  evaluation: <list-id>
  human_approval: <list-id>
  release: <list-id>
  production_observation: <list-id>
  done: <list-id>
target_repository: https://github.com/example/mealflow.git
authorized_product_owner_ids:
  - <trello-member-id>
policy_profiles:
  MEALFLOW-DEFAULT: policy-profiles/MEALFLOW-DEFAULT.md
```

The Conductor uses the configured board ID and explicit `trello_lists` mapping to resolve workflow stages. It does not contain a product-specific board ID, assume a board title, or infer a stage from list names, order, or visual position. Each mapped list ID must exist on the configured board, and each canonical stage must map to exactly one list. A missing, duplicate, or cross-board mapping blocks processing until an authorized owner corrects the configuration. The product's assigned board can use any board title; a greenfield board uses the canonical list names from the [Trello workflow template](../templates/trello-board-workflow.yaml). Existing boards may use other list names when a human explicitly maps each lifecycle stage to a Trello list. The provider integration remains Trello-specific; board agnostic here means the Conductor can be configured for any product's Trello board without product-specific code. See [Board Setup](BOARD_SETUP.md) for creation and binding requirements.

The Conductor validates the board actor against that product's configured authorized product-owner identities for owner-only decisions. Those identity values are not secrets; authentication credentials remain in the component's approved secret store. `policy_profiles` paths must be relative to the product namespace, remain inside the intent-backlog repository, and resolve at the same pinned commit as the intent.

`product.yaml` maps each policy-profile ID to a repository-relative profile path in the same product namespace. The selected profile ID and version are recorded with the intent. The Conductor resolves that mapping and reads `intent.md`, `product.yaml`, and the selected policy profile from the intent's pinned `intent-backlog` commit, then records the source commit and exact-byte fingerprints at Prioritized. The Skill creates the initial Trello card in New Ideas and records the intent and selected profile reference; it does not create, activate, or revoke a freeze. `intent.md` and its Trello card identify the same product, intent, and version, and the card also shows the commit and fingerprint of that revision (see [Artifacts](ARTIFACTS.md)). What an intent must capture is described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). Cross-product initiatives are parent/portfolio intents decomposed into one execution intent per product; one execution intent never spans unrelated product repositories.

### Multiple boards and repository routing

The product registry may contain multiple product configurations. Each configured Trello board ID must resolve to exactly one product configuration. The Conductor selects the product from the verified board ID on the event, then verifies that the card and lists belong to that board. An unknown board, duplicate board mapping, or card/list mismatch is rejected before workflow state changes. Product names, board titles, list names, and visual positions are never routing keys.

The intent-backlog GitHub repository is configured once for the SDLC installation. The resolved product configuration determines the product namespace inside that repository, where the canonical intent and its version history are stored. `target_repository` in a product configuration is the product's application-code repository, not the intent-backlog repository. These repository roles must not be conflated.

All stage agents are product- and board-neutral workers. The Conductor passes the resolved product ID, target repository, immutable intent/profile references, and assigned work branch or base revision in the invocation. An agent must not contain a MealFlow, PerioParrot, board ID, board title, or repository constant, and must not independently search Trello or choose a product repository. The same agent definition can serve work originating on any registered board.

## Execution workspace and Git handoff

Before the initial Spec & Design invocation, the Conductor retrieves the frozen intent and policy profile from their recorded `intent-backlog` revisions using its read-only access, verifies their exact-byte fingerprints, and supplies the verified bytes as immutable workspace inputs. The first engineering worker creates the work branch from the target repository's assigned base commit, verifies the supplied snapshots against the freeze references, and commits and pushes only its assigned artifacts, including the frozen copies. Stage agents do not access or fetch from `intent-backlog`. Each later worker starts with a fresh isolated checkout of the same branch at the exact commit SHA supplied in its invocation and independently verifies the frozen copies against the recorded fingerprints; it does not depend on another agent's local files or chat history. Only one worker may write to the branch at a time, and pushes must advance the assigned branch without force-pushing.

The Evaluator checks the immutable candidate SHA read-only. It may publish only its assigned Evaluation Package, which must identify that candidate SHA and must not modify candidate code. The Conductor records and validates the returned refs but never creates, checks out, commits to, pushes, or merges product-repository branches. The authorized integration path merges only after Evaluation passes and the accountable human approves the named candidate for release.

A suggested durable layout is:

```text
.agent/work/INT-MF-0042/
  intent.md
  policy-profile.md
  spec.md
  work-contract.yaml
  evidence-package/
  evaluation-package/
  release-record/
  observation-record/
```

The frozen intent, accepted `spec.md`, frozen work contract, candidate, and later evidence form one traceable delivery chain across commits on the work branch. Each handoff records the exact commit SHA so a new device can fetch the branch and reproduce the assigned state. Artifact details are defined in [Artifacts](ARTIFACTS.md) and [Stage Contracts](STAGE_CONTRACTS.md).

## GitHub authentication

GitHub access has two separate credential boundaries. The `intent-backlog` repository is accessed only with its dedicated access tokens: the Orchestrator uses read-only access to resolve and verify frozen intents, while the Intent Creation Skill uses read/write access to create and version intents. Spec & Design, Coding, and Evaluator receive verified frozen snapshots through their stage handoffs and do not access or fetch from `intent-backlog`. The GitHub App is not installed on `intent-backlog`.

Product-code repositories use GitHub App installation tokens. Each invocation receives a token limited to the assigned repository and the minimum permission required: Spec & Design and Coding need contents read/write to create and update their work; the Evaluator needs contents read/write to publish its evaluation package without changing candidate code; the Orchestrator needs read-only access for repository and handoff checks. GitHub App permissions are repository-level; repository branch protection and workflow rules govern which branches can be changed. Runtime secrets must stay out of requests, Trello, artifacts, logs, and source control. Tokens injected as container environment variables can be inspected by principals with Docker daemon access, so restrict that access. An agent never receives the intent-backlog write token unless its role is explicitly authorized to write intents. See [Governance](GOVERNANCE.md) for the access matrix.

## Design constraints

Agents remain stateless between runs; durable state belongs in versioned artifacts and Conductor-managed workflow records. Grant least privilege by role. Enforce critical architecture, validation, and policy invariants mechanically where possible. Record facts, inferences, and unresolved decisions separately. LLMs may assist either plane, but they do not own state transitions, override a deterministic validation failure, or receive authority from their model identity. The Conductor retries only idempotent, clearly transient control-plane or runner failures; it escalates unknown side effects or incomplete execution state to a human.

## Scope boundary

This repository defines lifecycle stages, role boundaries, artifact controls, and governance outcomes. Agent and skill runtime, packaging, deployment, credential mechanics, and implementation-specific recovery belong in the responsible component repository. Those choices must satisfy the lifecycle controls defined here without changing them.
