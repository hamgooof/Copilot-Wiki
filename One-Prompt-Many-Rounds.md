# One prompt, many rounds: turns, tools and the agent loop

| Page status | Intended audience | Last reviewed |
| --- | --- | --- |
| Draft foundation page | Existing Copilot users new to agentic workflows | 8 August 2026 |

You ask Copilot to fix one bug. It searches, reads three files, edits one, runs a test, sees a failure, edits again and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

That distinction explains why one apparently simple message can take time, use several tools and process a surprising amount of context.

## On this page

- [The short version](#the-short-version)
- [Follow one turn from start to finish](#follow-one-turn-from-start-to-finish)
- [Turn, round and run](#turn-round-and-run)
- [What the harness does](#what-the-harness-does)
- [What a tool is](#what-a-tool-is)
- [Why later rounds become larger](#why-later-rounds-become-larger)
- [Permissions and safety](#permissions-and-safety)
- [Keeping an agent loop efficient](#keeping-an-agent-loop-efficient)

## The short version

| Term | Meaning in the VS Code harness | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final assistant response | One message sent; one answer returned |
| **Round** | One loop pass: build prompt, call model, execute requested tools, record results, decide whether to continue | Usually hidden unless you inspect debug logs |
| **Agent loop** | The control mechanism that performs those rounds | The agent appears to keep working |
| **Run** | The full execution of all rounds in the turn | The complete piece of work |

![A user turn containing several model-and-tool rounds](Media/turns-rounds-agent-loop.svg)

## Follow one turn from start to finish

Suppose you send:

```text
Find the cause of the failing checkout test, fix it and verify the result.
```

A plausible turn looks like this:

| Round | Model decides to… | Harness does… | New context produced |
| ---: | --- | --- | --- |
| 1 | Search for checkout tests | Executes repository search | Matching paths and snippets |
| 2 | Read the likely test and implementation | Reads those files | File contents |
| 3 | Reproduce the failure | Runs the focused test command | Failure output |
| 4 | Correct the implementation | Applies the requested edit | Updated workspace state |
| 5 | Verify the fix | Runs the test again | Passing or failing output |
| 6 | Stop using tools and answer | Returns the final response | Summary and evidence |

The person initiated one turn. The harness may have sent the accumulated prompt to the model six times.

## Turn, round and run

The [VS Code coding-harness article](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode) gives us the clearest user-facing distinction:

> **Turn** = one user-visible chat exchange. **Round** = one pass through the internal model-and-tool loop.

During each round, the harness:

1. Builds or rebuilds the prompt from current context.
2. Calls the selected model.
3. Receives text, tool calls or both.
4. Validates and executes any tool calls.
5. Records tool results and workspace changes.
6. Checks cancellation, limits, hooks and whether another round is needed.

If the model returns a final answer without requesting another tool, the loop can finish and the turn ends.

### Why you may see “turn” used differently

GitHub's Copilot SDK documentation calls each LLM call and its consequences a **turn**. In other words, the SDK's event vocabulary uses *turn* where the user-facing VS Code article uses *round*.

Neither page can be silently substituted for the other. This wiki follows the VS Code harness wording because it matches what a Copilot user experiences. When discussing SDK event logs such as `assistant.turn_start`, we will say **SDK turn** explicitly.

## What the harness does

The model itself cannot open a file, run a command or edit your workspace. It generates text or structured requests. The **agent harness** turns those requests into useful editor actions.

The harness is responsible for:

- Assembling the context sent to the model.
- Declaring which tools are available and their input formats.
- Checking and executing tool calls.
- Returning tool results to the next round.
- Applying permissions, approvals, limits and hooks.
- Managing long conversations through techniques such as compaction.
- Adapting prompts and tools to the selected model.

This is why “which model is best?” is only part of the question. The model is the engine; the harness is the rest of the vehicle.

## What a tool is

A tool lets the agent interact with something outside the model. Common examples include:

- Searching for files or text.
- Reading and editing files.
- Running terminal commands and tests.
- Inspecting source-control changes.
- Fetching current documentation.
- Querying GitHub, a database or another service through MCP.
- Delegating a bounded task to a subagent.

The model chooses from the tools exposed to it by reading their names, descriptions and schemas. The harness—not the model—executes the selected tool.

### Tools are capabilities, not knowledge

Giving an agent a database tool does not put the database in its context. The agent first decides to call the tool; the returned rows then become context for a later round.

The same applies to repository files. Access to a file-reading tool is not the same as having already read every file.

## Why later rounds become larger

The prompt is rebuilt on each round and can include the results accumulated so far:

![Context sources assembled for a model call](Media/context-assembly.svg)

A later round may therefore contain:

- The original system and custom instructions.
- Your current message and conversation history.
- Tool definitions.
- Files read during earlier rounds.
- Search output and terminal results.
- A summary of workspace changes.

This has two practical consequences:

1. **Quality:** useful evidence helps the model make a better next decision; noisy results can distract it.
2. **Usage:** accumulated input may be processed again on later rounds, subject to the product's context management and prompt caching.

There is no universal “cost per tool call.” Cost depends on the model, tokens, cache behaviour, tool output and number of rounds needed.

## Permissions and safety

Tools have different impact. Reading a file is not equivalent to deploying to production.

Use the smallest useful toolset:

| Role or task | Sensible starting capability |
| --- | --- |
| Code reviewer | Read and search |
| Documentation editor | Read, search and edit documentation paths |
| Test fixer | Read, search, edit and focused test execution |
| Deployment investigator | Read logs first; require approval before changes |

Instructions such as “be careful” influence model behaviour but do not enforce a boundary. Use tool restrictions, permissions, hooks, protected branches and explicit approvals for controls that matter.

## Keeping an agent loop efficient

Before starting:

- State the outcome, scope, constraints and evidence of success.
- Point to a known file, error or command when you have one.
- Pick only the tools and MCP servers relevant to the task.
- Ask for read-only discovery first when the task is ambiguous or risky.

While it runs:

- Prefer focused commands over thousands of lines of output.
- Stop repeated failures that are not producing new information.
- Redirect searches that are drifting into unrelated code.
- Split genuinely independent, noisy investigation into a subagent.

Before accepting the result:

- Ask what was tested and inspect the evidence.
- Review the diff rather than trusting the summary alone.
- Record decisions that must outlive the session in a file, issue or pull request.

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Copilot SDK agent loop and SDK-specific turn events](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/agent-loop)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Trust and safety for AI in VS Code](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
