# Prompt files

_For Copilot users in VS Code, Visual Studio and JetBrains - Last reviewed 4 October 2026_

Prompt files are saved requests that a person invokes deliberately. They are useful when the same task needs new inputs each time but should not run automatically. A prompt file is like a saved sat-nav destination: nothing happens until you choose to start it.

> Prompt files are supported in VS Code and Visual Studio and remain in preview in JetBrains, with different controls in each client.

> **VS Code is moving prompt files to skills.** For new work in VS Code, prefer an [agent skill](Agent-skills.md). See [VS Code deprecation](#vs-code-deprecation) at the end of this page.

[[_TOC_]]

## Where prompt files live

A repository prompt file normally lives at:

```text
.github/prompts/<prompt-name>.prompt.md
```

It can contain:

- A reusable request
- An input hint or variables
- A selected agent, model or tool list where supported
- Links to workspace files
- The expected output format

Unlike instructions, it does not apply automatically. Unlike a skill, it is primarily a saved request the person chooses to run. Here, **prompt file** means a saved user request, rather than the complete model input assembled for a call.

## Example: explain an API path

```markdown
---
name: explain-api-path
description: Trace how an endpoint, method or symbol is reached through this application.
argument-hint: "[endpoint, method or symbol]"
agent: ask
---

Trace `${input:target:endpoint, method or symbol}` through this repository.

1. Find direct and indirect callers
2. Identify the HTTP endpoint when one exists
3. Trace the .NET controller, application and domain layers
4. Identify the Angular client or service call when present
5. Return the ordered call path with file and symbol locations

Explain uncertainty where dynamic dispatch, reflection or generated code prevents a complete trace.
Do not change files.
```

In current VS Code, a user can run:

```text
/explain-api-path OrdersController.Create
```

Input controls vary by client. The `argument-hint` and placeholder should make the required input clear even when the client does not display a form.

## Run a prompt file

- **VS Code:** type `/` followed by the prompt name, run **Chat: Run Prompt**, or open the file and select the play button
- **Visual Studio:** use `#prompt:` in chat or the context picker provided by the installed version
- **JetBrains:** type `/` followed by the prompt name in Copilot Chat, or manage the files through **Settings > Tools > GitHub Copilot > Customizations**

Check the current client documentation before standardising UI steps across a team, particularly while JetBrains support remains in public preview.

## When a custom agent fits instead

The API-path example is one saved task. It does not need a persistent role or a special worker configuration.

Use a custom agent only if tracing is part of a wider recurring role with its own instructions, tools or model. A prompt file can also select a custom agent where the client supports that combination.

## VS Code deprecation

VS Code's documentation (checked 4 October 2026) says: "Prompt files are deprecated for Agent Host sessions and aren't loaded by Agent Host. They continue to work with the Local agent for now, but the Local agent will be removed in a future release."

In practice, prompt files still run today in the session type VS Code calls the Local agent, but not in Agent Host sessions, and the Local agent itself is due to be removed. VS Code recommends converting existing prompt files to agent skills, and offers a prompt-file migration for this. Use a skill for any playbook the team will rely on, and check the Visual Studio and JetBrains documentation separately before changing a workflow there.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Prompt files and context in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
