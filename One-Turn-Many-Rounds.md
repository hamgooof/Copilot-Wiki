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

> **"Turn" means different things in different tools.** In this wiki a turn is your message, all the work Copilot does, and its final answer; each model call inside it is a round. See [How Copilot works](How-Copilot-Works.md#the-30-second-explanation) for the full note.

## Follow one turn from start to finish

You send:

```text
The checkout tests are failing. Find the failing test, fix the cause and verify the result.
```

Here is how that turn might go:

| Round | Model output (tool request) | Harness action | New result, which feeds the next round |
| ---: | --- | --- | --- |
| 1 | Search for test files matching `*Checkout*Tests*` or `*Checkout*spec*` | Runs the search tool | `tests/Shop.Tests/CheckoutTests.cs`, so the command is `dotnet test tests/Shop.Tests --filter Checkout` |
| 2 | Run `dotnet test tests/Shop.Tests --filter Checkout` | Asks you to approve the command, then runs it in the terminal | One failure: `Voucher_is_applied_once` expected 90.00 but got 81.00, at line 42 |
| 3 | Read `CheckoutTests.cs` around line 42 and `src/Shop/Checkout/CheckoutService.cs` | Reads both files | The test applies a 10 percent voucher to a two-item order; `ApplyVoucher` is called once per line item, so the voucher is applied twice |
| 4 | Edit `CheckoutService.cs` so `ApplyVoucher` runs once per order | Applies the edit and shows you the diff | Updated file |
| 5 | Run the same `dotnet test` command again | Runs it | 1 passed |
| 6 | Final answer, no tool request | Stops the loop and returns the text | "The voucher was applied once per line item. Fixed in `ApplyVoucher`; the Checkout tests pass." |

Each row depends on the one above: round 1 finds the command that round 2 runs, round 2 finds the failure that round 3 reads. A model call can ask for several tools at once (round 3 reads two files), but only when they do not depend on each other; a read that needs a search result has to wait for the next round. You sent one message; the model was called six times. Your next message starts a new turn, and all six rounds go with it as history.

The progress messages you see are summaries, not the model's private reasoning.

## Why later rounds cost more

Each round's model input carries everything from the rounds before it, plus the new tool results. By round 5 in the example, the model is sent the search results, both files, the first test run and the diff all over again before it decides to rerun the test.

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
