# Working efficiently and managing cost

_For regular Copilot users - Last reviewed 27 August 2026_

> **Three useful habits:** start a fresh session for unrelated work, describe the outcome and evidence, and keep always-on instructions short.

Efficient Copilot use comes from clear tasks, relevant context and timely steering. Saving a few words rarely helps if Copilot then searches the wrong area or needs several corrections.

[[_TOC_]]

## What drives usage

GitHub's usage-based billing measures model interactions in AI credits. The cost of an interaction depends on factors including:

- The selected model
- Input, output and supported reasoning tokens
- Cached-token pricing
- The number of model calls needed to complete the turn

One GitHub AI Credit corresponds to USD $0.01. Credits are the billing unit; tokens measure model input and output. Different models price tokens differently.

Code completions and next-edit suggestions are currently not billed in AI credits on paid plans. Chat, Edit and Agent interactions can consume credits under the current usage model.

### See your usage

- **GitHub:** open your profile menu, select **Copilot settings**, then **Usage**
- **VS Code:** select the Copilot icon in the status bar
- **Visual Studio:** select the Copilot badge, then **Copilot Usage**; older versions may call it **Copilot Consumptions**
- **JetBrains:** select the Copilot icon, then **View quota usage**

The screens available depend on your organisation's plan, IDE version and policies.

## 1. Use the lightest interaction that fits

| Need | A sensible starting point |
| --- | --- |
| Complete the line or nearby code | Inline suggestion |
| Explain or investigate without editing | Chat or Ask |
| Make one bounded edit | Inline chat or edit workflow |
| Investigate and change several files | Agent |

Agent can complete larger tasks independently, which can involve more rounds and tool output. Use it when that capability helps the task. Skills are reusable guidance used within these interactions; the [technology chooser](Copilot-Technologies/Choose-the-right-technology.md) explains when they fit.

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

This gives Copilot enough direction to avoid broad exploration and unnecessary rework.

## 3. Start a new session for unrelated work

Continue a session while its earlier decisions and evidence remain useful. Start a new one when the goal, repository or problem changes.

A fresh session removes the previous conversation history. It does not remove repository instructions or other customisations that apply automatically.

## 4. Keep persistent instructions lean

- Keep repository-wide instructions short and genuinely universal
- Use path-specific instructions for local language or component rules
- Move occasional detailed workflows into skills
- Remove duplicated or contradictory guidance
- Prefer formatters, analysers and tests for rules they can enforce

