# Custom agents and subagents

_For developers using Copilot agent mode in a supported IDE - Last reviewed 12 August 2026_

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

Save a shared repository agent under `.github/agents`. Select it according to the client:

- **VS Code:** choose it from the agent picker in Copilot Chat
- **Visual Studio 2026 18.4 or later:** type `@` followed by the custom-agent name; the agent-picker dropdown is currently limited to Visual Studio 2026 Insiders
- **JetBrains:** custom agents are a preview feature; choose a discovered agent through Copilot Chat's agent selector, whose label can vary by plugin version

If the expected agent is absent, check the file location, installed client version and organisation policy before relying on it.

### When the custom agent earns its place

Use the ordinary Ask or Agent experience for a one-off review. Create the Reviewer agent when the team repeatedly wants the same worker role, tool configuration, model choice or follow-up behaviour.

The Reviewer can use `review-changes` and other relevant skills without owning their detailed checklists. Those procedures remain reusable by other agents and direct user requests.

## Handoffs

VS Code custom agents can offer buttons that move from one agent to another with a pre-filled request and relevant conversation context.

A handoff is useful when continuity is wanted, such as Plan to implementation. For an independent review with less prior influence, begin from a fresh chat or use an isolated subagent.

Start a new chat and select the Reviewer, or delegate a scoped review to a subagent where supported.

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

By default, a VS Code subagent runs on the main conversation's model. A named custom agent can override the model and restrict the tools for the delegated task.

The current first-party documentation describes the delegated workflow in detail for VS Code. Visual Studio does not currently support subagents, while GitHub lists JetBrains support as preview. Check the installed JetBrains plugin before relying on the feature, and avoid teaching the VS Code controls as a cross-client workflow.

## When a subagent helps

- A focused investigation would create a large amount of intermediate context
- A review benefits from a fresh perspective
- Independent questions can be investigated separately
- The main agent should receive a concise result rather than every intermediate step

Avoid delegation for tiny or tightly coupled changes. It adds another model interaction and the parent still needs to validate the result.

## Skill, custom agent or subagent

A skill supplies a reusable procedure, a custom agent supplies a reusable worker configuration, and a subagent supplies an isolated delegated run. See the [technology chooser](Choose-the-right-technology.md#skill-or-custom-agent) for the full comparison and feature-development scenarios.

## Sources

- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
