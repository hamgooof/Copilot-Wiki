# How Copilot works in your IDE

_For new and regular Copilot users · Last reviewed 11 August 2026_

Copilot combines the model you select with the surrounding IDE experience. In an [Agent task](Home.md#my-first-ten-minutes), the IDE prepares the information the model can see, offers it tools and carries out the actions it requests.

Microsoft calls the layer around the model the **agent harness**. A simple understanding of its job is enough to make Copilot much easier to use well.

[[_TOC_]]

## The 30-second explanation

1. You send Copilot a message
2. The IDE assembles the input for the model from your message, instructions, conversation history, available tool descriptions and any workspace context already supplied or retrieved
3. The model returns text, a request to use a tool, or both
4. The harness checks and executes the requested tool
5. The tool result is carried into the next model call
6. The process continues until the model returns the final response

A quick question may need one model call. Fixing a feature can require several rounds of searching, reading, editing, testing and correcting.

## What reaches the model

![The context sources VS Code can assemble before calling a language model](Media/context-assembly.svg =900x)

Your chat message is one part of a larger input. Depending on the IDE, mode and task, the assembled prompt can contain:

- Built-in system instructions and descriptions of the tools available to the model
- Your custom instructions, selected agent and loaded skills
- Your current message
- Earlier messages in the current session
- Implicit editor context such as the active file, selection, visible errors and Git state
- Files, folders or other information that you referenced explicitly
- Results from earlier search, read, terminal and editing tools

The categories are explanatory; their actual order inside the prompt can differ.

File content can reach the model in several ways. You can attach or reference it, the IDE can provide implicit context or indexed snippets, and an agent can request search or read tools. Each model call receives a selected set of content, while the rest of the repository remains available for retrieval.

## The four parts worth remembering

### 1. The model decides what to say or do

The language model processes the assembled input. It can produce an answer or a structured request to use a tool.

Models differ in capability, speed, context-window size, tool use and cost. Think of them as engines: some are more powerful, some are faster, and some use less fuel. The surrounding vehicle can stay familiar even when you change the engine.

### 2. The harness connects the model to the IDE

The harness handles the plumbing around the model. It:

- Assembles the model input
- Describes the available tools
- Checks and executes tool calls
- Returns tool results to the model
- Manages the agent loop, permissions and limits
- Adapts prompts and tools for different model families

If the model is the engine, the harness is the vehicle around it: controls, steering, safety systems and the connection to the road.

### 3. Context supplies the task information

**Context** is the information available to the model for its current call. This includes the assembled prompt and any task-specific material it contains.

Useful context tells Copilot:

- What outcome you want
- Where it should work
- Which constraints matter
- How success should be checked

The context window has a token limit, so relevance matters. More context can help when it is useful; unrelated material consumes space and can distract the model.

### 4. Tools give the model hands

The model cannot directly open a file, edit code or run a test. Tools give it hands to work in your workspace.

Common tools can:

- Search for files or text
- Read and edit files
- Run terminal commands and tests
- Inspect errors and source-control changes

The model chooses a tool and supplies the arguments. The harness carries out the action and returns the result. That result can then guide the next round, such as correcting an edit after a failed test.

## Where Copilot technologies fit

Instructions, skills, custom agents and prompt files change the guidance or working setup available to Copilot. The [technology chooser](Copilot-Technologies/Choose-the-right-technology.md) compares them in one place.

## Information you may need to provide

Copilot can draw on its training and the context assembled by the IDE. It may still need you to supply:

- A team decision that was never written down
- Details from another independent session
- The exact file or component you mean
- A current external fact or version requirement
- The command or evidence that proves the work is correct
- A clear boundary when words such as "improve" could cover half the repository

Writing those details into the request reduces discovery work and guessing. The [working efficiently guide](Working-Efficiently-and-Managing-Cost.md#2-define-the-finish-line-before-starting) shows how to structure a task and choose the lightest suitable interaction.

## What to read next

- Context and usage: [Tokens and context windows](Tokens-and-Context-Windows.md)
- A worked agent task: [One prompt, many rounds](One-Prompt-Many-Rounds.md)
- New vocabulary: [Copilot terminology without the headache](Copilot-Terminology.md)
- Choosing a technology: [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agent harnesses in VS Code](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context assembly and implicit context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Adding files and other context to VS Code chat](https://code.visualstudio.com/docs/chat/copilot-chat-context)
