# One prompt, many rounds

_For new and regular Copilot users - Last reviewed 12 August 2026_

You ask Copilot to fix one bug. It finds the test command, reproduces the failure, reads the relevant code, edits it and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

[[_TOC_]]

## Turn, round and agent loop

| Term | Meaning here | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final answer | One message sent, zero or more tools or edits, one final answer |
| **Round** | One pass through the internal model-and-tools loop | Status updates, tool activity and approvals may appear while it works |
| **Agent loop** | The harness mechanism that runs those rounds | Copilot continues until it can answer or needs you |

![A user turn containing six model-and-tool rounds](Media/turns-rounds-agent-loop.svg =820x)

The labels follow the VS Code engineering explanation linked in the sources. Other SDKs and products sometimes use the word *turn* differently.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

A plausible turn looks like this:

| Round | Model requests or returns | Harness action | New result |
| ---: | --- | --- | --- |
| 1 | Find how checkout tests are run | Read project scripts and test configuration | Test command and scope |
| 2 | Reproduce the problem | Run the focused checkout tests | Failing test, path and error |
| 3 | Inspect the failure | Read the exact test and relevant implementation | Expectations and code |
| 4 | Correct the cause | Apply the requested edit | Updated workspace state |
| 5 | Verify | Run the focused test again | Passing or failing output |
| 6 | Finish | Stop using tools and return the answer | Summary and evidence |

The person initiated one turn. The harness might have called the model six times.

## Inside each round

During a round, the harness:

1. Assembles the next prompt from existing conversation state and new results
2. Calls the selected model
3. Receives generated text, tool requests or both
4. Validates and executes requested tools
5. Records results and workspace changes
6. Continues the loop or returns the final answer

The user-facing status or intent summaries are not the model's raw internal reasoning.

## Tools: request, execution and result

The model requests a tool with arguments. The harness validates and executes it, and may ask you for approval. The tool result can then enter the next round's input.

For example:

```text
model requests: read the failing test
harness executes: file read
tool returns: test contents
next model call: receives those contents as context
```

The generated tool request counts as model output. Its result can become model input in the next round. Large generated edits and large tool results can therefore affect usage, but a tool call does not have one fixed token or credit price.

## Why later rounds can be larger

The next prompt can carry existing conversation state forward and add search results, file contents, terminal output or edits. Useful and irrelevant material can both persist.

This affects:

- **Quality:** relevant evidence helps; noisy results can distract
- **Usage:** accumulated input can be processed again in later rounds, subject to caching and context management

[Tokens and context windows](Tokens-and-Context-Windows.md#how-context-grows-during-a-turn) illustrates this accumulation.

## When to steer

You know the system, repository and intended outcome better than the agent. Step in when Copilot appears stuck in a negative loop, searches the wrong area, misunderstands the goal or continues without useful progress.

Give it the missing constraint, point it towards the right component or command, or stop and narrow the task. Reviewing the completed **Files changed** list after the turn is also a normal workflow.

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
