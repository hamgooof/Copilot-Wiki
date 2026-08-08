# GH-600: Developing in Agentic AI Systems

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| High-level orientation, not an exam cram guide | Prospective GH-600 candidates | 8 August 2026 |

GH-600 is broader than “how to use GitHub Copilot.” It is aimed at people operating, integrating, supervising and governing production agentic AI systems within software-development workflows, using GitHub as a system of record and control.

The topics in this page set are relevant, but instructions, skills and custom agents are only part of the exam scope.

## Current skills measured

Microsoft's study guide lists these domains:

| Domain | Weight |
| --- | ---: |
| Prepare agent architecture and SDLC processes | 15–20% |
| Implement tool use and environment interaction | 20–25% |
| Manage memory, state and execution | 10–15% |
| Perform evaluation, error analysis, and tuning | 15–20% |
| Orchestrate multi-agent coordination | 15–20% |
| Implement guardrails and accountability | 10–15% |

Because exam objectives change, use the [current Microsoft GH-600 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-600) as the source of truth before booking or final revision.

## How this knowledge base maps to GH-600

| GH-600 area | Relevant pages | What this set covers |
| --- | --- | --- |
| Prepare agent architecture and SDLC processes | [Decision guide](Copilot-Technologies/Choose-the-right-technology.md); [agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) | Selecting boundaries, planning versus execution, specialist roles |
| Implement tool use and environment interaction | [Other technologies](Copilot-Technologies/Other-useful-technologies.md) | Tools, MCP, permissions, environment scope |
| Manage memory, state, and execution | [Context, memory and models](Copilot-Technologies/Context-memory-and-models.md) | Context windows, compaction, durable state and Copilot Memory |
| Perform evaluation, error analysis, and tuning | [To Test](To-Test.md) | Measurable experiments, failure classification and regression testing |
| Orchestrate multi-agent coordination | [Agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) | Isolation, delegation, model routing and conflict risks |
| Implement guardrails and accountability | [Instructions](Copilot-Technologies/Custom-instructions.md); [other technologies](Copilot-Technologies/Other-useful-technologies.md) | Least privilege, hooks, approvals, auditability and deterministic controls |

## Areas requiring deeper study

The following exam objectives go beyond these initial Confluence pages:

- Defining task inputs, outputs and success criteria.
- Separating planning, reasoning and action, including approval gates.
- Agent execution in CI, branch scoping and autonomous pull requests.
- Retries, cancellation, escalation and recovery patterns.
- Short-term, long-term and external memory strategies.
- Drift, stale context and state continuity across tools.
- Evaluation signals, automated scanning, traces and root-cause analysis.
- Multi-agent conflict handling, rollback and lifecycle management.
- Risk-based autonomy, human-in-the-loop design and accountability.

## Recommended study posture

Do not study only file formats and feature names. Be able to explain trade-offs in a production scenario:

- Why should this action be autonomous, approval-gated or prohibited?
- Which tools does the agent need, and what is the least privilege required?
- Where is state stored, how does it expire and how is stale context detected?
- What evidence proves the agent succeeded?
- How are partial failures, retries and cancellation handled?
- How can another person audit decisions and outputs later?
- How do multiple agents avoid duplicate or conflicting changes?

## Practical exercises

1. Create a read-only custom agent and verify its effective tools.
2. Build a task-specific skill with a script and explicit success criteria.
3. Connect a test MCP server with minimal permissions and record tool traces.
4. Delegate independent work to two subagents, then deliberately create and resolve a conflict.
5. Add an approval boundary between plan generation and execution.
6. Create an evaluation set with passing, failing and adversarial prompts.
7. Run a long session, compact it and test whether critical constraints survive.
8. Compare single-agent and multi-agent completion quality, credits, latency and auditability.

## Sources

- [Study guide for Exam GH-600: Developing in Agentic AI Systems](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-600)
- [Microsoft Applied Skills and certification resources](https://learn.microsoft.com/en-us/credentials/)
- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
