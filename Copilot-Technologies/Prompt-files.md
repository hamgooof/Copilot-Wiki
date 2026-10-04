# Prompt files

_For Copilot users in VS Code, Visual Studio, Rider and WebStorm - Last reviewed 4 October 2026_

A prompt file is a saved request you run yourself, with new inputs each time. Copilot never runs it on its own. A prompt file is like a saved sat-nav destination: nothing happens until you choose to start it.

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
- A selected agent, model or tool list, if your IDE supports it
- Links to workspace files
- The expected output format

A prompt file holds your request only, not the whole model input.

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

Write the `argument-hint` and placeholder so the input is obvious even if your IDE shows no form.

## Run a prompt file

- **VS Code:** type `/` followed by the prompt name, run **Chat: Run Prompt**, or open the file and select the play button
- **Visual Studio:** use `#prompt:` in chat or the context picker
- **Rider and WebStorm (preview):** type `/` followed by the prompt name in Copilot Chat, or manage the files through **Settings > Tools > GitHub Copilot > Customizations**

## When a custom agent fits instead

The API-path example is a single task, so it does not need its own agent.

Use a custom agent only if tracing is part of a wider recurring role with its own instructions, tools or model. A prompt file can also select a custom agent if your IDE supports it.

## VS Code deprecation

VS Code's documentation (checked 4 October 2026) says: "Prompt files are deprecated for Agent Host sessions and aren't loaded by Agent Host. They continue to work with the Local agent for now, but the Local agent will be removed in a future release."

So prompt files still work in Local agent sessions but not in Agent Host sessions, and the Local agent is going away. VS Code can convert your prompt files to skills. Use a skill for anything the team relies on. This change is to VS Code only.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Prompt files and context in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
