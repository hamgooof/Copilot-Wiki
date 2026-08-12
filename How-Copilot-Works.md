# How Copilot works in your IDE

_For new and regular Copilot users - Last reviewed 12 August 2026_

GitHub Copilot is a coding experience built around a language model, with software connecting that model to your IDE. In this wiki, that surrounding software is called the **agent harness**.

Your **request** is the message you type. **Context** is all the information available to the model for one call. The harness packages that information as the **model input** (the **assembled prompt** in the diagram below).

[[_TOC_]]

## The 30-second explanation

Each time the harness sends model input to the model is a **model call**.

1. You send Copilot a request
2. The harness assembles the model input
3. The model generates output: prose, one or more tool requests, or both where the model integration supports it
4. The harness validates and executes requested tools, asking you for approval where required
5. Tool results become available as context for the next model call
6. The loop continues until Copilot returns its final response

A quick question might need one model call and no tools. Fixing a feature can require several rounds of searching, reading, editing, testing and correcting.

![A user request passing through the harness and model, with an optional tool loop, before a final response returns](Media/agent-loop.svg =760x)

## What reaches the model

The request in the chat box is only the part you typed. Before calling the model, the harness can assemble a larger model input containing instructions, conversation history, tool descriptions and task information.

![Illustrative categories that can form the model input](Media/context-assembly.svg =760x)

Depending on the IDE, mode and task, that input can contain:

- Built-in system instructions and descriptions of available tools
- Repository, path-specific or personal instructions
- The selected custom agent and any skill loaded for the task
- Your current request and earlier messages in the session
- Editor signals such as the active file, selection, visible errors and Git state
- File content or other material you explicitly reference
- Search, file, terminal and editing results from earlier rounds

These categories explain what may be present. They are not shown in assembly order, and not every category appears in every model call.

The whole repository is not automatically copied into the model input. File content can be supplied as editor context, attached explicitly or retrieved when the model requests search and read tools.

## The four parts worth remembering

### 1. The model generates the next output

The language model processes the current input and generates output one token at a time. Tokens are the model's numbered text units. The output can be prose, one or more tool requests, or both where the model integration supports it.

Models differ in capability, speed, context-window size, tool use and cost. Think of the model as the engine: changing it can alter how the same surrounding Copilot experience performs.

### 2. The harness runs the experience

The harness is the software around the model. It:

- Assembles the model input
- Describes the available tools
- Validates and executes tool requests
- Returns tool results as context for later calls
- Manages the agent loop, approvals and limits
- Adapts the experience for different model families

If the model is the engine, the harness is the vehicle around it: controls, steering and the connection to the working environment.

### 3. Context gives the model information for this call

**Context** is all the information available to the model for its current call. It can include your request, instructions, relevant files or editor selections, earlier conversation, tool descriptions and results from searches, file reads or terminal commands.

A good request helps by stating:

- The outcome you want
- Where Copilot should work
- Which constraints matter
- How success should be checked

The request is one source of context, not the whole of it. The context window has a finite capacity. Useful evidence can improve the next decision; irrelevant material still occupies space and can distract the model.

### 4. Tools give the model hands

The model cannot directly open a file, edit code or run a test. Tools give it hands to work in your local workspace.

Common tools can:

- Search for files, symbols or text
- Read and edit files
- Run terminal commands and tests
- Inspect errors and source-control changes

The model requests one or more tools and supplies their arguments. The harness checks and performs the actions, asking you to approve them where required, then returns the results. A failed test can therefore guide the next edit in another round.

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

- See [One request, many rounds](One-Request-Many-Rounds.md) for a worked agent loop
- Learn about capacity and compaction in [Tokens and context windows](Tokens-and-Context-Windows.md)
- Look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agent harnesses in VS Code](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
