# Working efficiently and managing cost

_For regular Copilot users · Last reviewed 11 August 2026_

> **If you change only three habits:** start a fresh session for unrelated work; describe the outcome, scope, constraints and evidence; keep always-on instructions short. Those changes usually matter more than shaving a few words from a prompt.

Efficient Copilot use comes from clear tasks, relevant context and timely steering. Saving a few words in the prompt rarely matters if Copilot then searches the wrong area or needs three corrections.

[[_TOC_]]

## What currently drives cost

GitHub's usage-based billing measures model interactions in AI credits. The cost of an interaction depends primarily on:

- The model used
- Input tokens sent to the model
- Output and reasoning tokens generated
- Cached-token pricing for that model
- The number of model rounds or calls needed to complete the turn

One GitHub AI Credit corresponds to USD $0.01. Credits are the billing unit; tokens measure model input and output. Different models price tokens differently.

Code completions and next-edit suggestions are currently not billed in AI credits on paid plans. Chat, Edit and Agent interactions use model tokens and can consume credits under the current usage-based model.

### See your own usage

- **GitHub:** open your profile menu, select **Copilot settings**, then **Usage**
- **VS Code:** select the Copilot icon in the status bar to see quota and usage information
- **Visual Studio:** select the Copilot icon in the upper-right corner, then **Copilot Consumptions**
- **JetBrains:** select the Copilot icon in the status bar, then **View quota usage**

The screens available to you depend on your organisation's plan and policies.

## 1. Use the lightest interaction that fits

| Need | Start with |
| --- | --- |
| Complete the line or nearby code | Inline suggestion |
| Explain a function or error | Focused chat question |
| Make one bounded edit | Inline chat or edit workflow |
| Investigate and change several files | Agent |
| Apply a repeatable specialist process | Skill or custom agent |

Agent mode is valuable because it can work independently. That same autonomy can cause more rounds and tool output than a small task needs.

