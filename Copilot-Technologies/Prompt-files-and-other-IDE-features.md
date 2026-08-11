# Prompt files and explicit context

_For Copilot users in VS Code, Visual Studio and JetBrains - Last reviewed 12 August 2026_

Prompt files are saved requests that a person invokes deliberately. They are useful when the same task needs new inputs each time but should not run automatically.

> Prompt files are currently in Public Preview and can change. They are available in VS Code, Visual Studio and JetBrains, with different controls in each client.

[[_TOC_]]

## Prompt files

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

Unlike instructions, it does not apply automatically. Unlike a skill, it is primarily a saved request the person chooses to run.

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
/explain-api-path MyApplicationLayerFunction
```

The exact input-variable behaviour is model-driven. The `argument-hint` and text after the slash command should make the required input clear even when the client does not display a form.

## Run a prompt file

- **VS Code:** type `/` followed by the prompt name, run **Chat: Run Prompt**, or open the file and select the play button
- **Visual Studio:** use `#prompt:` in chat or the context picker provided by the installed version
- **JetBrains:** open **Agent Customizations** from Copilot Chat settings and use the prompt controls in the installed plugin

Because this feature is in Preview, keep examples small and check the current client documentation before standardising UI steps.

## Why this is not automatically a custom agent

The API-path example is one saved task. It does not need a persistent role or a special worker configuration.

Use a custom agent only if tracing is part of a wider recurring role with its own instructions, tools or model. A prompt file can also select a custom agent where the client supports that combination.

## Explicit context

IDE chat can accept files, folders, symbols, terminal output, source-control changes and other references. Use explicit context when a particular item must be considered.

- **VS Code:** type `#`, select **Add Context**, or drag a file into Chat
- **Visual Studio:** use the context picker in the chat box
- **JetBrains:** select or open the relevant code and use the available Copilot Chat context controls

Start with the smallest useful scope. If you already know the relevant file, symbol or failing test, naming it gives Copilot a better starting point than a repository-wide search.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Prompt files and context in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=vs-2022)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Providing context to GitHub Copilot](https://docs.github.com/en/copilot/concepts/context)
- [Adding context to VS Code chat](https://code.visualstudio.com/docs/chat/copilot-chat-context)
