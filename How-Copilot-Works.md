# How Copilot works in your IDE

_For new and regular Copilot users - Last reviewed 12 August 2026_

Copilot combines a language model with an **agent harness**: the software around the model that connects it to your IDE, prepares its input, offers tools and carries out requested actions. GitHub Copilot, Claude Code and Codex are different examples of harnessed coding experiences.

[[_TOC_]]

## The 30-second explanation

1. You send Copilot a message
2. The harness assembles input for the model
3. The model generates text, requests a tool, or sometimes does both
4. The harness validates and executes the requested tool, asking for approval where required
5. The tool result becomes available for the next model call
6. The loop continues until Copilot returns its final response

A quick question might need one model call. Fixing a feature can require several rounds of searching, reading, editing, testing and correcting.

![A user request passing through the harness, model and optional tools before a response returns](Media/agent-loop.svg =760x)

## What reaches the model

The message in the chat box is only the part you typed. Before calling the model, the harness can assemble a larger input containing instructions, conversation history, tool descriptions and task information.

![Illustrative categories that can form the input to a language model](Media/context-assembly.svg =760x)

Depending on the IDE, mode and task, that input can contain:

- Built-in system instructions and descriptions of available tools
- Repository, path-specific or personal instructions
- The selected custom agent and any skill loaded for the task
- Your current message and earlier messages in the session
- Editor signals such as the active file, selection, visible errors and Git state
- File content or other material you explicitly reference
- Search, file, terminal and editing results from earlier rounds

These categories explain what may be present. They are not shown in prompt order, and not every category appears in every model call.

The whole repository is not automatically copied into the prompt. File content can be supplied as editor context, attached explicitly or retrieved when the model requests search and read tools.

## The four parts worth remembering

### 1. The model generates the next output

The language model processes the current input and generates output one token at a time. That output can be prose, a structured tool request, or both where the model integration supports it.

Models differ in capability, speed, context-window size, tool use and cost. Think of the model as the engine: changing it can alter how the same surrounding Copilot experience performs.

### 2. The harness runs the experience

The harness is the software around the model. It:

- Assembles the input
- Describes the available tools
- Validates and executes tool requests
- Returns tool results to the model
- Manages the agent loop, approvals and limits
- Adapts the experience for different model families

If the model is the engine, the harness is the vehicle around it: controls, steering and the connection to the working environment.

### 3. Context supplies the task information

**Context** is the information available to the model for its current call. Useful context explains:

- The outcome you want
- Where Copilot should work
- Which constraints matter
- How success should be checked

The context window has a finite capacity. Useful evidence can improve the next decision; irrelevant material still occupies space and can distract the model.

### 4. Tools give the model hands

The model cannot directly open a file, edit code or run a test. Tools give it hands to work in your workspace.

Common tools can:

- Search for files, symbols or text
- Read and edit files
- Run terminal commands and tests
- Inspect errors and source-control changes

The model requests a tool and supplies its arguments. The harness checks and performs the action, then returns the result. A failed test can therefore guide the next edit in another round.

## What Copilot does not automatically know

You may still need to supply:

- A team decision that was never written down
- Details from another independent chat
- The exact component or behaviour you mean
- A current external requirement
- The command or evidence that proves the work is correct
- A clear boundary when words such as "improve" could cover half the repository

Writing these details into the request or an accessible repository file reduces discovery work and guessing.

## Where customisations fit

Instructions, skills, custom agents and prompt files change the guidance or working setup available to Copilot. The [technology chooser](Copilot-Technologies/Choose-the-right-technology.md) compares them in one place.

## What to read next

- [Tokens and context windows](Tokens-and-Context-Windows.md)
- [One prompt, many rounds](One-Prompt-Many-Rounds.md)
- [Copilot terminology without the headache](Copilot-Terminology.md)
- [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agent harnesses in VS Code](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
