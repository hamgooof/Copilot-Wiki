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

Skills appear as slash commands in VS Code by default and can also be selected automatically. Visual Studio and JetBrains invocation controls differ, so check the installed client before teaching one command across the team.

## Skill or custom agent

- Use the `review-changes` **skill** when the reusable asset is the review procedure
- Add a `Reviewer` **custom agent** when the reusable asset also includes a worker role, chosen model, restricted tools or isolated delegation

A Reviewer agent can use `review-changes` rather than duplicating the checklist. It might also use separate `security-review` or `api-contract-review` skills when those procedures are substantial and independently useful.

Start with one complete skill. Split it only when a section is reused elsewhere, has its own supporting resources or should load selectively.

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
