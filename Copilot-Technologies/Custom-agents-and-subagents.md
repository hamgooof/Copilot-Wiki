# Custom agents and subagents

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Draft technology page | Developers using Copilot agent mode or CLI | 8 August 2026 |

> Read [Requests, turns, tools and the agent loop](../Requests-Turns-Tools-and-Agent-Loops.md) first if agent execution is unfamiliar.

A **custom agent** is a reusable specialist definition. A **subagent** is a separate runtime worker created to complete delegated work. The main agent can invoke a custom agent as a subagent, but the terms describe different things.

## Custom agent

A custom agent profile can define:

- Its role and instructions.
- The tools it may use.
- Optional MCP servers or tools.
- A preferred model, depending on surface.
- Whether people can select it.
- Whether another agent may invoke it automatically.

Example:

```markdown
---
name: security-reviewer
description: Review authentication and authorization changes for exploitable defects.
tools: [read, search]
model: claude-sonnet-4.6
---

Review only. Do not edit files. Report evidence, impact and a minimal remediation.
Ignore formatting and style issues.
```

Property names and supported values vary across Copilot surfaces. Use GitHub's [custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration) and the target IDE documentation rather than assuming a profile is completely portable.

## Subagent

**Documented for Copilot CLI and VS Code:** a subagent runs with its own context window, separate from the main agent and other subagents. This allows exploration, logs or detailed intermediate work to stay out of the main conversation context.

The parent delegates a task. The child works in isolation and returns a result. It is better to think of this as a handoff with a report back, not shared consciousness.

```text
Main agent context
  plan + user conversation
       |
       +--> Explore subagent context --> findings summary
       |
       +--> Test subagent context ----> test result summary
       |
       +--> Review subagent context --> review findings
       |
       +<-- selected results return to main context
```

## Does a subagent inherit the parent's context?

The safe answer is **not automatically in full**.

GitHub documents a separate context window. The child receives a delegated task and can access whatever tools, repository and instructions its configuration provides. The product may supply relevant task context, but a separate window should not be described as a copy of the entire parent's transcript.

If a decision or constraint matters to the child, include it explicitly in the delegation or put it in an instruction source the child is documented to load.

The exact seeding of child context, instruction inheritance and result payload by surface is listed as **To Test**.

## When to use a custom agent

- A reviewer should be read-only.
- A specialist needs a distinct set of MCP tools.
- A task benefits from a consistent role and reporting format.
- A specialist model should handle this class of task.
- A reusable agent should be eligible for automatic delegation.

Avoid a custom agent when the only requirement is some guidance text. A skill is lighter and does not necessarily introduce a separate worker.

## When to use a subagent

- Codebase exploration would generate a large amount of intermediate context.
- Tests, builds or logs can be analysed separately.
- Independent reviews can run in parallel.
- The main agent should remain focused on planning and integration.
- Different subtasks benefit from different specialist models.

Avoid delegation for tiny or tightly coupled changes. Handoffs consume time, credits and context, and the parent must reconcile results.

## Model selection

Model selection is surface-specific:

- **Copilot CLI custom agents:** if the profile does not specify a model, current documentation says it inherits the outer agent's model. A CLI session using `Auto` has special inheritance behaviour documented in the command reference.
- **VS Code subagents:** current VS Code documentation gives precedence to an explicitly requested invocation model, then the custom agent's configured model, then the parent model. It also documents cost-tier restrictions on child model choice.
- **Copilot SDK:** custom agent definitions can override the parent session's model and reasoning effort; SDK semantics should not be assumed to apply to the end-user clients.

Therefore, an “Opus orchestrator, Sonnet children” pattern is feasible only where the selected models are available and that surface supports per-agent routing. Make the intended model explicit and verify it in usage/trace data.

## Coordination risks

- Two agents edit the same file or make incompatible design decisions.
- A child does not receive a constraint that existed only in the parent chat.
- The child returns a summary that omits evidence needed by the parent.
- A cheaper model saves credits but needs retries or creates integration work.
- Recursive delegation multiplies usage and makes audit trails harder to follow.
- The parent accepts reports without validating the resulting repository state.

Use small delegated scopes, explicit deliverables, ownership boundaries and parent-side verification.

## Sources

- [About custom agents in Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents)
- [Creating and using custom agents for Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli)
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Custom agents in VS Code: format and file locations](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Running tasks in parallel with `/fleet`](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet)
- [Copilot SDK custom agents and sub-agent orchestration](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/custom-agents)
