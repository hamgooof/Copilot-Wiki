# One prompt, many rounds: turns, tools and the agent loop

_For new and regular Copilot users · Last reviewed 11 August 2026_

You ask Copilot to fix one bug. It searches, reads three files, edits one, runs a test, sees a failure, edits again and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

That distinction explains why one apparently simple message can take time, use several tools and process a surprising amount of context.

[[_TOC_]]

## The short version

| Term | Meaning in the VS Code harness | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final assistant response | One message sent; one answer returned |
| **Round** | One loop pass: assemble the next prompt, call the model, execute requested tools, record results and decide whether to continue | Usually hidden unless you inspect debug logs |
| **Agent loop** | The control mechanism that performs those rounds | The agent appears to keep working |

![A user turn containing several model-and-tool rounds](Media/turns-rounds-agent-loop.svg =900x)

Microsoft's VS Code engineering blog distinguishes turns from rounds in this way. This wiki uses those two labels for IDE agent behaviour.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

A plausible turn looks like this:

| Round | Model decides to… | Harness does… | New context produced |
| ---: | --- | --- | --- |
| 1 | Find how checkout tests are run | Reads project scripts and test configuration | Checkout test command and scope |
| 2 | Narrow down the failure | Runs the checkout test command | Failing test name, path and error output |
| 3 | Inspect the failure | Reads the exact failing test and relevant implementation | Test expectations and implementation details |
| 4 | Correct the implementation | Applies the requested edit | Updated workspace state |
| 5 | Verify the fix | Runs the focused test again | Passing or failing output |
| 6 | Stop using tools and answer | Returns the final response | Summary and evidence |

The person initiated one turn. The harness may have sent the accumulated prompt to the model six times.

## Inside each round

During each round, the harness:

1. Assembles the next prompt, carrying forward the useful conversation and adding new results
2. Calls the selected model
3. Receives text, tool calls or both
4. Validates and executes any tool calls
5. Records tool results and workspace changes
6. Checks cancellation, limits and whether another round is needed

If the model returns a final answer without requesting another tool, the loop can finish and the turn ends.

## Tool access and tool results

A tool gives the model hands in your workspace. The model requests an action, the harness performs it, and the result can become context for the next round. [How Copilot works in your IDE](How-Copilot-Works.md#4-tools-let-copilot-act) explains tools in more detail.

Tool access and tool results are different. A file-reading tool provides access; the requested file contents enter the working context after the agent chooses to read them.

## Why later rounds become larger

The next prompt normally carries useful conversation forward and adds new results. A later round may include files, search output and terminal results that were not available at the start.

This has two practical consequences:

1. **Quality:** useful evidence helps the model make a better next decision; noisy results can distract it
2. **Usage:** accumulated input may be processed again on later rounds, subject to the product's context management and prompt caching

Tool calls do not have one fixed token price. [Tokens and context windows](Tokens-and-Context-Windows.md#how-context-grows-during-a-turn) shows how accumulated results affect later rounds and overall usage.

## Permissions and safety

Tool impact ranges from reading a file to changing code or running a deployment command.

Use the smallest useful toolset. Instructions such as "be careful" influence behaviour but do not enforce a boundary. For reusable read-only or specialist roles, configure a [custom agent](Copilot-Technologies/Custom-agents-and-subagents.md#custom-agent) with the required tools. Keep protected branches, approvals and other deterministic controls outside the prompt.

## When to intervene

The loop is working well while each round produces useful new evidence or moves the task towards completion. Step in when Copilot repeats an approach, searches unrelated code, uses the wrong test command or continues after the goal is met.

You know the repository and intended behaviour. Give the missing constraint or point Copilot towards the right file, component or command. More practical examples are in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md#10-detect-and-stop-wasteful-loops).

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Trust and safety for AI in VS Code](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
