# How Copilot works

| Page status | Intended audience | Last reviewed |
| --- | --- | --- |
| Draft for internal review | Existing Copilot users new to agentic features | 8 August 2026 |

You do not need to understand how a large language model is built to use Copilot well. You do need a useful mental model of what happens after you submit a request.

## The simple mental model

```text
Your request
    + relevant conversation history
    + instructions
    + selected or retrieved code
    + descriptions of available tools
            |
            v
      language model
            |
       chooses either
       /            \
final response    tool request
                       |
                 tool produces a result
                       |
                 result returns to model
                       |
                 repeat until finished
```

A language model generates a response from the information it is given. It does not directly open files, execute code or browse a system. Copilot provides an agent around the model that can call tools to perform those actions.

## The five building blocks

### 1. The model reasons and generates

The language model reads an assembled prompt and generates text. That text might be an explanation, code, an edit or a request to use a tool.

Models differ in reasoning ability, speed, context-window size and cost. The most capable model is not automatically the best choice for every task.

### 2. Context supplies what the model can see

Context can contain your current message, earlier messages, relevant files, instructions and tool results. The model can reason only about information that has been placed in its current context.

Copilot may retrieve relevant code through workspace indexing, but it does not simply load an entire repository into every request.

### 3. Tools let Copilot act

Tools can read and edit files, search a repository, run terminal commands and connect to other systems. Each tool has a description that helps the model decide when to use it.

The output from a tool becomes more context for the next model decision.

### 4. The agent coordinates the work

An agent combines the model, context and tools. It can inspect the task, select an action, observe the result and continue until it produces a final response.

This repeated process is the **agent loop**. A simple question might require no tools. Implementing a feature can require many loops through reading, editing, testing and correcting.

### 5. Customizations shape the agent

Instructions, skills and custom agents do not replace the language model. They change the guidance, resources, tools or execution profile available to it.

- Instructions say what broadly applicable rules to follow.
- Skills provide a workflow for a particular kind of task.
- Custom agents define a specialised worker with its own instructions and tools.
- MCP servers add external tools and data.
- Hooks execute commands at defined lifecycle events.

## Completion, chat and agent work are different

| Experience | What happens | Good for |
| --- | --- | --- |
| Inline suggestion | A specialised model predicts code while you type | Completing the current line or nearby code |
| Chat/question | A model answers using supplied and retrieved context | Explanation, advice and focused questions |
| Edit | Copilot makes a targeted change to selected code | Small, bounded modifications |
| Agent task | An agent uses tools and iterates | Multi-file work, investigation, implementation and validation |

Do not use full agent autonomy for every small edit. More autonomy usually means more model calls, tools, context and validation work.

## What Copilot does not automatically know

- Details that are not in its training data or current context.
- The contents of a file it has not been shown or retrieved.
- A decision made in another independent session.
- Your team's unwritten preferences.
- Whether generated code is correct without suitable validation.
- Whether an external fact is still current unless it can retrieve a current source.

This is why good Copilot use is mostly good context management: give it a clear goal, the relevant constraints and a way to verify success.

## A worked example

Request:

```text
Add validation to the registration endpoint. Reject duplicate email addresses,
preserve the existing error format, add tests, and run the relevant test suite.
```

A capable agent might:

1. Search for the endpoint and existing validation patterns.
2. Read the error-response and data-access code.
3. Edit the endpoint.
4. Add or update tests.
5. Run the relevant test command.
6. Read the failure output if a test fails.
7. Correct the implementation and test again.
8. Summarise the changes and evidence.

Each search, file read and test result adds working context. A clear request helps the agent select fewer irrelevant actions.

## What to read next

- [Copilot terminology](Copilot-Terminology.md)
- [Tokens and context windows](Tokens-and-Context-Windows.md)
- [Requests, turns, tools and the agent loop](Requests-Turns-Tools-and-Agent-Loops.md)

## Sources

- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
