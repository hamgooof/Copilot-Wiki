# Prompt files

_For Copilot users in VS Code, Visual Studio and JetBrains - Last reviewed 12 August 2026_

Prompt files are saved requests that a person invokes deliberately. They are useful when the same task needs new inputs each time but should not run automatically.

> Prompt files are in public preview and subject to change. GitHub currently supports them in VS Code, Visual Studio and JetBrains, with different controls in each client.

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
/explain-api-path OrdersController.Create
```

The exact input-variable behaviour is model-driven. The `argument-hint` and text after the slash command should make the required input clear even when the client does not display a form.

## Run a prompt file

- **VS Code:** type `/` followed by the prompt name, run **Chat: Run Prompt**, or open the file and select the play button
- **Visual Studio:** use `#prompt:` in chat or the context picker provided by the installed version
- **JetBrains:** type `/` followed by the prompt name in Copilot Chat, or manage the files through **Settings > Tools > GitHub Copilot > Customizations**

Check the current client documentation before standardising UI steps across a team, particularly while JetBrains support remains in public preview.

## When a custom agent fits instead

The API-path example is one saved task. It does not need a persistent role or a special worker configuration.

Use a custom agent only if tracing is part of a wider recurring role with its own instructions, tools or model. A prompt file can also select a custom agent where the client supports that combination.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Prompt files and context in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
