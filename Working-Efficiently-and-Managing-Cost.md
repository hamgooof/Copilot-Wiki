# Working efficiently and managing cost

_For regular Copilot users - Last reviewed 4 October 2026_

> **Three useful habits:** start a fresh session for unrelated work, describe the outcome and evidence, and keep always-on instructions short.

Trimming words from your request saves little. A vague request that sends Copilot searching the wrong code, or needs three corrections, costs far more.

[[_TOC_]]

## What uses credits

Chat, Edit and Agent use AI credits. Code completions and next-edit suggestions do not.

1 AI credit = USD 0.01 (100 credits = 1 dollar). Each model prices tokens differently; GitHub's [models and pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing) page lists the rates.

A turn costs more when:

- you pick a more expensive model
- the model reads or writes more tokens
- the turn needs more model calls (rounds) to finish

### See your usage

- **VS Code:** select the Copilot icon in the status bar
- **Visual Studio:** select the Copilot badge, then **Copilot Usage**
- **JetBrains IDEs:** select the Copilot icon, then **View quota usage**
- **If your Copilot account is linked to github.com:** open your profile menu, select **Copilot settings**, then **Usage**

## 1. Use the lightest interaction that fits

| Need | A sensible starting point |
| --- | --- |
| Complete the line or nearby code | Inline suggestion |
| Explain or investigate without editing | Chat or Ask |
| Make one bounded edit | Inline chat or edit workflow |
| Investigate and change several files | Agent |

Agent works through many rounds and reads every tool result, so one turn usually costs more than a chat answer. Use it when the task spans several files.

## 2. Define the finish line

A useful task description includes:

- **Outcome:** what should be true at the end
- **Scope:** where Copilot should work and what it should avoid
- **Constraints:** interfaces or behaviour that must remain intact
- **Evidence:** tests or checks that prove success

Example:

```text
Update the orders endpoint to reject negative quantities.
Limit changes to the orders API and its tests. Preserve the existing error schema.
Add a regression test and run the focused orders tests.
```

Copilot now knows where to look and when it is done.

Treat the **Evidence** line like the acceptance criteria on a ticket: if you cannot say how you will check it, the request is not ready.

## 3. Start a new session for unrelated work

Continue a session while its earlier decisions and evidence remain useful. Start a new one when the goal, repository or problem changes. Weigh the cost too: coming back to a long session after a break means the whole history is re-read at full price, and a new session with a short brief can be cheaper than waking an old one.

A fresh session removes the previous conversation history. It does not remove repository instructions or other customisations that apply automatically.

## 4. Keep persistent instructions lean

- Keep repository-wide instructions short and genuinely universal
- Use path-specific instructions for local language or component rules
- Move occasional detailed workflows into skills
- Remove duplicated or contradictory guidance
- Prefer formatters, analysers and tests for rules they can enforce

## 5. Choose a model for the task at hand

There is rarely one best model for an entire repository. A model that performs well on an Angular refactor might be less convincing on a .NET API design or SQL query.

Experiment with representative tasks and notice:

- How much correction the model needs
- Its familiarity with the language and framework
- How well it uses tools and follows repository guidance
- The quality and size of the final diff
- Latency, AI-credit use and rework

Use a stronger reasoning model for hard debugging and unclear design. Use a cheaper model for routine, well-scoped changes and simple questions about the code. A cheap model that needs two reruns is not cheap.

Changing the model, reasoning effort or enabled tools during a task can stop the next model call from matching the previously cached input. Pick them when you start a turn, then leave them alone until it finishes.

### Why a long pause can matter

When the start of a model call's input matches a recent model call, the provider reuses that prefix at the cheaper cached rate. The cache expires after an idle period. For Claude models, assume more than about five minutes; some models keep it longer. After that, your next message pays full price to re-read the whole conversation, however short the message is.

For example, using GitHub's rates reviewed on 27 August 2026:

| 100,000 repeated input tokens | Read from cache | Written to cache after a miss |
| --- | ---: | ---: |
| Claude Sonnet 4.6 | 3 AI credits | 37.5 AI credits |
| Claude Opus 4.8 | 5 AI credits | 62.5 AI credits |

