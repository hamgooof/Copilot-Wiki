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

This differs from manually selecting tools and typing a one-off prompt because the complete worker setup can be selected again by the team.

### Example: Reviewer

```markdown
---
name: Reviewer
description: Review repository changes and return evidence-backed findings.
tools: ['read', 'search']
---

Work as an independent code reviewer.

- Do not edit production files
- Inspect enough surrounding code to validate each finding
- Separate confirmed defects from questions or assumptions
- Use the `review-changes` skill when it is available
- Return findings in the format required by that skill
```

Tool names and supported fields vary by client. Treat this as an illustrative profile and use the target IDE's editor or documentation to select valid tools.

Save a shared repository agent under `.github/agents`. In VS Code, select it from the agent picker. Current Visual Studio custom-agent support requires Visual Studio 2026 18.4 or later. JetBrains support remains Preview.

### When the custom agent earns its place

Use the ordinary Ask or Agent experience for a one-off review. Create the Reviewer agent when the team repeatedly wants the same worker role, tool configuration, model choice or follow-up behaviour.

The Reviewer can use several skills without owning their detailed checklists:

- `review-changes`
- `security-review`
- `api-contract-review`
- `frontend-quality-review`

Those skills remain reusable by other agents and direct user requests.

## Handoffs

VS Code custom agents can offer buttons that move from one agent to another with a pre-filled prompt and relevant conversation context.

A handoff is useful when continuity is wanted, such as Plan to implementation. It should not be described as a clean independent review because the next agent continues with relevant context from the existing chat.

For a less anchored review, start a new chat and select the Reviewer, or delegate a scoped review to a subagent where supported.

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

By default, a VS Code subagent inherits the main agent, model and tools. A named custom agent can override those settings for the delegated task.

Visual Studio does not currently support subagents. JetBrains support is Preview.

## When a subagent helps

- A focused investigation would create a large amount of intermediate context
- A review benefits from a fresh perspective
- Independent questions can be investigated separately
- The main agent should receive a concise result rather than every intermediate step

Avoid delegation for tiny or tightly coupled changes. It adds another model interaction and the parent still needs to validate the result.

## Skill, custom agent or subagent

- **Skill:** reusable procedure used by the current worker
- **Custom agent:** reusable worker configuration
- **Subagent:** separate runtime worker handling a delegated task

The [technology chooser](Choose-the-right-technology.md#skill-or-custom-agent) shows all three in one feature-development example.

## Sources

- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
