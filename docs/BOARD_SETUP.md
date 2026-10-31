# Board Setup Contract

**Audience:** product owners and builders of the Conductor setup capability. This document defines the required outcome for creating a Trello board or connecting an existing board to the Agentic SDLC. It does not prescribe a CLI, UI, API, runtime, or deployment.

## Purpose and boundary

The Conductor must be configurable for any registered product's Trello board. It must not contain a board name, board ID, product name, or list ID as a product-specific constant. Each product's `product.yaml` is the durable binding between its identity, its target repository, its Trello board, and that board's lifecycle lists.

Board setup is an explicitly authorized, one-time control-plane setup action. It is not an SDLC stage and does not create intent cards or set work priority. During normal workflow operation, the Conductor validates transitions and updates workflow state only under the rules in [Workflow](WORKFLOW.md).

This contract makes the Conductor board-instance agnostic within the current Trello integration. It does not define adapters for other board providers.

## Supported setup outcomes

Setup must support both:

1. **Create a greenfield board.** Create a board for the selected product from the canonical [Trello workflow template](../templates/trello-board-workflow.yaml). Create exactly one list for each of the ten canonical workflow stages and capture the Trello IDs returned for the board and lists.
2. **Bind an existing board.** Connect a human-selected Trello board and explicitly map each of its lists to the ten canonical workflow stages. List names may differ. The setup capability must not infer a mapping from list position, board name, or similar text.

Additional lists on an existing board may remain outside the SDLC workflow. A list may not be mapped to more than one lifecycle stage, and every canonical stage must map to exactly one list. Sharing one board across multiple products is not defined; reject an ambiguous product-to-board assignment until that behavior is specified.

## Required inputs

The accountable product owner supplies or confirms:

- stable product ID and display name;
- whether to create a new board or bind an existing board;
- for an existing board, the selected board and the explicit list-to-stage mapping;
- target product repository;
- authorized product-owner identities;
- policy-profile ID-to-path mappings;
- the `intent-backlog` product namespace where the resulting `product.yaml` will be reviewed and stored.

Trello credentials and webhook secrets are runtime secrets; they must not be written to `product.yaml`, a card, or this repository.

## Required setup actions

The setup capability must:

1. Require explicit human authorization before creating a board or changing an existing board.
2. For a new board, use the canonical workflow template and preserve its ten named lifecycle lists.
3. For an existing board, apply only the human-confirmed mapping. Do not silently rename, delete, reorder, or move existing cards or lists.
4. Capture the board ID and one distinct list ID for every canonical stage.
5. Validate that the board exists, every mapped list belongs to that board, all ten stage keys are present, and no list ID is reused.
6. Produce a `product.yaml` conforming to the current `ProductConfig` schema, including the explicit board/list mapping, repository, owner identities, and policy-profile paths.
7. Present the proposed product binding and validation results for human review before it becomes active.
8. Make the reviewed binding available to the configured Conductor. The Conductor must register or verify its Trello webhook for the selected board before processing its events; setup must report an unavailable or unverified callback instead of claiming the board is operational.

Creating or binding a board does not authorize work or freeze an intent. Webhook verification is a Conductor integration check; deploying a callback endpoint and building or deploying agents follow their own component contracts and are outside this SDLC board-setup contract.

## Canonical stage keys

The `trello_lists` object in `product.yaml` maps these stable keys to Trello list IDs. Display labels come from the canonical workflow or the explicitly mapped existing lists.

| Stage key | Canonical display label |
| --- | --- |
| `new_ideas` | New Ideas |
| `backlog` | Backlog |
| `prioritized` | Prioritized |
| `spec_design` | Spec & Design |
| `execution` | Execution |
| `evaluation` | Evaluation |
| `human_approval` | Human Approval |
| `release` | Release |
| `production_observation` | Production Observation |
| `done` | Done |

## Product binding and runtime checks

The registry may bind several Trello boards at once, with one product configuration per board. Incoming webhook events are routed by verified board ID to exactly one configuration. Board IDs must be unique in the registry; unknown or ambiguous board IDs, and cards or lists not belonging to the mapped board, block processing. Board titles and matching list names are not routing keys.

`product.yaml` uses `schema_version: "1.1"` and includes:

```yaml
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
```

The full record is defined by `ProductConfig` in the [lifecycle schema](../schemas/lifecycle-contracts.schema.json). It is stored under the product namespace in `intent-backlog` and updated through the repository's normal review process. A setup capability must not activate an incomplete or invalid binding.

`ProductConfig` 1.1 adds a required explicit list-ID mapping. Existing 1.0 product records must be migrated and reviewed before a Conductor version enforcing this contract is enabled. During implementation rollout, do not silently treat a 1.0 record as a valid 1.1 binding.

At startup and before acting on a transition, the Conductor verifies that the event board ID matches exactly one product configuration, that the current card belongs to the configured board and destination list, and that the configured list IDs still belong to that board. Missing, stale, duplicate, or ambiguous configuration blocks processing and asks for human correction; the Conductor must not guess or fall back to list names or board order.

If a board or one of its mapped lists is deleted and recreated, the new Trello IDs must be explicitly rebound and reviewed before the workflow resumes. The configuration must never silently follow a newly created list with the same name.

## Completion evidence

Board binding is complete when the human-reviewed product configuration validates against `ProductConfig`, every canonical stage resolves to exactly one list on the selected board, and a read-only verification confirms the board and list IDs. Board operation is complete only after the configured Conductor verifies its webhook callback. Neither check proves downstream agents are operating correctly.
