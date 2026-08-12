# Agent skills

_For developers creating repeatable Copilot workflows - Last reviewed 12 August 2026_

An agent skill is a reusable playbook for performing one kind of task. It can contain instructions, scripts, examples, templates and reference material. The active agent loads it when relevant, or the user invokes it directly where the client supports that behaviour.

[[_TOC_]]

## When a skill helps

Use a skill when a task needs:

- A repeatable set of steps
- A checklist or expected output format
- Detailed guidance that should load only for relevant work
- Supporting scripts, examples or references

A skill changes the method used for a task. It does not normally replace the active agent's overall role, model or tool configuration.

## Project and personal skills

Store a team-owned project skill under:

```text
.github/skills/<skill-name>/SKILL.md
```

Project skills can be reviewed and versioned with the repository. Personal skills under `~/.copilot/skills` are available across projects in clients that support them.

Current VS Code documentation says skill discovery starts with the name and description, the `SKILL.md` body loads when the skill is used, and referenced resources are read as needed. The exact discovery cost and controls can differ between clients and versions.

## Example: review branch changes

This skill performs a complete, structured review without requiring a custom agent.

```text
.github/
  skills/
    review-changes/
      SKILL.md
      review-checklist.md
```

`SKILL.md`:

```markdown
---
name: review-changes
description: Review the current branch against a supplied base reference. Use for a structured pre-PR or peer review.
argument-hint: "[base branch or commit]"
---

# Review changes

1. Identify the supplied base reference and determine the branch diff
2. Read the changed code and enough surrounding implementation to judge behaviour
3. Apply [the review checklist](review-checklist.md)
4. Report only actionable findings supported by evidence
5. Do not edit files unless the user asks for fixes in a later turn

Return findings in this order: Critical, Major, Minor.

For each finding use:

`<Severity><number> - <Issue title>`

- Summary
- Location
- Recommended fix

If no findings survive review, say so and list the checks completed.
```

In current VS Code, run it as:

```text
/review-changes origin/develop
```

Use skills according to the client:

- **VS Code:** select the skill automatically by describing a matching task, or invoke it directly with `/review-changes origin/develop`
- **Visual Studio 2026 18.5 or later:** Copilot discovers applicable skills automatically. To make your intent clear, ask it to use the named `review-changes` skill; do not rely on the VS Code slash-command syntax
- **JetBrains:** agent skills are a preview feature. Let Copilot select an applicable skill automatically, or ask it to use the skill by name; check the installed plugin before documenting a direct UI control

Automatic selection depends on a clear skill name and description. If using a particular playbook matters, name it in the request and check the agent's references or progress rather than assuming it loaded.

## Skill or custom agent

This page's example is a skill because the reusable asset is the review procedure. A custom Reviewer agent can use it without duplicating the checklist. See [Choose the right Copilot technology](Choose-the-right-technology.md#skill-or-custom-agent) for the full skill, custom-agent and subagent scenarios.

## Other useful code-repository skills

- **API compatibility review:** check .NET endpoint and DTO changes against Angular clients
- **Project test workflow:** select and run the focused .NET or Angular tests using repository conventions
- **Call-path tracing:** trace an Angular action through client services, a .NET endpoint and application layers
- **Authentication review:** inspect authentication and authorisation changes across API and front-end boundaries
- **Error-handling review:** check HTTP responses, problem details and front-end user feedback together
- **Dependency update review:** identify breaking API changes and focused regression tests

## Common mistakes

- A vague description such as "helps with code"
- Repeating universal repository rules in every skill
- Loading the main file with reference material that could live in linked resources
- Creating several overlapping skills whose descriptions match the same task
- Treating natural-language steps as deterministic enforcement
- Bundling scripts without reviewing their trust, permissions and portability

## Design checklist

- Give the skill one recognisable job
- Say what it does and when to use it
- Keep the main workflow concise
- Link large examples and references
- State preconditions, validation and expected output
- Test both automatic selection and explicit invocation where supported
- Review the result like any other repository change

## Sources

- [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
