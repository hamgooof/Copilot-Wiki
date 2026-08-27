# Custom agents and subagents

_For developers using Copilot agent mode in a supported IDE - Last reviewed 27 August 2026_

A **custom agent** defines a reusable worker configuration. A **subagent** is a separate worker created to complete a delegated task. A custom agent can be used directly or, where supported, invoked as a subagent.

[[_TOC_]]

## Custom agent

A custom agent can define:

- Its continuing role and instructions
- The tools available to it
- A preferred model, depending on the client
- Whether people can select it
- Whether another agent may delegate work to it
- Suggested handoffs to another agent

This differs from manually selecting tools and typing a one-off request because the complete worker setup can be selected again by the team.

### Example: Reviewer

This example focuses on the role. Add the valid read, search and source-control tools through the target IDE's configuration rather than copying tool names between clients.

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

The `review-changes` skill needs access to the branch diff as well as surrounding code. Tool names and supported fields vary by client, so use the target IDE's editor or documentation to select valid read, search and source-control or read-only terminal capabilities.

Save a shared repository agent as `.github/agents/<name>.agent.md`, for example `.github/agents/reviewer.agent.md`. Select it according to the client:

- **VS Code:** choose it from the agent picker in Copilot Chat
- **Visual Studio 2026 18.4 or later:** choose it from the agent picker or type `@` followed by the custom-agent name
- **JetBrains:** custom agents are a preview feature; choose a discovered agent through Copilot Chat's agent selector, whose label can vary by plugin version

If the expected agent is absent, check the file location, installed client version and organisation policy before relying on it.

### When the custom agent earns its place

Use the ordinary Ask or Agent experience for a one-off review. Create the Reviewer agent when the team repeatedly wants the same worker role, tool configuration, model choice or follow-up behaviour.

The Reviewer can use `review-changes` and other relevant skills without owning their detailed checklists. Those procedures remain reusable by other agents and direct user requests.

## Handoffs

VS Code custom agents can offer buttons that move from one agent to another with a pre-filled request and relevant conversation context.

A handoff is useful when continuity is wanted, such as planning to implementation. It is not a fresh context boundary and it does not make the plan durable.

VS Code uses similar handoff language for more than one transition. The custom-agent documentation describes handoff buttons as moving with **relevant context**. The session documentation separately says that changing a session's target carries its conversation history and context, while the built-in Plan documentation says **Start Implementation** carries the plan and conversation context. Do not treat those descriptions as one stronger promise about custom-agent context selection.

> **Observed, not contractual:** local VS Code 1.131 and Claude Haiku 4.5 probes on 27 August 2026 exactly continued every visible user and assistant message through normal handoffs and direct agent selection. The target system prompt replaced the source agent instructions, but a real edit's visible tool call, arguments and result also reached the tester. Completed-turn reasoning was absent from the later handoff request, although reasoning was reused inside the implementer's active post-tool loop. Every tested agent change reported zero cached input while later same-agent requests could use cached input. This combination can make a long handoff expensive. The result covers small, uncompacted conversations on the recorded version; it is not a permanent full-history or cache contract.

![A planning conversation moving through a normal agent transition that replaces agent instructions but retains visible messages and tool traffic, compared with a fresh context boundary](../Media/custom-agent-context-boundaries.svg =900x)

| Transition | What moves | Useful when |
| --- | --- | --- |
| Handoff | The pre-filled request and relevant conversation context | The next agent needs planning context and a visible transition |
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

