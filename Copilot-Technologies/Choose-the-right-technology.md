# Choose the right Copilot technology

_For new and regular Copilot users · Last reviewed 11 August 2026_

Instructions, skills, prompt files and agents can all guide Copilot. The main difference is when they activate and how much of the working setup they change.

[[_TOC_]]

## Quick decision guide

Ask these questions in order:

1. **Should this rule apply to most work in the repository?** Use [short repository instructions](Custom-instructions.md)
2. **Should it apply only to particular files or folders?** Use [path-specific instructions](Custom-instructions.md#path-specific-instructions)
3. **Is this a detailed workflow that is only relevant for some tasks?** Use an [agent skill](Agent-skills.md)
4. **Should a person deliberately start the same request with new inputs?** Use a [prompt file](Prompt-files-and-other-IDE-features.md#prompt-files)
5. **Does the task need a named specialist, a restricted toolset or a different model?** Use a [custom agent](Custom-agents-and-subagents.md#custom-agent) where your IDE supports it
6. **Would a separate worker help keep investigation or review out of the main context?** In VS Code, allow the main agent to use a [subagent](Custom-agents-and-subagents.md#subagent) where the feature is enabled
7. **Must a check always run?** Put it in CI, repository policy or another deterministic control

## Comparison

| Customisation | How it starts | Best for | Context behaviour |
| --- | --- | --- | --- |
| [Repository instructions](Custom-instructions.md) | Applied automatically | Short rules and commands that matter often | Can occupy context across many requests |
| [Path-specific instructions](Custom-instructions.md#path-specific-instructions) | Applied for matching paths | Component, language or directory-specific guidance | Loaded only when the path scope matches |
| [`AGENTS.md`](Custom-instructions.md#agentsmd-requires-surface-awareness) | Discovered by supporting agents | Repository guidance shared across compatible tools | Discovery rules vary by IDE and version |
| [Skill](Agent-skills.md) | Selected when relevant or invoked directly | Detailed workflows, scripts and supporting references | Loaded for the task that needs it |
| [Prompt file](Prompt-files-and-other-IDE-features.md#prompt-files) | Started by a person | Reusable one-off requests | Added when the prompt file is run |
| [Custom agent](Custom-agents-and-subagents.md#custom-agent) | Selected or delegated to | A specialist role, model or toolset | Its profile becomes part of that agent's context |
| [Subagent](Custom-agents-and-subagents.md#subagent) | Delegated at runtime | Isolated exploration, testing or review | Uses a separate context window and returns a result |

## Instructions or skill

Use **instructions** for short guidance that applies frequently:

```text
Run pnpm test for unit tests.
Use existing components from packages/ui.
```

Use a **skill** for a workflow with several steps, examples or supporting files:

```text
When preparing a database migration, inspect the schema, generate the migration,
run the validation script and produce the rollback checklist.
```

Putting the full migration process into always-loaded instructions would make unrelated requests carry it too.

## Skill or custom agent

A **skill** changes the method used for a relevant task. The current agent remains in charge and follows the playbook.

A **custom agent** changes the worker's role or setup. It can provide a specialist brief, a restricted toolset or a preferred model.

Use a skill for "follow our release-review process". Use a custom agent for "act as a read-only security reviewer".

## Custom agent or subagent

A custom agent is a reusable definition. A subagent is a separate worker created during a task.

A custom agent can be selected directly in a chat. In VS Code, the main agent can also invoke one as a subagent. The [custom agents and subagents page](Custom-agents-and-subagents.md) explains how invocation, context and model selection work.

## Prompt file or skill

Use a **prompt file** when a person should choose exactly when to run a reusable request.

Use a **skill** when Copilot should recognise that a workflow is relevant during a larger task.

## Keep the setup small

Every automatic instruction and available capability adds something for Copilot to consider. Add a technology when it solves a problem you can name, then use the checklist on the linked page.

## Current IDE support

GitHub's support table reported the following on 11 August 2026:

| Feature | VS Code | Visual Studio | JetBrains |
| --- | --- | --- | --- |
| Custom instructions | Supported | Supported | Preview |
| Prompt files | Supported | Supported | Preview |
| Custom agents | Supported | Supported | Preview |
| Subagents | Supported | Not supported | Preview |
| Agent skills | Supported | Supported | Preview |

The table shows feature-level support. Individual instruction formats and UI controls still differ. JetBrains provides an **Agent Customizations** editor from the settings icon in Copilot Chat; VS Code and Visual Studio use their own controls. Check the linked support table when a feature is missing or before standardising repository files.

## Sources

- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
