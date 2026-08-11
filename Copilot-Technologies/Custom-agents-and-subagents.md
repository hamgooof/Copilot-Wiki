# Custom agents and subagents

_For developers using Copilot agent mode in a supported IDE · Last reviewed 11 August 2026_

> Read [One prompt, many rounds](../One-Prompt-Many-Rounds.md) first if agent execution is unfamiliar.

A **custom agent** is a reusable specialist definition. A **subagent** is a separate runtime worker created to complete delegated work. The main agent can invoke a custom agent as a subagent, but the terms describe different things.

[[_TOC_]]

## Custom agent

A custom agent profile can define:

- Its role and instructions
- The tools it may use
- A preferred model, depending on surface
- Whether people can select it
- Whether another agent may invoke it automatically

Example:

```markdown
---
name: security-reviewer
description: Review authentication and authorisation changes for exploitable defects.
tools: [read, search]
model: claude-sonnet-4.6
---

Review only. Do not edit files. Report evidence, impact and a minimal remediation.
Ignore formatting and style issues.
```

Save a repository agent as `.github/agents/security-reviewer.agent.md`. In VS Code, choose it from the agent dropdown in Chat; **Chat: New Custom Agent** can create the file for you. Visual Studio also supports repository custom agents. JetBrains support is currently preview. See the [IDE support table](Choose-the-right-technology.md#current-ide-support) before sharing one setup across clients.

Property names and supported values vary across Copilot surfaces. Check GitHub's [custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration) and the target IDE documentation before sharing a profile between clients.

## Subagent

**Documented for VS Code:** a subagent runs with its own context window, separate from the main agent and other subagents. This allows exploration, logs or detailed intermediate work to stay out of the main conversation context.

The parent delegates a task. The child works in isolation and reports its result back to the parent:

1. The main agent sends a scoped task to the subagent
2. The subagent searches, tests or reviews inside its own context
3. Its selected findings return to the main agent as new context

### How subagents are invoked in VS Code

Subagents are normally started by the main agent through the `agent/runSubagent` tool. Check that this tool is enabled before expecting delegation.

You can suggest delegation in an ordinary prompt by asking for isolated research or parallel analysis. You can also request a named custom agent, for example:

```text
Run the security-reviewer agent as a subagent and return the evidence it finds.
```

The main agent still decides whether to delegate. Visual Studio does not currently support subagents; JetBrains support is in preview. Check the [IDE support table](Choose-the-right-technology.md#current-ide-support) as this changes.

## Does a subagent inherit the parent's context?

Assume that the child starts with a separate, deliberately scoped context.

GitHub documents a separate context window. The child receives a delegated task and can access whatever tools, repository and instructions its configuration provides. The product may supply relevant task context, but a separate window should not be described as a copy of the entire parent's transcript.

If a decision or constraint matters to the child, include it explicitly in the delegation or put it in an instruction source the child is documented to load.

The exact seeding of child context and instruction inheritance is covered by [T07](../To-Test.md#t07-parent-to-subagent-context-transfer). The result returned to the parent is covered by [T08](../To-Test.md#t08-subagent-to-parent-return).

## When to use a custom agent

- A reviewer should be read-only
- A specialist needs a distinct set of built-in tools
- A task benefits from a consistent role and reporting format
- A specialist model should handle this class of task
- A reusable agent should be eligible for automatic delegation

Use a skill when the requirement is guidance for a particular workflow. Reserve a custom agent for a distinct role, model or toolset.

## When to use a subagent

- Codebase exploration would generate a large amount of intermediate context
- Tests, builds or logs can be analysed separately
- Independent reviews can run in parallel
- The main agent should remain focused on planning and integration
- Different subtasks benefit from different specialist models

Avoid delegation for tiny or tightly coupled changes. Handoffs consume time, credits and context, and the parent must reconcile results.

## Model selection

Model selection is surface-specific. Current VS Code documentation gives precedence to an explicitly requested invocation model, then the custom agent's configured model, then the parent model. It also documents cost-tier restrictions on child model choice.

An "Opus orchestrator, Sonnet children" pattern is feasible only where the selected models are available and that surface supports per-agent routing. Make the intended model explicit and verify it in usage or trace data.

## Coordination risks

- Two agents edit the same file or make incompatible design decisions
- A constraint mentioned only in the parent chat may be absent from the child's context
- The child returns a summary that omits evidence needed by the parent
- A cheaper model saves credits but needs retries or creates integration work
- Recursive delegation multiplies usage and makes audit trails harder to follow
- The parent accepts reports without validating the resulting repository state

Use small delegated scopes, explicit deliverables, ownership boundaries and parent-side verification.

## Sources

- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Custom agents in VS Code: format and file locations](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