The [first ten minutes](Home.md#my-first-ten-minutes) section shows where these interactions appear in each IDE.

## 2. Define the finish line before starting

A strong task description contains:

- **Outcome:** what should be true at the end
- **Scope:** where Copilot should work and what it should avoid
- **Constraints:** interfaces, standards or behaviour that must remain intact
- **Evidence:** tests, commands or checks that prove success

Example:

```text
Update the orders endpoint to reject negative quantities.
Limit changes to the orders API and its tests. Preserve the existing error schema.
Add a regression test and run the orders unit-test command.
```

This may be longer than "fix quantity validation", but it can avoid several exploratory and corrective rounds.

## 3. Start a new session for unrelated work

Old conversation history consumes context and can bias a new task. Use one session per coherent task or closely related phase of work.

Continue a session when prior decisions are genuinely relevant. Start a new one when the goal, repository or problem changes.

## 4. Keep persistent instructions lean

Persistent instructions are valuable when they prevent recurring mistakes. They are wasteful when they contain long, rarely relevant process documentation.

- Keep universal instructions short and specific
- Use path-specific instructions for local rules
- Move occasional detailed workflows into skills
- Remove duplicated or contradictory files
- Review instructions after architecture or tool changes

## 5. Choose a model for the task

Use the model picker in chat to see the models your organisation makes available. **Auto** lets Copilot select a model for the current interaction. A manual choice is useful when you already know which model suits the job.

- Try stronger reasoning models for architecture, difficult debugging and ambiguous design work
- Try faster or lower-cost models for routine, well-scoped transformations
- In VS Code, open the arrow beside a supported reasoning model to change **Thinking Effort**
- Avoid changing the model, thinking effort or enabled tools repeatedly during one task because those changes can disrupt prompt caching

A low-cost model can become expensive after several retries. Measure the cost and quality of completing the task as a whole.

## 6. Research and plan before expensive implementation

For complex or unclear work:

1. Ask for read-only discovery and a plan
2. Resolve ambiguities and review the proposed scope
3. Implement once the plan is sound
4. Validate against explicit acceptance criteria

Planning has a cost, but it can prevent larger wrong changes and repeated rework. Skip formal planning for obvious, small tasks.

## 7. Control tools and their output

- Enable only the tools relevant to the task. In VS Code, use **Configure Tools** in the chat box; in Visual Studio, select the **Tools** icon
- Use a read-only [custom agent](Copilot-Technologies/Custom-agents-and-subagents.md#custom-agent) for recurring review or investigation work
- Filter commands to produce focused output
- Ask for failure summaries before full logs
- Avoid reading generated, vendored or build-output directories without a reason
- Put repeatable mechanical steps into scripts

Large tool results consume context. Prefer focused commands and filtered output so the next model call receives the useful evidence with less noise.

## 8. Keep investigation focused

Broad exploration creates more rounds, tool output and context. Give Copilot a known file, failing test, error message or component whenever you can.

If the agent starts searching widely:

- Point it towards the part of the repository you know is relevant
- Ask it to explain what it is trying to find
- Give it the correct test or build command
- Stop the run and tighten the request if the search keeps drifting

VS Code can delegate isolated work to subagents. Most people can ignore this advanced option until a complex task would benefit from a separate investigation. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) explains the boundary.

## 9. Preserve useful cache boundaries

Stable input can allow prompt caching, reducing latency and cost. VS Code's current documentation identifies several changes that rebuild the cache boundary.

VS Code controls the order in which prompt components are assembled. Choose the relevant settings and definitions before the task, then leave them stable while it runs.

Practical habits:

- Choose the model, reasoning effort and enabled tools before starting when practical
- Avoid toggling tools or reasoning settings during the task
- Edit instruction files and custom-agent definitions between sessions where possible
- Use `/compact` at a natural phase boundary in VS Code
- Use VS Code's Cache Explorer when investigating actual cache behaviour; [Tokens and context windows](Tokens-and-Context-Windows.md#inspect-context-and-usage) shows how to open it

VS Code documents model changes, reasoning-effort changes, context-size changes, tool toggles, instruction edits and custom-agent edits as cache-breaking changes. Skill metadata, skill activation, editor state and terminal creation still need measured testing. These are listed in [T18](To-Test.md#t18-vs-code-cache-boundary-matrix).

## 10. Detect and stop wasteful loops

You know the system, repository and intended outcome better than the agent. Steering it is part of the job. Redirect or stop when Copilot:

- Repeats the same command without learning from the result
- Alternates between two unsuccessful edits
- Searches increasingly unrelated parts of the repository
- Continues after the acceptance criteria are already met
- Produces changes faster than they can be reviewed

Tell it what it has missed, point it towards the right file or command, reduce the scope or ask for a plan. If the task itself has changed, return to the [new-session guidance](#3-start-a-new-session-for-unrelated-work).

## Learn which models suit your work

Model guidance is a starting point. Try different available models on representative tasks and build your own feel for where each one works well.

Pay attention to:

- Whether it understood the task without repeated correction
- How well it used tools and followed repository instructions
- The quality of the final diff and tests
- How long it took and how much rework you needed

Keep a short personal or team note when a pattern repeats, such as one model working well for planning and another for routine implementation.

## A compact working pattern

1. Start a clean session for the task
2. State outcome, scope, constraints and evidence
3. Attach or name only the most relevant context
4. Plan first if the task is ambiguous or high risk
5. Use an appropriate model and toolset
6. Let Copilot implement and validate
7. Review the diff and test evidence
8. Record durable decisions outside the chat

## Sources

- [Improving agent quality to optimize AI usage](https://docs.github.com/en/enterprise-cloud@latest/copilot/tutorials/optimize-ai-usage)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Monitoring GitHub AI Credits usage](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [About Copilot auto model selection](https://docs.github.com/en/copilot/concepts/models/auto-model-selection)
- [VS Code February 2026 release notes: context compaction](https://code.visualstudio.com/updates/v1_110)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Optimize AI usage in VS Code](https://code.visualstudio.com/docs/agents/guides/optimize-usage)
