# One turn, many rounds

_For new and regular Copilot users - Last reviewed 4 October 2026_

You ask Copilot to fix one bug. It finds the test command, reproduces the failure, reads the relevant code, edits it and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

If terms such as model input, harness or tool request are new to you, [How Copilot works](How-Copilot-Works.md) introduces them.

[[_TOC_]]

## Turn, round and agent loop

| Term | Meaning here | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final answer | One message, then zero or more progress updates, tool executions, approvals and file edits, followed by one final answer |
| **Round** | One internal pass that builds model input, makes one model call and, when requested, executes one or more tools | Status updates, tool activity and approvals may appear while it works |
| **Agent loop** | The harness mechanism that runs those rounds | Copilot continues until it can answer or needs you |

![One user turn containing six rounds of model calls, tool activity and a final response](Media/turns-rounds-agent-loop.svg =820x)

> **A word about "turn".** Different documentation uses it differently. Some count every step as a turn: a *user turn* when you send a message, an *LLM turn* each time the model is called, a *tool turn* when a tool runs. VS Code's Cache Explorer lists each model call as a "model turn", and several SDKs count model calls in their `max_turns` limits. **In this wiki a turn is the whole exchange: your message, all the work Copilot does, and its final answer. Each model call inside it is a round.** When you read "turn" elsewhere, check which meaning is in use.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

A plausible turn looks like this:

| Round | Model output (tool request) | Harness action | New result, which feeds the next round |
| ---: | --- | --- | --- |
| 1 | Search for `*Checkout*Tests*.cs` and read `tests/Shop.Tests/Shop.Tests.csproj` | Runs the search and read tools | `tests/Shop.Tests/CheckoutTests.cs` exists; the project uses xUnit, so the command is `dotnet test tests/Shop.Tests --filter Checkout` |
| 2 | Run `dotnet test tests/Shop.Tests --filter Checkout` | Asks you to approve the command, then runs it in the terminal | One failure: `Applies_discount_once` expected 90.00 but got 81.00, at line 42 |
| 3 | Read `CheckoutTests.cs` around line 42 and `src/Shop/Checkout/CheckoutService.cs` | Reads both files | The test applies one 10 percent code; `ApplyDiscounts` applies the code to every line item instead of once per order |
| 4 | Edit `CheckoutService.cs` so the discount is applied once per order | Applies the edit and shows you the diff | Updated file |
| 5 | Run the same `dotnet test` command again | Runs it | 1 passed |
| 6 | Final answer, no tool request | Stops the loop and returns the text | "The discount was applied per line item. Fixed in `ApplyDiscounts`; the Checkout tests pass." |

Each row only exists because of the row above it: the command found in round 1 is what round 2 runs, the failure in round 2 is what round 3 reads, and so on. The person initiated one turn. In this example, the harness sends model input to the model six times. Your next message starts a new turn, and those six rounds travel with it as history.

## What happens in a round

Each round follows the loop in [How Copilot works](How-Copilot-Works.md#the-30-second-explanation): build model input, make one model call, then run any requested tools and record the results.

For example, the model might request reads of both the failing test and its implementation in one round. The harness performs the permitted reads and their results can enter the next round's model input. The model requests; the harness validates and executes; a person may approve.

Generated tool requests count as model output. Tool results can become model input in a later round. Large generated edits and large tool results can therefore affect usage, but a tool request does not have one fixed token or credit price.

The progress messages you see are summaries, not the model's private reasoning.

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
