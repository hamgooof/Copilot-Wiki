# Working efficiently and managing cost

| Page status | Intended audience | Last reviewed |
| --- | --- | --- |
| Draft foundation page | Regular Copilot users | 8 August 2026 |

Efficient Copilot use is not about making every request as cheap as possible. It is about completing useful work with the least unnecessary model work, repetition and human correction.

The biggest savings usually come from better task definition, cleaner context and an appropriate model—not from making prompts artificially short.

## On this page

- [What currently drives cost](#what-currently-drives-cost)
- [Use the lightest interaction that fits](#1-use-the-lightest-interaction-that-fits)
- [Define the finish line](#2-define-the-finish-line-before-starting)
- [Keep sessions and instructions focused](#3-start-a-new-session-for-unrelated-work)
- [Choose a model for the task](#5-choose-a-model-for-the-task)
- [Control tools and subagents](#7-control-tools-and-their-output)
- [Preserve useful cache boundaries](#9-preserve-useful-cache-boundaries)
- [Detect and stop wasteful loops](#10-detect-and-stop-wasteful-loops)
- [A compact working pattern](#a-compact-working-pattern)

> **If you change only three habits:** start a fresh session for unrelated work; describe the outcome, scope, constraints and evidence; keep always-on instructions short. Those changes usually matter more than shaving a few words from a prompt.

## What currently drives cost

GitHub's usage-based billing measures model interactions in AI credits. The cost of an interaction depends primarily on:

- The model used.
- Input tokens sent to the model.
- Output and reasoning tokens generated.
- Cached-token pricing for that model.
- The number of model rounds or calls needed to complete the turn.

One GitHub AI Credit corresponds to USD $0.01, but a credit is a billing unit, not a token. Different models price tokens differently.

Code completions and next-edit suggestions are currently not billed in AI credits on paid plans. Chat, CLI, cloud agent, Spaces and other model-powered features do consume credits under the current usage-based model.

Some existing annual individual subscriptions may remain on legacy premium-request billing until the annual plan ends. Check the billing page for the actual plan before comparing usage.

## 1. Use the lightest interaction that fits

| Need | Start with |
| --- | --- |
| Complete the line or nearby code | Inline suggestion |
| Explain a function or error | Focused chat question |
| Make one bounded edit | Inline chat or edit workflow |
| Investigate and change several files | Agent |
| Perform noisy research separately | Subagent |
| Apply a repeatable specialist process | Skill or custom agent |

Agent mode is valuable because it can work independently. That same autonomy can cause more rounds and tool output than a small task needs.

## 2. Define the finish line before starting

A strong task description contains:

- **Outcome:** what should be true at the end.
- **Scope:** where Copilot should work and what it should avoid.
- **Constraints:** interfaces, standards or behaviour that must remain intact.
- **Evidence:** tests, commands or checks that prove success.

Example:

```text
Update the orders endpoint to reject negative quantities.
Limit changes to the orders API and its tests. Preserve the existing error schema.
Add a regression test and run the orders unit-test command.
```

This may be longer than “fix quantity validation,” but it can avoid several exploratory and corrective rounds.

## 3. Start a new session for unrelated work

Old conversation history consumes context and can bias a new task. Use one session per coherent task or closely related phase of work.

Continue a session when prior decisions are genuinely relevant. Start a new one when the goal, repository or problem changes.

## 4. Keep persistent instructions lean

Persistent instructions are valuable when they prevent recurring mistakes. They are wasteful when they contain long, rarely relevant process documentation.

- Keep universal instructions short and specific.
- Use path-specific instructions for local rules.
- Move occasional detailed workflows into skills.
- Remove duplicated or contradictory files.
- Review instructions after architecture or tool changes.

## 5. Choose a model for the task

- Use stronger reasoning models for architecture, difficult debugging and ambiguous design work.
- Use mid-tier models when the plan is clear and execution remains non-trivial.
- Use lighter models for routine, well-scoped transformations and documentation.
- Prefer Auto where appropriate; GitHub currently documents cost-aware routing and cache-boundary behaviour.
- Avoid switching models repeatedly during one task because it can disrupt prompt caching.

A cheaper model is not cheaper if poor results require several retries. Measure end-to-end completion, not just the price of one call.

## 6. Research and plan before expensive implementation

For complex or unclear work:

1. Ask for read-only discovery and a plan.
2. Resolve ambiguities and review the proposed scope.
3. Implement once the plan is sound.
4. Validate against explicit acceptance criteria.

Planning has a cost, but it can prevent larger wrong changes and repeated rework. Skip formal planning for obvious, small tasks.

## 7. Control tools and their output

- Enable only relevant MCP servers and tools.
- Use a read-only agent for review or investigation.
- Filter commands to produce focused output.
- Ask for failure summaries before full logs.
- Avoid reading generated, vendored or build-output directories without a reason.
- Put repeatable mechanical steps into scripts.

Large tool results consume context. Current Copilot CLI mitigates very large outputs by storing them in temporary files and supplying a preview, but users should still design focused commands.

## 8. Use subagents deliberately

Subagents can reduce pressure on the main context by doing focused work in a separate window. They also create more model work.

Use them when:

- Research is independent and likely to produce large intermediate results.
- Several genuinely independent analyses can run in parallel.
- A lower-cost specialist model can complete a bounded subtask.

Avoid them when:

- The subtask is tiny.
- Several workers would inspect the same files and duplicate effort.
- The work is tightly coupled and requires constant shared context.

## 9. Preserve useful cache boundaries

Stable input can allow prompt caching, reducing latency and cost. Changing large parts of the context or switching models can reduce reuse.

Practical habits:

- Keep stable instructions at the beginning of a session.
- Avoid unnecessary model switches mid-task.
- Compact at a natural phase boundary rather than randomly.
- Use a fresh session when the task changes completely.
- Use VS Code's [Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer) when investigating actual cache behaviour. It compares consecutive model requests and shows where the matching prompt prefix diverges.

## 10. Detect and stop wasteful loops

Redirect or stop when Copilot:

- Repeats the same command without learning from the result.
- Alternates between two unsuccessful edits.
- Searches increasingly unrelated parts of the repository.
- Continues after the acceptance criteria are already met.
- Produces changes faster than they can be reviewed.

Give the missing constraint, reduce scope, ask for a plan or start a clean session.

## Monitor rather than guess

- Use the context-window control and Cache Explorer in VS Code.
- Use `/context` and `/usage` in Copilot CLI.
- Review GitHub's AI usage page or organisation reporting.
- Set budgets and session limits where appropriate.
- Compare credits, latency, quality and rework—not credits alone.

Pricing, included allowances and product terminology change. Do not copy numeric plan allowances into internal policy without linking and dating the source.

## A compact working pattern

```text
1. Start a clean session for the task.
2. State outcome, scope, constraints and evidence.
3. Attach or name only the most relevant context.
4. Plan first if the task is ambiguous or high risk.
5. Use an appropriate model and toolset.
6. Let Copilot implement and validate.
7. Review the diff and test evidence.
8. Record durable decisions outside the chat.
```

## Sources

- [Optimizing AI usage to maximize efficiency and reduce cost](https://docs.github.com/en/enterprise-cloud@latest/copilot/tutorials/optimize-ai-usage)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Monitoring GitHub AI Credits usage](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage)
- [Setting an AI-credit session limit in Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/set-session-limit)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [About Copilot auto model selection](https://docs.github.com/en/copilot/concepts/models/auto-model-selection)
- [VS Code February 2026 release notes: context compaction](https://code.visualstudio.com/updates/v1_110)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Managing context in Copilot CLI](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/copilot-cli/context-management)
- [Copilot CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)
