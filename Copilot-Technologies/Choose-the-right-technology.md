# Choose the right Copilot technology

_For new and regular Copilot users - Last reviewed 4 October 2026_

Instructions, skills, prompt files and custom agents can all guide Copilot. Choose based on what you want to reuse.

First ask what you want to reuse; then pick the technology. [Repository knowledge](../Repository-Knowledge.md) is versioned information that several agents and workflows can read. [Spec-Driven Development](../Spec-Driven-Development.md) is a process that can combine planning, files, agents and review points. Neither is another Copilot technology.

![Rules route to instructions, procedures to skills, saved requests to prompt files, worker roles to custom agents, isolated work to subagents and guarantees to deterministic enforcement](../Media/technology-choice.svg =760x)

[[_TOC_]]

## Quick decision guide

1. **A rule or fact should apply to most repository work:** use [repository instructions](Custom-instructions.md)
2. **Guidance applies only to particular files or folders:** use [path-specific instructions](Custom-instructions.md#path-specific-instructions)
3. **Copilot needs a repeatable procedure for one kind of task:** use an [agent skill](Agent-skills.md)
4. **A person should deliberately run the same saved request with new inputs:** use a [prompt file](Prompt-files.md); in VS Code, prefer a skill you invoke by name (see [Prompt file or skill](#prompt-file-or-skill))
5. **Copilot needs a reusable role, model or tool configuration:** use a [custom agent](Custom-agents-and-subagents.md#custom-agent)
6. **A separate worker would help isolate delegated work:** use a [subagent](Custom-agents-and-subagents.md#subagents) where supported
7. **A rule must always be enforced:** use a formatter, analyser, test, permission or repository control

## Comparison

| Customisation | What it reuses | How it starts | Example |
| --- | --- | --- | --- |
| [Repository instructions](Custom-instructions.md) | Shared facts and rules | Applied automatically | Correct build and test commands |
| [Path-specific instructions](Custom-instructions.md#path-specific-instructions) | Rules for matching code | Applied for relevant files | Angular conventions under `src/client` |
| [Skill](Agent-skills.md) | A task procedure and supporting resources | Loaded when relevant or invoked directly | Review branch changes against a checklist |
| [Prompt file](Prompt-files.md) | A saved request | Invoked by a person | Trace callers of a supplied API symbol |
| [Custom agent](Custom-agents-and-subagents.md#custom-agent) | A worker role and configuration | Selected or delegated | Read-focused independent Reviewer |
| [Subagent](Custom-agents-and-subagents.md#subagents) | Isolated delegated work | Invoked by another agent | Investigate one question and return a report |

## Skill or custom agent

This is the distinction most people need:

- A **custom agent defines the worker**: role, continuing instructions, tools, model, subagent access and handoffs
- A **skill defines a playbook**: steps, checklist, examples, scripts, references and output format

One agent can use several skills. The same skill can be reused by different agents. If the model is the engine, a custom agent is a vehicle fitted out for one job, and a skill is a route card any vehicle can pick up when the job matches.

### Scenario A: occasional structured review

In VS Code, enter this slash command in Copilot Chat:

```text
/review-changes develop
```

The skill compares the branches, applies the team checklist and returns Critical, Major and Minor findings. No custom agent is needed because the reusable asset is the procedure.

### Scenario B: dedicated independent reviewer

Start a new chat and select a `Reviewer` custom agent. Its configuration can keep the worker read-focused, use a preferred review model and retain reviewer behaviour during follow-up questions.

That worker can invoke reusable skills such as:

- `security-review`
- `api-contract-review`
- `frontend-quality-review`
- `test-coverage-review`

Here the agent defines **who is working** and the skills define **how particular reviews are performed**.

Do not split every checklist heading into a skill. Start with one `review-changes` skill and separate a section only when it is substantial, reused independently or has its own supporting material.

### Scenario C: ordinary feature work

Use the built-in Plan agent when a cross-cutting change needs investigation, the general Agent for implementation, and a testing skill when the repository has a repeatable test procedure. Start a new chat for an independent review when prior implementation context would bias the check.

Plan, Implement, Test and Review are workflow phases. They do not each need to become a custom agent.

For durable specifications, plans, task splits, human review points and fresh-session options, use the [Spec-Driven Development guide](../Spec-Driven-Development.md). In a client without the documented VS Code controls, use a read-only planning request and save the approved plan explicitly.

## Prompt file or skill

Use a **prompt file** when the person should choose exactly when to run a saved request. It is a convenient named shortcut with inputs.

Use a **skill** when the procedure can be selected automatically during a larger task, or when it carries a fuller workflow with supporting resources.

Both can appear as slash commands in current VS Code. Do not teach that syntax as universal: their lifecycle and invocation controls differ by IDE.

For new work in VS Code, prefer a skill. VS Code has deprecated prompt files for Agent Host sessions and recommends converting existing prompt files to skills; see the [note on the Prompt files page](Prompt-files.md#vs-code-deprecation).

## Instructions or skill

Use instructions for short guidance that applies frequently:

```text
Run the focused API tests before completing back-end changes.
Use existing components from the shared Angular library.
```

Use a skill for the detailed procedure: which tests to select, how to interpret failures and how to report the evidence.

## Check client support

See the reviewed [cross-IDE support table](../Copilot-Technologies.md#current-ide-support) and its version caveats before sharing one setup or invocation method across VS Code, Visual Studio and JetBrains.

## What to read next

Open the implementation page for the option you chose, build [repository knowledge](../Repository-Knowledge.md), or use the [Copilot glossary](../Copilot-Glossary.md) when two terms still seem close.

## Sources

- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Agent customisation decision matrix in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
