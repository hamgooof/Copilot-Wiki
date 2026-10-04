# Custom agents and subagents

_For developers using Copilot agent mode in a supported IDE - Last reviewed 4 October 2026_

A **custom agent** is a saved Copilot setup (role, tools and model) that you pick from the agent list. A **subagent** is a separate run that Copilot starts to do one task and report back. In VS Code, a custom agent can also run as a subagent.

[[_TOC_]]

## Custom agent

A custom agent can define:

- Its continuing role and instructions
- The tools available to it
- A preferred model, depending on the IDE
- Whether people can select it
- Whether another agent may delegate work to it
- Suggested handoffs to another agent

Think of a custom agent as a vehicle fitted out for one job, with a chosen engine and only the equipment that job needs.

The point is reuse: instead of choosing tools and retyping the role each time, anyone on the team picks the agent.

### Example: Reviewer

This example focuses on the role. Add tools through your IDE rather than copying tool names between IDEs. The `review-changes` skill needs the branch diff as well as surrounding code, so in VS Code, open the `.agent.md` file and use the tools picker to choose read, search and source-control tools; it writes valid names for you.

```markdown
---
name: Reviewer
description: Review repository changes and return evidence-backed findings.
---

Work as an independent code reviewer.

- Do not edit production files
- Inspect enough surrounding code to validate each finding
- Separate confirmed defects from questions or assumptions
- Use the `review-changes` skill when it is available
- Return findings in the format required by that skill
```

Save a shared repository agent as `.github/agents/<name>.agent.md`, for example `.github/agents/reviewer.agent.md`. Select it in your IDE:

- **VS Code:** choose it from the agent picker in Copilot Chat
- **Visual Studio 2026 18.4 or later:** choose it from the agent picker or type `@` followed by the custom-agent name
- **JetBrains IDEs (preview):** choose it from Copilot Chat's agent selector

If the agent does not appear, check the file is in `.github/agents/` and that your IDE or plugin is up to date.

### When the custom agent earns its place

Use the ordinary Ask or Agent experience for a one-off review. Create the Reviewer agent when the team repeatedly wants the same worker role, tool configuration, model choice or follow-up behaviour.

The Reviewer can use `review-changes` and other relevant skills without owning their detailed checklists. Those procedures remain reusable by other agents and direct user requests.

Create a role only when its tool restrictions, model selection, handoffs or continuing behaviour will be reused.

## Handoffs

VS Code custom agents can offer buttons that move from one agent to another with a pre-filled request and the conversation so far.

Use a handoff when the next agent needs everything so far, for example going from planning to implementation. A handoff is changing driver without stopping: the new driver gets the full journey log, including every tool call and result. It does not start a clean context or save the plan anywhere, and the cache usually starts again. For a clean start, save the plan to a file and open a new chat.

![A planning conversation moving through a normal agent transition that replaces agent instructions but retains visible messages and tool traffic, compared with a fresh context boundary](../Media/custom-agent-context-boundaries.svg =900x)

| Transition | What moves | Useful when |
| --- | --- | --- |
| Handoff | The pre-filled request and the whole visible conversation, including tool calls and results | The next agent needs planning context and a visible transition |
| New chat from a saved plan | Only the context the new session reads, including the plan file | A clean conversation and durable artefact are more valuable than continuity |
| Subagent | A bounded brief to an isolated worker, followed by its result | One investigation or review should not fill the parent context |

Keep `send: false` when a person should inspect or edit the next request:

```yaml
handoffs:
  - label: Review and start implementation
    agent: plan-implementer
    prompt: Implement only the approved plan above.
    send: false
```

`send` defaults to `false`. Setting it to `true` submits the next request automatically and removes that manual pause.

For a review that is not swayed by the earlier conversation, start a new chat or use a subagent. [Spec-Driven Development](../Spec-Driven-Development.md#move-from-planning-to-implementation-in-vs-code) explains when each transition fits.

## Select models for each phase

In current VS Code custom-agent frontmatter, the top-level `model` accepts either one model:

```yaml
model: GPT-5.4 mini (copilot)
```

or a prioritised fallback array:

```yaml
model:
  - GPT-5.4 mini (copilot)
  - GPT-5 mini (copilot)
```

The fallback array is VS Code-specific. Other IDEs that read `.github/agents/*.agent.md` document a single model name, so use one name if the agent is shared across IDEs.

VS Code tries the array in order until a model is available. If `model` is absent, the model picker selection is used. Copy model names exactly as your model picker shows them.

A handoff can also name a one-off model (`handoffs.model`); prefer setting the model on the target agent instead.

One way to set this up:

1. Leave `model` unset on a planning agent so the person deliberately selects a reasoning-capable model
2. Put a balanced or efficient model, or fallback array, on the reusable implementation agent
3. Keep the handoff's `send: false` review pause

For the built-in Plan flow, the corresponding current settings are `chat.planAgent.defaultModel` and `github.copilot.chat.implementAgent.model`.

> If an agent seems to ignore its `model` setting, update VS Code and check again. Older versions did not always apply it.

A strong planner plus a cheaper implementer makes each step easier to check. It is not automatically cheaper: planning, the cache restart and the review all add model calls.

## Subagents

In VS Code Local sessions, a subagent is a stateless delegated worker with its own context. The parent passes a task, the child works independently, and only its result returns to the parent.

A subagent is a second vehicle sent on an errand: only its delivery note comes back.

The parent cannot send a follow-up message to the same completed subagent, so the delegation should include:

- The exact question or scope
- Important constraints
- The evidence to inspect
- The expected output format

Example:

```text
Use the Reviewer agent as a subagent.
Review the current branch against develop.
Return only Critical, Major and Minor findings with file and symbol locations.
Do not edit files.
```

Current VS Code documentation gives this model priority for a subagent:

1. A model explicitly requested by the parent when invoking the subagent
2. The custom agent's top-level `model`, including its fallback array
3. The parent conversation's model

When you pick its model yourself, a subagent can run on the same model as the parent or a cheaper one, never a more expensive one; letting VS Code choose the model (Auto) is the exception. It can still cost more overall, because it builds its own context and makes its own model calls.

Subagents are documented in detail for VS Code and are in preview in JetBrains IDEs. Visual Studio does not list them.

## When a subagent helps

- A focused investigation would create a large amount of intermediate context
- A review benefits from a fresh perspective
- Independent questions can be investigated separately
- The main agent should receive a concise result rather than every intermediate step

Avoid delegation for tiny or tightly coupled changes. It adds more model calls and the parent still needs to validate the result.

## Skill, custom agent or subagent

See the [technology chooser](Choose-the-right-technology.md#skill-or-custom-agent) for the full comparison and feature-development scenarios.

## Sources

- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Sessions and handoff in VS Code](https://code.visualstudio.com/docs/agents/concepts/sessions)
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
