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

The **control plane** makes the lifecycle legible and controlled. It owns the Trello projection, intent synchronization and freeze validation, stage routing, durable handoff records, and presentation of decisions to accountable humans. The Conductor is its stateful coordinator; humans retain authority for priority, specification acceptance, release approval, policy applicability, and closure.

The **execution plane** consumes only validated, frozen inputs and produces bounded engineering artifacts. It contains the Spec & Design Agent, Coding Agent, and independent Evaluator working in assigned isolated workspaces. It cannot move cards, revise an intent, grant approval, or alter a release decision.

The boundary is a durable contract rather than a chat handoff. Control-plane-to-execution-plane messages identify immutable input revisions and permissions. Execution-plane-to-control-plane messages identify status, immutable output references, and observed evidence. The Conductor validates those records before any route changes.

The shared message and record shapes are in [`schemas/lifecycle-contracts.schema.json`](../schemas/lifecycle-contracts.schema.json). The Trello adapter validates a `BoardTransitionEvent`; the Conductor then emits a `StageInvocation` containing the work identity, immutable inputs, target revision, and explicit read/write paths. The worker returns a correlated `StageResult` and the stage-specific durable artifact. The Conductor verifies schema version, invocation identity, artifact reachability, and hashes, then writes a `StageResultDecision` before applying any route. A component may use a different internal request format, but its adapter must preserve this logical contract at the boundary.

## LLM assignment

Use a separately configurable LLM assignment for each plane: one control-plane LLM and one execution-plane LLM. Any capable provider or model may fill either assignment. The assignments are operational configuration, not Trello columns, identity, or authority. Record the provider/model/version used for a run in the appropriate audit or evidence record when available, but do not let that metadata substitute for artifact lineage.

Using different LLMs supports separation of concerns and makes it easier to compare behavior, but it is not a safety control by itself. The execution plane has one configurable default LLM; the Evaluator may use an explicitly configured override when risk and evidence justify it. The Evaluator remains independent through its role, separate invocation, frozen inputs, and read-only access to candidate code; it may write only its assigned Evaluation Package.

| Component | Owns | Does not own |
| --- | --- | --- |
| Trello | Human-visible flow and summaries | Canonical artifact contents |
| Intent Creation Skill | Refinement and synchronization contract | Engineering execution while a freeze is active |
| `intent-backlog` | Canonical evolving product intent and revision history | Product code or execution evidence |
| Conductor | State transitions, routing, bounded retries, and board updates | Execution conclusions |
| Target product repository | Branch-scoped engineering artifacts, code, candidate, and evidence | Product intent or workflow state |
| Spec & Design Agent | Produce `spec.md` from a frozen intent | Alter the frozen intent |
| Coding Agent | Carry out assigned work from durable inputs | Mutate frozen inputs or workflow state |
| Evaluator | Independently assess an immutable candidate and evidence | Modify code, approve, or release work |
| Release and Observation Controls | Record authorized integration, delivery, and outcome facts | Decide product intent, approval, or compliance applicability |
| Humans | Priority and consequential judgment | Routine deterministic work |

## Product and intent identity

The intent backlog is namespaced by product. A lightweight product registry supplies product ID/name, Trello board ID, target repository, authorized product-owner identities, and policy-profile paths. Product IDs are unique and stable within the registry. An intent ID is stable, unique within its product, includes that product's identifying prefix, and is never reused; its precise textual format is configured rather than parsed as workflow state. Intent versions are positive integers beginning at `1` and increasing by one. Each product entry validates against `ProductConfig` in the [lifecycle schemas](../schemas/lifecycle-contracts.schema.json). Example:

```text
products/mealflow/product.yaml
products/mealflow/policy-profiles/MEALFLOW-DEFAULT.md
products/mealflow/intents/INT-MF-0042/intent.md
products/agentic-sdlc/intents/INT-AS-0017/intent.md
```

`product.yaml` identifies the product and its repository and maps every selectable profile ID to an exact path:

```yaml
schema_version: "1.0"
product_id: MF
product_name: MealFlow
trello_board_id: <board-id>
target_repository: https://github.com/example/mealflow.git
authorized_product_owner_ids:
  - <trello-member-id>
policy_profiles:
  MEALFLOW-DEFAULT: policy-profiles/MEALFLOW-DEFAULT.md
```

The Conductor resolves the configured board's actual Trello list IDs by exact match to the canonical names in [Workflow](WORKFLOW.md) and the board template; it does not infer a stage from a list's visual position. Missing, duplicate, or unknown required names stop the workflow until a human corrects the board/configuration. It validates the board actor against that product's configured authorized product-owner identities for owner-only decisions. Those identity values are not secrets; authentication credentials remain in the component's approved secret store. `policy_profiles` paths must be relative to the product namespace, remain inside the intent-backlog repository, and resolve at the same pinned commit as the intent.

`product.yaml` maps each policy-profile ID to a repository-relative profile path in the same product namespace. The selected profile ID and version are recorded with the intent. The Conductor resolves that mapping and reads `intent.md`, `product.yaml`, and the selected policy profile from the intent's pinned `intent-backlog` commit, then records the source commit and exact-byte fingerprints at Prioritized. The Skill creates the initial Trello card in New Ideas and records the intent and selected profile reference; it does not create, activate, or revoke a freeze. `intent.md` and its Trello card identify the same product, intent, and version, and the card also shows the commit and fingerprint of that revision (see [Artifacts](ARTIFACTS.md)). What an intent must capture is described in [Intent Creation Skill](INTENT_CREATION_SKILL.md). Cross-product initiatives are parent/portfolio intents decomposed into one execution intent per product; one execution intent never spans unrelated product repositories.

## Execution workspace

The worktree is created only from a frozen input. A suggested durable layout is:

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

The frozen intent, accepted `spec.md`, frozen work contract, candidate, and later evidence form one traceable delivery chain. Artifact details are defined in [Artifacts](ARTIFACTS.md) and [Stage Contracts](STAGE_CONTRACTS.md).

## Design constraints

Agents remain stateless between runs; durable state belongs in versioned artifacts and Conductor-managed workflow records. Grant least privilege by role. Enforce critical architecture, validation, and policy invariants mechanically where possible. Record facts, inferences, and unresolved decisions separately. LLMs may assist either plane, but they do not own state transitions, override a deterministic validation failure, or receive authority from their model identity. The Conductor retries only idempotent, clearly transient control-plane or runner failures; it escalates unknown side effects or incomplete execution state to a human.

## Scope boundary

This repository defines lifecycle stages, role boundaries, artifact controls, and governance outcomes. Agent and skill runtime, packaging, deployment, credential mechanics, and implementation-specific recovery belong in the responsible component repository. Those choices must satisfy the lifecycle controls defined here without changing them.