The table counts only the repeated input, not new input or output.

If a turn is unexpectedly slow or expensive, VS Code's Cache Explorer can show how much input each model call reused and where the matching prefix changed. Cache Explorer lists each model call as a "model turn"; this wiki calls it a round.

### Count model work around tools and files

Anything the model writes, including tool requests and file content, is output. Anything read back in a later call, including file content, search results and a saved plan, is input. Writing a file has no separate fee, and a saved file is not automatically cached.

## 6. Use Plan when the work is unclear

Current VS Code and Visual Studio versions include a built-in Plan agent that performs read-only research, asks clarifying questions and prepares an implementation plan. JetBrains IDEs also provide Plan.

Use planning when scope or design needs agreement before code changes. Skip it for obvious, small work.

In VS Code the Plan agent keeps the plan in a session memory file, which lives only as long as that conversation. **Start Implementation** stays in the same conversation, so the implementing agent gets the plan and everything discussed so far. If you want the plan to outlive the conversation, or a clean start for implementation, use **Open in Editor**, save the agreed plan as a file, and begin a new Agent session from it.

A handoff is changing driver without stopping: the new driver is handed the full journey log. A fresh chat with a saved plan is a new car that gets only the printed route.

![Same-agent continuation retains accumulated context, a custom-agent handoff changes instructions while retaining the transcript, and a fresh chat can start from a bounded saved plan](Media/context-cost-options.svg =760x)

See [Spec-Driven Development](Spec-Driven-Development.md) for durable specification, plan and task options.

## 7. Guide tools without micromanaging every call

You do not need to tell Copilot which search or command to run. The default tools are fine for most turns. Narrow them when Copilot keeps picking irrelevant tools or hits a tool limit. If the same narrow set is useful every time, put it in a [custom agent](Copilot-Technologies/Custom-agents-and-subagents.md) so nobody has to set it up by hand.

Useful controls include:

- Use Ask for ordinary read-only investigation
- Point Copilot to the relevant file, failing test or error when you know it
- In VS Code, use **Configure Tools** to switch off tools the task does not need
- Put the correct focused build or test command in repository instructions
- Use existing quiet or filtered command options when large logs repeatedly flood the conversation
- Keep generated, vendored and build-output directories out of broad searches

If a repeatable command helps developers and CI as well as Copilot, give it a clear script or task name. Keep scripts tied to a normal team or build need so somebody owns and maintains them.

## 8. Steer when progress drifts

You are the expert on the repository and intended behaviour. Intervene when Copilot goes round in circles, searches the wrong code or misreads the goal.

Tell it what it missed, point it towards the right component or command, reduce the scope or ask for a plan. A useful correction states the observed problem, the intended boundary and the next check, for example:

```text
Stop changing the Angular client. The defect is in the orders API mapping.
Inspect OrdersController and its focused tests, then explain the proposed fix before editing.
```

Steer early when the current direction is clearly wrong. If useful work is nearly complete, let it finish and review the **Files changed** view. After a large correction, restate the finish line so later rounds do not continue from the earlier misunderstanding.

## What to read next

- [Understand tokens and context windows](Tokens-and-Context-Windows.md)
- [Choose a Copilot technology](Copilot-Technologies/Choose-the-right-technology.md), or look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [Improving agent quality to optimize AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)
- [Monitoring GitHub AI Credits usage](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Sessions and handoff in VS Code](https://code.visualstudio.com/docs/agents/concepts/sessions)
- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
- [About Copilot automatic model selection](https://docs.github.com/en/copilot/concepts/models/auto-model-selection)
- [Optimise AI usage in VS Code](https://code.visualstudio.com/docs/agents/guides/optimize-usage)
- [Models and pricing for GitHub Copilot](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Plan mode in Copilot Chat for JetBrains](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide?tool=jetbrains)
- [Manage Copilot usage and models in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-usage-and-models?view=visualstudio)
