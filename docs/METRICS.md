# Metrics

Metrics are decision aids, not targets to game. Segment results by product, work type, risk class, and automation level; review trends with qualitative evidence.

| Area | Measure | Why it matters |
| --- | --- | --- |
| Flow | Lead time from Prioritized to Done; time in each state; blocked time; WIP | Reveals queues and constrained human attention |
| Intent quality | Refinement cycles before freeze; post-freeze material-change rate; ambiguity/escalation rate | Tests whether commitment happens with sufficient clarity |
| Specification quality | Clarifications or rework requested after Spec & Design | Tests whether the frozen intent was translated clearly enough for Execution |
| Execution | Time in Execution; blocked execution time; evidence completeness | Reveals where the currently defined execution approach needs improvement |
| Evaluation | Evaluation pass/fail/needs-decision rate; finding recurrence; time to address findings | Tests whether coding evidence is sufficient and independent review is useful |
| Delivery | Approval-to-release time; release failure/rollback rate; health-check completion | Reveals reliability and friction in the authorized release path |
| Outcome | Observation completion rate; expected-outcome evidence; follow-up and incident rate | Connects delivery activity to what happened after release |
| Governance | Freeze-integrity failures; lineage-completeness rate; unauthorized-transition attempts | Shows whether current controls are operating, not merely documented |

Tests, code coverage, and green builds are tracked as execution evidence when applicable. They are not a standalone product-success metric.