For an independent review with less prior influence, begin from a fresh chat or use an isolated subagent. [Spec-Driven Development](../Spec-Driven-Development.md#move-from-planning-to-implementation-in-vs-code) explains when each transition fits.

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

The fallback array is VS Code-specific. Other Copilot surfaces that can read `.github/agents/*.agent.md` currently document a single model name, so check the target surface before sharing an array across clients.

VS Code tries the array in order until a model is available. If `model` is absent, the model picker selection is used. Availability depends on the Copilot plan, organisation policy and installed client, so confirm the exact names in the picker before sharing a configuration.

This supports a human-directed option:

1. Leave `model` unset on a planning agent so the person deliberately selects a reasoning-capable model
2. Put a balanced or efficient model, or fallback array, on the reusable implementation agent
3. Keep the handoff's `send: false` review pause

For the built-in Plan flow, the corresponding current settings are `chat.planAgent.defaultModel` and `github.copilot.chat.implementAgent.model`.

### Keep the handoff model separate

`handoffs.model` is a different field. It accepts one qualified model name for that transition, not a fallback array:

```yaml
handoffs:
  - label: Start implementation with a one-off model
    agent: plan-implementer
    prompt: Implement the approved plan above.
    send: false
    model: GPT-5.4 mini (copilot)
```

Current VS Code implementation applies this as a transition override after selecting the target agent, ahead of the target agent's top-level `model`. The public schema documents both fields but does not publish that complete precedence as a compatibility promise. Prefer configuring the implementation model in one place. Usually that means setting the target agent's fallback list and omitting `handoffs.model`; use the handoff field only for an intentional one-off override.

> **Version-sensitive behaviour:** retest model routing after material VS Code or Copilot updates. An older local experiment found custom-agent model pins were ignored, while current documentation and implementation support them.

A capable planner and narrower worker can improve reviewability and reduce task ambiguity. Planning, cache rebuilding, workers and review also add model work, so this is not evidence that the combined workflow is cheaper. Use [matched workflow trials](../Spec-Driven-Development.md#evaluate-the-workflow).

## Subagents

Current VS Code documentation describes a subagent as a stateless delegated worker with its own context. The parent passes a task, the child works independently, and only its result returns to the parent.

The parent cannot send a follow-up message to the same completed subagent, so the delegation should include:

- The exact question or scope
- Important constraints
- The evidence to inspect
- The expected output format

Example:

```text
Use the Reviewer agent as a subagent.
Review the current branch against origin/develop.
Return only Critical, Major and Minor findings with file and symbol locations.
Do not edit files.
```

Current VS Code documentation gives this model priority for a subagent:

1. A model explicitly requested by the parent when invoking the subagent
2. The custom agent's top-level `model`, including its fallback array
3. The parent conversation's model

An explicitly requested subagent model cannot exceed the cost tier of the parent model. A high-tier coordinator can therefore route to a narrower worker in the same or a lower tier, subject to model availability and organisation policy. The subagent's separate context and additional model calls can still increase total AI-credit use.

The current first-party documentation describes the delegated workflow in detail for VS Code. Current Visual Studio documentation does not list subagents as supported, while GitHub lists JetBrains support as preview. Check the installed JetBrains plugin before relying on the feature, and avoid teaching the VS Code controls as a cross-client workflow.

## When a subagent helps

- A focused investigation would create a large amount of intermediate context
- A review benefits from a fresh perspective
- Independent questions can be investigated separately
- The main agent should receive a concise result rather than every intermediate step

Avoid delegation for tiny or tightly coupled changes. It adds another model interaction and the parent still needs to validate the result.

## Skill, custom agent or subagent

A skill supplies a reusable procedure, a custom agent supplies a reusable worker configuration, and a subagent supplies an isolated delegated run. See the [technology chooser](Choose-the-right-technology.md#skill-or-custom-agent) for the full comparison and feature-development scenarios.

Planning, implementation and review are workflow phases. They do not each need a custom agent. Create a role only when its tool restrictions, model selection, handoffs or continuing behaviour will be reused.

## Sources

- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Sessions and handoff in VS Code](https://code.visualstudio.com/docs/agents/concepts/sessions)
- [Current VS Code handoff model implementation](https://github.com/microsoft/vscode/blob/4227cbcd9ecacd36425e1cccbcb36c12809394c2/src/vs/workbench/contrib/chat/browser/widget/chatWidget.ts#L1768-L1786)
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
