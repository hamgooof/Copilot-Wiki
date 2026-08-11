# Choose the right Copilot technology

_For new and regular Copilot users - Last reviewed 12 August 2026_

Instructions, skills, prompt files and custom agents can all guide Copilot. The useful question is not "which one is best?" but "what are we trying to reuse?"

[[_TOC_]]

## Quick decision guide

1. **A rule or fact should apply to most repository work:** use [repository instructions](Custom-instructions.md)
2. **Guidance applies only to particular files or folders:** use [path-specific instructions](Custom-instructions.md#path-specific-instructions)
3. **Copilot needs a repeatable procedure for one kind of task:** use an [agent skill](Agent-skills.md)
4. **A person should deliberately run the same saved request with new inputs:** use a [prompt file](Prompt-files-and-other-IDE-features.md#prompt-files)
5. **Copilot needs a reusable role, model or tool configuration:** use a [custom agent](Custom-agents-and-subagents.md#custom-agent)
6. **A separate worker would help isolate delegated work:** use a [subagent](Custom-agents-and-subagents.md#subagents) where supported
7. **A rule must always be enforced:** use a formatter, analyser, test, permission or repository control

## Comparison

| Customisation | What it reuses | How it starts | Example |
| --- | --- | --- | --- |
| [Repository instructions](Custom-instructions.md) | Shared facts and rules | Applied automatically | Correct build and test commands |
| [Path-specific instructions](Custom-instructions.md#path-specific-instructions) | Rules for matching code | Applied for relevant files | Angular conventions under `src/client` |
| [Skill](Agent-skills.md) | A task procedure and supporting resources | Loaded when relevant or invoked directly | Review branch changes against a checklist |
| [Prompt file](Prompt-files-and-other-IDE-features.md#prompt-files) | A saved request | Invoked by a person | Trace callers of a supplied API symbol |
| [Custom agent](Custom-agents-and-subagents.md#custom-agent) | A worker role and configuration | Selected or delegated | Read-focused independent Reviewer |
| [Subagent](Custom-agents-and-subagents.md#subagents) | Isolated delegated work | Invoked by another agent | Investigate one question and return a report |

## Skill or custom agent

This is the distinction most people need:

- A **custom agent defines the worker**: role, continuing instructions, tools, model, subagent access and handoffs
- A **skill defines a playbook**: steps, checklist, examples, scripts, references and output format

One agent can use several skills. The same skill can be reused by different agents.

### Scenario A: occasional structured review

Run this from the ordinary Ask or Agent experience:

```text
/review-changes origin/develop
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

1. Use the built-in Plan agent when the Angular and .NET change needs investigation
2. Use the general Agent to implement it
3. Let a testing skill supply the repository's test procedure and conventions
4. Start a new chat and invoke the review skill for an independent pass

Plan, Implement, Test and Review are workflow phases. They do not each need to become a custom agent.

## Prompt file or skill

Use a **prompt file** when the person should choose exactly when to run a saved request. It is a convenient named shortcut with inputs.

Use a **skill** when the procedure can be selected automatically during a larger task, or when it carries a fuller workflow with supporting resources.

Both can appear as slash commands in current VS Code. Their lifecycle and invocation controls differ by IDE.

## Instructions or skill

Use instructions for short guidance that applies frequently:

```text
Run the focused API tests before completing back-end changes.
Use existing components from the shared Angular library.
```

Use a skill for the detailed procedure: which tests to select, how to interpret failures and how to report the evidence.

## Current IDE support

The latest official documentation reported the following on 12 August 2026:

| Feature | VS Code | Visual Studio | JetBrains |
| --- | --- | --- | --- |
| Custom instructions | Supported | Supported | Preview in GitHub's lifecycle table |
| Prompt files | Public preview | Public preview | Public preview |
| Custom agents | Supported | Visual Studio 2026 18.4+ | Preview |
| Subagents | Supported | Not supported | Preview |
| Agent skills | Supported | Visual Studio 2026 18.5+ | Preview |

Support depends on the installed IDE and Copilot extension or plugin version. Visual Studio 2022 17.14 does not provide every feature listed for Visual Studio 2026. GitHub still labels several JetBrains customisation features as Preview.

Check the current support pages before sharing one setup across all three clients.

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
