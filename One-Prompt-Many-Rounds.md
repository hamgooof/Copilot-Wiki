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
| **Run** *(blog term)* | The full execution of all rounds in the turn | The complete piece of work |

![A user turn containing several model-and-tool rounds](Media/turns-rounds-agent-loop.svg =900x)

Microsoft's VS Code engineering blog defines turn, round and run in this way. This wiki uses the same vocabulary for IDE agent behaviour.

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

## Turn, round and run

The [VS Code coding-harness article](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode) gives us the clearest user-facing distinction. This wiki uses **turn** for one user-visible chat exchange and **round** for one pass through the internal model-and-tool loop.

During each round, the harness:

1. Assembles the next prompt, carrying forward the useful conversation and adding new results
2. Calls the selected model.
3. Receives text, tool calls or both.
4. Validates and executes any tool calls.
5. Records tool results and workspace changes.
6. Checks cancellation, limits and whether another round is needed

If the model returns a final answer without requesting another tool, the loop can finish and the turn ends.

## What the harness does

The model itself cannot open a file, run a command or edit your workspace. It generates text or structured requests. The **agent harness** turns those requests into useful editor actions.

The harness is responsible for:

- Assembling the context sent to the model
- Declaring which tools are available and their input formats
- Checking and executing tool calls
- Returning tool results to the next round
- Applying permissions, approvals and limits
- Managing long conversations through techniques such as compaction
- Adapting prompts and tools to the selected model

Model choice matters, and the harness also shapes how well that model can work inside the IDE.

## What a tool is

A tool lets the agent interact with the IDE and workspace. Tools give the model hands: the model requests an action, then the harness performs it. Common examples include:

- Searching for files or text
- Reading and editing files
- Running terminal commands and tests
- Inspecting source-control changes
- Fetching current documentation

The model chooses from the exposed tools by reading their names, descriptions and schemas. The harness executes the selected tool.

### Tool access and tool results

A tool gives the agent a capability. The information returned by that tool becomes context after the agent uses it.

For example, a file-reading tool lets the agent request a file. The file contents enter the working context when that read occurs.

## Why later rounds become larger

The next prompt normally carries the conversation forward and adds the results accumulated so far:

![Context sources assembled for a model call](Media/context-assembly.svg =900x)

A later round may therefore contain:

- The original system and custom instructions
- Your current message and conversation history
- Tool definitions
- Files read during earlier rounds
- Search output and terminal results
- A summary of workspace changes

VS Code describes the prompt as being rebuilt for each round, meaning that it assembles the latest effective context for the model. Prompt caching can still reuse a matching prefix, so the rebuilt prompt may contain both cached and fresh input.

This has two practical consequences:

1. **Quality:** useful evidence helps the model make a better next decision; noisy results can distract it.
2. **Usage:** accumulated input may be processed again on later rounds, subject to the product's context management and prompt caching.

Tool calls do not have one fixed token price. Usage depends on the model, input, output, cache behaviour, tool results and number of rounds.

## Permissions and safety

Tool impact ranges from reading a file to changing code or running a deployment command.

Use the smallest useful toolset:

| Role or task | Sensible starting capability |
| --- | --- |
| Code reviewer | Read and search |
| Documentation editor | Read, search and edit documentation paths |
| Test fixer | Read, search, edit and focused test execution |
| Deployment investigator | Read logs first; require approval before changes |

Instructions such as “be careful” influence model behaviour but do not enforce a boundary. Use tool restrictions, permissions, protected branches and explicit approvals for controls that matter.

## Keeping an agent loop efficient

Before starting:

- State the outcome, scope, constraints and evidence of success
- Point to a known file, error or command when you have one
- Keep the available tools focused on the task
- Ask for read-only discovery first when the task is ambiguous or risky

While it runs:

- Prefer focused commands over thousands of lines of output
- Stop repeated failures that are not producing new information
- Redirect searches that are drifting into unrelated code
- Pause broad investigation and narrow the question when the search starts to drift

Before accepting the result:

- Ask what was tested and inspect the evidence
- Compare the final summary with the actual diff
- Record decisions that must outlive the session in a file, issue or pull request

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Trust and safety for AI in VS Code](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