If a repository needs more detail, keep a short pointer to a [repository-knowledge index](Repository-Knowledge.md#use-the-index-for-smart-lookup) and let the agent read only task-relevant documents. Those documents still consume input when read. The proposed benefit is less repeated orientation and correction across later work, which needs testing rather than assuming.

## 5. Choose a model for the task at hand

There is rarely one best model for an entire repository. A model that performs well on an Angular refactor might be less convincing on a .NET API design or SQL query.

Experiment with representative tasks and notice:

- How much correction the model needs
- Its familiarity with the language and framework
- How well it uses tools and follows repository guidance
- The quality and size of the final diff
- Latency, AI-credit use and rework

Try stronger reasoning models for difficult debugging and ambiguous design. Faster or lower-cost models can suit routine, well-scoped changes. Include corrections and reruns when judging the real cost of a model for a task.

Changing the model, reasoning effort or enabled tools during a task can stop the next request from matching the previously cached input. Choose them before starting when practical, but do not turn configuration into a ritual for every request.

### Why a long pause can matter

When the start of a model request matches a recent request, the provider can reuse that prefix at the lower cached-input rate. After an idle period, some or all of that reusable state might no longer be available. The next request can therefore cost more even when your new message is short.

For example, using GitHub's rates reviewed on 27 August 2026:

| 100,000 repeated input tokens | Read from cache | Written to cache after a miss |
| --- | ---: | ---: |
| Claude Sonnet 4.6 | 3 AI credits | 37.5 AI credits |
| Claude Opus 4.8 | 5 AI credits | 62.5 AI credits |

This example counts only those 100,000 input tokens. It excludes new input, output and reasoning, and it illustrates the size of the difference rather than predicting when a cache miss will occur. Cache retention can vary by model, Copilot route and service load.

If a later request is unexpectedly slow or expensive, VS Code's Cache Explorer can show how much input was reused and where the matching prefix changed.

### Count model work around tools and files

There is no fixed per-file or per-tool credit formula. Count the model interactions around the operation:

- A model-generated read, command, edit or file-write request is model output
- File content, search results, terminal output and edit results included in a later model call become input
- Generated plan, documentation and code content supplied through an edit tool is output
- A fresh session that reads a saved plan processes that plan as ordinary input
- Further validation, correction, delegation and review can add calls and tokens

Current GitHub documentation does not describe a separate AI-credit fee for the operating-system act of writing a file. A saved file is not automatically cached. Prompt caching is a separate reuse mechanism based on a matching model-request prefix and current provider rules.

## 6. Use Plan when the work is unclear

Current VS Code and Visual Studio versions include a built-in Plan agent that performs read-only research, asks clarifying questions and prepares an implementation plan. JetBrains also provides Plan. In an older or policy-restricted client without Plan, use Ask or another read-only chat request to agree an implementation plan, then switch to Agent when edits are appropriate.

Use planning when scope or design needs agreement before code changes. Skip it for obvious, small work.

In VS Code, **Start Implementation** carries the plan and conversation context to the chosen implementation agent. The Plan agent's automatic memory file is cleared when the conversation ends. If you want a durable plan and clear context boundary, use **Open in Editor**, save the concise agreed plan, and begin a new Agent session from it.

A handoff is useful for continuity; a fresh session reads the saved plan as ordinary input. Neither route automatically saves AI credits. Planning itself uses model calls, so its value may be accuracy, reviewability and resumability even when total usage rises. See [Spec-Driven Development](Spec-Driven-Development.md) for durable specification, plan and task options and a matched evaluation.

## 7. Guide tools without micromanaging every call

The model requests tools and the harness defines how they operate. You normally do not write every search or terminal command yourself.

For an ordinary request, the default tool selection is usually a sensible starting point. Available tools can vary with the client, model, session and organisation policy. Adjust the selection when it becomes confusing, exceeds a client limit or exposes capabilities unrelated to the task.

Useful controls include:

- Use Ask for ordinary read-only investigation where your IDE provides it
- Point Copilot to the relevant file, failing test or error when you know it
- In VS Code, use the **Configure Tools** control when irrelevant tools cause confusion; use the equivalent in another client only where it is available
- Put the correct focused build or test command in repository instructions
- Use existing quiet or filtered command options when large logs repeatedly flood the conversation
- Keep generated, vendored and build-output directories out of broad searches where practical

If a repeatable command helps developers and CI as well as Copilot, give it a clear script or task name. Keep scripts tied to a normal team or build need so somebody owns and maintains them.

## 8. Steer when progress drifts

You are the expert on the repository and intended behaviour. Intervene when Copilot appears stuck in a negative loop, searches unrelated areas, misunderstands the goal or continues without useful progress.

Tell it what it missed, point it towards the right component or command, reduce the scope or ask for a plan. A useful correction states the observed problem, the intended boundary and the next check, for example:

```text
Stop changing the Angular client. The defect is in the orders API mapping.
Inspect OrdersController and its focused tests, then explain the proposed fix before editing.
```

Steer early when the current direction is clearly wrong. If useful work is nearly complete, waiting for the final response and then reviewing the IDE's **Files changed** view can be less disruptive. After a large correction, restate the finish line so later rounds do not continue from the earlier misunderstanding.

## A compact working pattern

1. Start a suitable session for the task
2. State the outcome, scope, constraints and evidence
3. Point Copilot to the best starting file, test or error
4. Plan first when the task is genuinely unclear
5. Let Copilot investigate, implement and validate
6. Steer when it lacks repository knowledge or drifts
7. Review the diff and test evidence
8. Record durable decisions in repository or workspace files

## What to read next

- [Understand tokens and context windows](Tokens-and-Context-Windows.md)
- [Choose a Copilot technology](Copilot-Technologies/Choose-the-right-technology.md), or look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [Improving agent quality to optimize AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)
- [Usage-based billing for organisations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
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
