# References

The links below were verified on 2026-09-06. They inform this architecture; they are not treated as mandates, and this repository intentionally documents its own choices where sources differ.

| Organization | First-party source | How it informs this design |
| --- | --- | --- |
| OpenAI | [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) | Humans steer while agents execute; repository-local context, isolated worktrees, legibility, feedback loops, and mechanically enforced invariants. |
| OpenAI | [Symphony orchestration specification](https://openai.com/index/open-source-codex-orchestration-symphony/) | A project-management board can be a control plane while an orchestrator manages agent work. Our Conductor adapts that separation of workflow state from workers. |
| Anthropic | [2026 Agentic Coding Trends Report](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf?hsLang=en) | Treat agentic coding as an SDLC change, concentrating human attention on intent and oversight rather than raw implementation throughput. |
| Google Cloud | [What is agentic coding?](https://cloud.google.com/discover/what-is-agentic-coding) | Guardrails, dependency governance, auditability, and human review before merging are core enterprise concerns. |
| Google Cloud | [Govern your agents](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern?hl=en) | Agent registry, identity, access policy, and request/response audit trails inform the governance model. |
| Microsoft/GitHub | [GitHub Spec Kit](https://github.github.com/spec-kit/) | Durable, structured phases—specification, plan, tasks, implementation—and coding-agent portability support our artifact chain. |
| Microsoft/GitHub | [Spec Kit: Agentic SDD](https://github.github.com/spec-kit/reference/agentic-sdd.html) | Clarification, checklists, analysis, and convergence are useful explicit quality gates rather than one-shot generation. |
| AWS | [Governance scope for agentic AI](https://docs.aws.amazon.com/prescriptive-guidance/latest/govern-architect-agentic-ai/what-needs-to-be-governed.html) | Central visibility, role permissions, quality controls, and lineage support least-privilege execution. |
| AWS | [Agent governance: AWS Well-Architected Agentic AI Lens](https://docs.aws.amazon.com/wellarchitected/latest/agentic-ai-lens/agentsus03.html) | Lifecycle ownership, deployment controls, telemetry, and continuous feedback loops inform production observation. |

## Interpretation notes

These sources agree on durable context, explicit control points, and observability. They do not prescribe this exact Trello-to-backlog-to-worktree flow or its vendor assignments. Those are deliberate local decisions designed to preserve a familiar human workflow while providing immutable, auditable machine inputs.
