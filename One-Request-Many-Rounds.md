# One request, many rounds

_For new and regular Copilot users - Last reviewed 12 August 2026_

You ask Copilot to fix one bug. It finds the test command, reproduces the failure, reads the relevant code, edits it and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

If terms such as model input, harness or tool request are new to you, [How Copilot works](How-Copilot-Works.md) introduces them.

[[_TOC_]]

## Turn, round and agent loop

| Term | Meaning here | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final answer | One message, then zero or more progress updates, tool executions, approvals and file edits, followed by one final answer |
| **Round** | One internal pass that builds model input, calls the model and, when requested, executes one or more tools | Status updates, tool activity and approvals may appear while it works |
| **Agent loop** | The harness mechanism that runs those rounds | Copilot continues until it can answer or needs you |

![One user turn containing six rounds of model calls, tool activity and a final response](Media/turns-rounds-agent-loop.svg =820x)

The labels follow the VS Code engineering explanation linked in the sources. Other SDKs and products sometimes use the word *turn* differently.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

A plausible turn looks like this:

| Round | Model output | Harness action | New result |
| ---: | --- | --- | --- |
| 1 | Request to find how checkout tests are run | Validate and run search or file-read tools | Test command and scope |
| 2 | Request to reproduce the problem | Validate and run the focused checkout tests | Failing test, path and error |
| 3 | Request to inspect the failure | Read the exact test and relevant implementation | Expectations and code |
| 4 | Request to correct the cause | Apply the edit | Updated workspace state |
| 5 | Request to verify | Run the focused test again | Passing or failing output |
| 6 | Final prose response | Stop using tools and return the answer | Summary and evidence |

The person initiated one turn. In this example, the harness sends model input to the model six times.

## What happens in a round

During a round, the harness:

1. Builds model input from the available context
2. Calls the selected model
3. Receives model output containing prose, one or more tool requests, or both where the integration supports it
4. Validates and executes requested tools, asking you for approval where required
5. Makes tool results and workspace changes available as context for a later round
6. Continues the loop or returns the final answer

For example, the model might request reads of both the failing test and its implementation in one round. The harness performs the permitted reads and their results can enter the next round's model input. The model requests; the harness validates and executes; a person may approve.

Generated tool requests count as model output. Tool results can become model input in a later round. Large generated edits and large tool results can therefore affect usage, but a tool request does not have one fixed token or credit price.

User-facing status or intent summaries are not the model's raw internal reasoning.

## Why later rounds can be larger

Later model input can carry conversation state forward and add search results, file contents, terminal output or edits. Accumulated context can contain both useful evidence and noise.

The harness may select, truncate or compact material as the session grows, but this does not guarantee that irrelevant material disappears. This affects:

- **Quality:** relevant evidence helps; noisy results can distract
- **Usage:** accumulated input can be processed again in later rounds, subject to caching and context management

[Tokens and context windows](Tokens-and-Context-Windows.md#how-context-grows-during-a-turn) illustrates this accumulation.

## When to steer

Step in when the loop drifts, repeats work or lacks a constraint you know; [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md#8-steer-when-progress-drifts) explains how to redirect it and review the result.

## What to read next

- See how input accumulates in [Tokens and context windows](Tokens-and-Context-Windows.md)
- Apply the practical habits in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
- Look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
