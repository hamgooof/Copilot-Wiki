# Agent skills

_For developers creating repeatable Copilot workflows - Last reviewed 4 October 2026_

An agent skill is a playbook for one kind of task: a `SKILL.md` file, plus any scripts, examples or templates it needs. Copilot always sees each skill's name and description. It loads the full playbook only when your task matches, or when you call the skill by name. If the model is the engine, a skill is a route card any vehicle can pick up when the job matches.

[[_TOC_]]

## When a skill helps

Use a skill when a task needs:

- A repeatable set of steps
- A checklist or expected output format
- Detailed guidance that should load only for relevant work
- Supporting scripts, examples or references

A skill changes how Copilot does the task, not who does it. The agent, its model and its tools stay the same.

## Project and personal skills

Store a team-owned project skill under:

```text
.github/skills/<skill-name>/SKILL.md
```

Project skills can be reviewed and versioned with the repository. Personal skills in `~/.copilot/skills` work in all your projects.

Every skill's name and description is sent with every model call. Keep the description short and specific: a vague one costs a little every time and can make Copilot pick the wrong skill.

![A skill's name and description are sent with every model call, while its body and linked resources load only when the task selects them](../Media/skill-progressive-loading.svg =760x)

## Example: review branch changes

This skill runs a full review by itself. You do not need a custom agent.

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
description: Review the current branch against a supplied base reference. Use before merging or for a peer review.
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
/review-changes develop
```

Use skills in each IDE:

- **VS Code:** describe the task and Copilot picks the skill, or run `/review-changes develop`
- **Visual Studio 2026 18.5 or later:** Copilot discovers applicable skills automatically. To make your intent clear, ask it to use the named `review-changes` skill; do not rely on the VS Code slash-command syntax
- **Rider and WebStorm (preview):** describe the task, or ask Copilot to use the `review-changes` skill by name

If you need a particular skill, name it in your request and check the response shows it was used.

## Skill or custom agent

A custom Reviewer agent can use this skill without copying its checklist. See [Choose the right Copilot technology](Choose-the-right-technology.md#skill-or-custom-agent) for the full skill, custom-agent and subagent scenarios.

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
- Adding scripts without checking what they do and what they can access

## Design checklist

- Give the skill one recognisable job
- Say what it does and when to use it
- Keep the main workflow concise
- Link large examples and references
- State preconditions, validation and expected output
- Check that Copilot picks the skill when you describe the task, and when you call it by name
- Review the result like any other repository change

## Sources

- [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
