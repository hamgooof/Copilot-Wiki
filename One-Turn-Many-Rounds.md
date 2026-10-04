# One turn, many rounds

_For new and regular Copilot users - Last reviewed 4 October 2026_

You ask Copilot to fix one bug. It finds the test command, reproduces the failure, reads the relevant code, edits it and reruns the test. You experienced **one turn**; Copilot completed several **rounds** inside it.

If terms such as model input, harness or tool request are new to you, [How Copilot works](How-Copilot-Works.md) introduces them.

[[_TOC_]]

## Turn, round and agent loop

| Term | Meaning here | What you see |
| --- | --- | --- |
| **Turn** | The complete exchange from one user message to the final answer | Your message, then progress, approvals and edits, then one answer |
| **Round** | One model call, plus any tools it asks for | A progress message or an approval prompt |
| **Agent loop** | The harness mechanism that runs those rounds | Copilot continues until it can answer or needs you |

![One user turn containing six rounds of model calls, tool activity and a final response](Media/turns-rounds-agent-loop.svg =820x)

> **A word about "turn".** Different documentation uses it differently. Some count every step as a turn: a *user turn* when you send a message, an *LLM turn* each time the model is called, a *tool turn* when a tool runs. VS Code's Cache Explorer lists each model call as a "model turn", and several SDKs count model calls in their `max_turns` limits. **In this wiki a turn is the whole exchange: your message, all the work Copilot does, and its final answer. Each model call inside it is a round.** We never qualify "turn"; for the steps inside a round we say *your request*, *model input*, *model call*, *model output* (which may contain *tool requests*), *tool result* and *final response*. When you read "turn" elsewhere, check which meaning is in use.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

Here is how that turn might go:

| Round | Model output (tool request) | Harness action | New result, which feeds the next round |
| ---: | --- | --- | --- |
| 1 | Search for `*Checkout*Tests*.cs` and read `tests/Shop.Tests/Shop.Tests.csproj` | Runs the search and read tools | `tests/Shop.Tests/CheckoutTests.cs` exists; the project uses xUnit, so the command is `dotnet test tests/Shop.Tests --filter Checkout` |
| 2 | Run `dotnet test tests/Shop.Tests --filter Checkout` | Asks you to approve the command, then runs it in the terminal | One failure: `Applies_discount_once` expected 90.00 but got 81.00, at line 42 |
| 3 | Read `CheckoutTests.cs` around line 42 and `src/Shop/Checkout/CheckoutService.cs` | Reads both files | The test applies one 10 percent code; `ApplyDiscounts` applies the code to every line item instead of once per order |
| 4 | Edit `CheckoutService.cs` so the discount is applied once per order | Applies the edit and shows you the diff | Updated file |
| 5 | Run the same `dotnet test` command again | Runs it | 1 passed |
| 6 | Final answer, no tool request | Stops the loop and returns the text | "The discount was applied per line item. Fixed in `ApplyDiscounts`; the Checkout tests pass." |

Each row depends on the one above: round 1 finds the command that round 2 runs, round 2 finds the failure that round 3 reads. You sent one message; the model was called six times. Your next message starts a new turn, and all six rounds go with it as history.

The progress messages you see are summaries, not the model's private reasoning.

## Why later rounds cost more

Each round's model input carries everything from the rounds before it, plus the new tool results. By round 5 in the example, the model is re-reading the search results, both files, the first test run and the diff before it decides to rerun the test.

That has two effects:

- **Quality:** useful results help the next decision; a long test log mostly gets in the way
- **Cost:** you pay to process the earlier input again in every round, though repeated input is cheaper when the provider has it cached

Tool requests are model output and tool results become model input, so a large edit or a noisy command costs more than a small one. The harness can trim or summarise old material when the session grows, but it does not reliably remove the noise for you.

[Tokens and context windows](Tokens-and-Context-Windows.md#how-context-grows-during-a-turn) shows this growth as a diagram.

## When to steer

Step in when Copilot goes in circles, wanders off the task, or is missing something you know. [How to steer](Working-Efficiently-and-Managing-Cost.md#8-steer-when-progress-drifts).

## What to read next

- See how input accumulates in [Tokens and context windows](Tokens-and-Context-Windows.md)
- Apply the practical habits in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
- Look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
