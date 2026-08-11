# How Copilot actually works

_For existing Copilot users who are new to agents · Last reviewed 11 August 2026_

You do not need to know how a language model is trained. You do need to know that the model is only one component in the Copilot experience.

This wiki uses the following practical, unofficial mental model:

> **Copilot agent experience = language model + harness + context + tools + execution environment**

The model supplies the reasoning and generation. The harness builds prompts, offers tools, executes actions and keeps the model working through multiple rounds. The execution environment is where those tools and code changes run, such as your local workspace or a cloud-hosted environment.

[[_TOC_]]

## The 30-second explanation

1. You give Copilot a message.
2. The harness combines it with instructions, history, relevant files and tool descriptions.
3. The model reads that assembled context and returns text or a request to use a tool.
4. If it requests a tool, the harness executes it and records the result.
5. Copilot sends the updated context back to the model for another round.
6. When the model stops requesting tools, the final text becomes the response you see.

A small question may need only one model call. A feature implementation may need many rounds of searching, reading, editing, testing and correcting.

## What reaches the model

![The context sources Copilot assembles before calling a language model](Media/context-assembly.svg =900x)

The chat box shows only the message you typed. The model can receive a much larger prompt containing system instructions, customizations, conversation history, files and tool results.

The opposite is equally important: the model cannot see any of **your** task-specific information—files, decisions, errors or current workspace state—that the harness did not put into the current context. It can still draw on knowledge learned during training, although that knowledge can be incomplete or outdated. Access to a repository does not mean every file is loaded into every request.

## The five building blocks

### 1. The model reasons and generates

The language model processes the assembled prompt and produces text or a structured request to use a tool. The optional [deeper token section](Copilot-Terminology.md#tokens-in-more-depth) explains how token IDs and decoding fit underneath that user-facing description.

Models differ in capability, speed, context-window size, tool use and cost. Think of them like vehicles: a forklift and a car are both useful, but they are designed for different jobs. The biggest model is not automatically the best choice for every task.

### 2. The harness turns model output into useful work

The agent harness is the bridge between the model and VS Code. It:

- Assembles context.
- Describes the available tools.
- Validates and executes tool calls.
- Feeds results back to the model.
- Applies loop limits, approvals and, on supported surfaces, hooks—commands that run at set moments.
- Adapts prompts and tool behaviour for different model families.

The model is the engine; the harness is the rest of the vehicle.

The execution environment is separate from both. A local IDE agent might change files and run commands on your machine; a cloud agent works in remote infrastructure with different access, isolation and controls.

### 3. Context supplies what the model can see

Context can contain your message, previous messages, selected or retrieved files, instructions and tool results. It has a token limit, so relevance matters more than sheer volume.

Good context says what outcome you want, where to work, which constraints matter and how success should be checked.

### 4. Tools let Copilot act

Tools can search, read and edit files, run terminal commands, inspect source control or connect to external systems. The model chooses a tool; the harness executes it.

A tool's output becomes potential input for the next round. This is how the agent can react to a failed test rather than merely claim that the code should work.

### 5. Customizations shape the work

Customizations change the guidance or capabilities available to the agent:

| Customization | Think of it as… |
| --- | --- |
| Instructions | Standing rules that apply automatically |
| Skill | A task-specific playbook with optional scripts and references |
| Custom agent | A named specialist with its own brief and tools |
| MCP server | A Model Context Protocol adaptor that exposes another system's tools or data |
| Hook | A command that always runs at a set moment, such as a validation check before an edit; support varies by surface |

They do not replace the language model; they shape the environment in which it works.

## Chat, edit and agent work

Use the lightest interaction that fits the task:

| Experience | What happens | Good for |
| --- | --- | --- |
| Inline suggestion | A specialised model predicts code as you type | Completing the current line or nearby code |
| Chat or question | A model answers using supplied and retrieved context | Explanations, advice and focused questions |
| Edit | Copilot makes a targeted change to selected code | Small, bounded modifications |
| Agent task | The harness lets a model use tools and iterate | Investigation, multi-file changes and validation |

Full agent autonomy is powerful, but it also creates more opportunities for tool calls, additional rounds and unwanted changes. Do not send a forklift to move a coffee mug.

## What Copilot does not automatically know

Copilot does not automatically know:

- The contents of a file it has not been given or retrieved.
- A decision made in another independent session.
- Your team's unwritten preferences.
- Whether an external fact is still current without retrieving a current source.
- Whether generated code is correct without suitable validation.
- Whether you meant “improve everything” or one specific behaviour.

Clear context is not bureaucratic ceremony. It is how you reduce guessing.

## Follow a real task

You submit:

```text
Add duplicate-email validation to the registration endpoint.
Preserve the existing error format, add a regression test and run the relevant tests.
```

One turn might then contain these rounds:

1. Search for the endpoint and existing validation patterns.
2. Read the error-response and data-access code.
3. Edit the endpoint and test.
4. Run the focused test command.
5. Read a failure and correct the implementation.
6. Run the test again.
7. Produce the final summary and evidence.

![One user turn containing several internal rounds](Media/turns-rounds-agent-loop.svg =900x)

Each round sees an updated prompt. A clear starting request helps the agent spend those rounds on useful work rather than discovering what you meant.

## What to read next

- New vocabulary: [Copilot terminology without the headache](Copilot-Terminology.md)
- Context and usage: [Tokens and context windows](Tokens-and-Context-Windows.md)
- The internal workflow: [One prompt, many rounds](One-Prompt-Many-Rounds.md)
- Choosing a customization: [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agent harnesses, execution environments, roles and models in VS Code](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Microsoft Learn: tokens, token IDs and embeddings](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
