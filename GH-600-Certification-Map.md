# GH-600: turn Copilot experience into exam-ready knowledge

_For prospective GH-600 candidates; this is an orientation, not an exam dump · Last reviewed 11 August 2026_

GH-600 is broader than “how do I use GitHub Copilot in VS Code?” It asks whether you can design, operate, evaluate and govern agentic AI systems through a software-delivery lifecycle.

Knowing what a skill or custom agent is will help. The exam also expects you to reason about autonomy, environments, CI, state, evaluation evidence, coordination and guardrails.

[[_TOC_]]

## What the exam covers

Microsoft's current study guide lists six domains:

| Domain | Weight | The practical question behind it |
| --- | ---: | --- |
| Prepare agent architecture and SDLC processes | 15–20% | Is this a suitable agent task, and where does it belong in delivery? |
| Implement tool use and environment interaction | 20–25% | Which tools, environment and permissions let it work safely? |
| Manage memory, state, and execution | 10–15% | What must persist, and how does execution resume or recover? |
| Perform evaluation, error analysis, and tuning | 15–20% | What evidence shows success or explains failure? |
| Orchestrate multi-agent coordination | 15–20% | How should agents divide work without duplicating or conflicting? |
| Implement guardrails and accountability | 10–15% | Which controls constrain action and preserve an audit trail? |

Use the [current Microsoft GH-600 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-600) as the source of truth before booking or revising. Domain wording and weights can change.

## How this wiki maps to the exam

| GH-600 area | Start here | What this wiki currently covers |
| --- | --- | --- |
| Prepare agent architecture and SDLC processes | [Choose the right technology](Copilot-Technologies/Choose-the-right-technology.md) | Selecting boundaries, planning versus execution and specialist roles |
| Implement tool use and environment interaction | [Other useful technologies](Copilot-Technologies/Other-useful-technologies.md) | Tools, MCP, permissions and environment scope |
| Manage memory, state, and execution | [Context, memory and models](Copilot-Technologies/Context-memory-and-models.md) | Context windows, compaction, durable state and Copilot Memory |
| Perform evaluation, error analysis, and tuning | [To Test](To-Test.md) | Controlled experiments, failure classification and regression evidence |
| Orchestrate multi-agent coordination | [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) | Isolation, delegation, model routing and coordination risks |
| Implement guardrails and accountability | [Custom instructions](Copilot-Technologies/Custom-instructions.md) and [other technologies](Copilot-Technologies/Other-useful-technologies.md) | Least privilege, hooks, approvals and deterministic controls |

This is a useful foundation, not complete exam coverage.

## Artefacts worth recognising on sight

Definitions alone are not enough. Practise opening an unfamiliar repository and explaining what these artefacts are likely to do:

| Artefact | What you should be able to identify |
| --- | --- |
| `.github/copilot-instructions.md` | Repository-wide Copilot instructions |
| `.github/instructions/*.instructions.md` | Instructions scoped by path patterns |
| `AGENTS.md` | Repository guidance for supporting agents; discovery varies by surface |
| `.github/prompts/*.prompt.md` | Reusable prompt files |
| `.github/agents/*.agent.md` or other supported custom-agent filenames | A specialist agent definition, its purpose and allowed tools |
| `.github/skills/<skill>/SKILL.md` | A task-specific skill and its activation description |
| `.github/hooks/*.json` | Commands attached to supported Copilot lifecycle events in Copilot CLI and the cloud coding agent |
| `.github/workflows/copilot-setup-steps.yml` | Environment preparation for Copilot coding agent |
| MCP configuration | Which external servers, tools, transport and credentials are involved |
| Workflow, pull-request and audit logs | Evidence of what ran, changed, failed, passed or was approved |

For each artefact, ask four questions:

1. What capability or guidance does it add?
2. Where and when does it apply?
3. What permission or risk does it introduce?
4. What evidence would prove that it behaved correctly?

## A useful community workbook

The [GH-600 Public Study Guide by naim149](https://gist.github.com/naim149/a8aa41c7468685b7d984822c38863aae) is a substantial community-authored workbook organised around the six official domains. Its most useful qualities are:

- It translates abstract objectives into GitHub artefacts such as YAML, Markdown, CLI output, pull-request timelines and audit logs.
- It includes implementation-shaped examples rather than stopping at definitions.
- It highlights domain traps and supplies self-check questions.
- It encourages least privilege, reviewable outputs and GitHub-native evidence.

Treat it as a **study companion**, not an authority or exam dump. It is not maintained by GitHub or Microsoft, and individual syntax or product claims can drift. Use its scenarios and questions, then verify answers against the linked official documentation.

One especially useful study pattern from the workbook is:

```text
Official domain objective
        ↓
Recognise the implementation artefact
        ↓
Explain inputs, outputs and permissions
        ↓
Identify validation and audit evidence
        ↓
Explain failure handling and human control
```

## What still needs deeper study

These initial wiki pages do not yet cover all GH-600 objectives in enough depth:

- Defining task inputs, outputs and measurable success criteria.
- Separating planning from state-changing execution with approval gates.
- Copilot execution in CI, branch scoping and autonomous pull requests.
- Retries, cancellation, escalation and recovery patterns.
- Short-term, long-term and external memory strategies.
- Drift, stale context and state continuity across tools.
- Evaluation datasets, traces, scans and root-cause analysis.
- Multi-agent conflict handling, rollback and lifecycle management.
- Risk-based autonomy, human-in-the-loop design and accountability.

These should become additional wiki pages rather than being squeezed into the terminology section.

## A practical study route

1. **Build the mental model.** Read [How Copilot actually works](How-Copilot-Works.md) and [One prompt, many rounds](One-Prompt-Many-Rounds.md).
2. **Learn the customization choices.** Compare instructions, prompt files, skills, custom agents, MCP and hooks.
3. **Read real artefacts.** Explain unfamiliar agent YAML, instruction files, skills, workflow files and logs without running them first.
4. **Implement small examples.** Create a read-only reviewer, a bounded skill and a test MCP configuration.
5. **Add controls.** Restrict tools, require an approval boundary and preserve evidence in a pull request or workflow artefact.
6. **Evaluate failures.** Build a small prompt set containing success, failure and adversarial cases; classify why each result occurred.
7. **Coordinate agents.** Delegate independent work, then test duplication, conflicting edits, cancellation and consolidation.
8. **Recheck the official outline.** Close gaps against the current Microsoft study guide before the exam.

## Questions to practise answering

- Why is this task suitable—or unsuitable—for an autonomous agent?
- Which tools are genuinely required, and what is the least privilege needed?
- Where does state live, how long should it persist and how is stale state detected?
- What proves the output is correct beyond the agent saying it succeeded?
- How are timeouts, retries, partial failure and cancellation handled?
- Which action requires human approval, and what enforces that boundary?
- How can another person reconstruct what happened later?
- How do several agents avoid duplicated research or conflicting writes?

If you can answer those questions against a concrete repository, workflow or incident, you are studying the system rather than memorising feature names.

## Sources

- [Official study guide for Exam GH-600: Developing in Agentic AI Systems](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-600)
- [GH-600 Public Study Guide by naim149 — community resource](https://gist.github.com/naim149/a8aa41c7468685b7d984822c38863aae)
- [GitHub Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [GitHub Copilot CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)
- [GitHub custom agents configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Configure the development environment for Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment)
