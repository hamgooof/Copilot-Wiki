# Agent skills

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Draft technology page | Developers creating repeatable Copilot workflows | 8 August 2026 |

> This page assumes the basic terms from [Copilot terminology](../Copilot-Terminology.md). The short version: a skill gives an existing agent a reusable procedure for a particular type of work.

An agent skill is a folder containing a required `SKILL.md` and optional scripts, examples and reference material. Copilot loads it when the task is relevant, giving the current agent specialist instructions without making the whole workflow permanently active.

## On this page

- [Why skills matter](#why-skills-matter)
- [Typical layout](#typical-layout)
- [Minimal skill](#minimal-skill)
- [What is loaded into context](#what-is-loaded-into-context)
- [Skills versus other options](#skills-versus-other-options)
- [Good skill candidates](#good-skill-candidates)
- [Common mistakes](#common-mistakes)
- [Design checklist](#design-checklist)

## Why skills matter

Skills solve two problems at once:

- They standardise a repeatable workflow.
- They defer detailed instructions until they are needed.

GitHub explicitly recommends skills for just-in-time guidance that should not overload the context window for unrelated tasks.

## Typical layout

```text
.github/
  skills/
    release-notes/
      SKILL.md
      template.md
      examples.md
      scripts/
        validate-release.ps1
```

Project skills can currently be placed under `.github/skills`, `.claude/skills` or `.agents/skills`. Personal skills can be placed under `~/.copilot/skills` or `~/.agents/skills`. Support differs by surface, so prefer the location explicitly supported by the team's target client.

## Minimal skill

```markdown
---
name: release-notes
description: Draft and validate release notes from merged pull requests. Use for release-note or changelog preparation.
---

1. Identify merged pull requests for the requested range.
2. Classify user-visible changes, fixes and breaking changes.
3. Draft the notes using [template.md](template.md).
4. Run the validation script.
5. Report missing issue links separately.
```

The description is important. Copilot uses the task and skill description to decide whether the skill is relevant.

## What is loaded into context

**Documented:** when Copilot chooses a skill, the `SKILL.md` content is injected into that agent's context. The files in the skill folder are available to the agent, and instructions can direct it to read or run them.

Do not assume every supporting file is automatically injected in full. GitHub says they are made available; the agent can use referenced scripts and examples. This distinction is why keeping detailed reference material in separate files can be useful.

**To Test:** public documentation does not define, consistently across every surface, exactly how long injected skill content remains present after use, how compaction represents it, or how much metadata about all installed skills occupies the prompt before invocation.

## Skills versus other options

| Requirement | Skill? | Better choice when not |
| --- | --- | --- |
| A detailed workflow used sometimes | Yes | — |
| Scripts/templates travel with the workflow | Yes | — |
| Always-on coding rule | No | Custom instructions |
| A human explicitly launches a single reusable prompt | Sometimes | Prompt file may be simpler |
| Needs a separate context window | No, not by itself | Subagent/custom agent |
| Needs a restricted toolset or different model | No, not by itself | Custom agent |
| Needs live data or external actions | It can guide their use | MCP server supplies the tools |
| Must execute a command every time | No guarantee | Hook or CI |

## Good skill candidates

- Diagnose a GitHub Actions failure using summary-first log inspection.
- Prepare release notes using the organisation's format.
- Perform a database migration readiness review.
- Produce an architecture decision record.
- Validate documentation with organisation-specific checks.
- Run an incident triage process using bundled scripts and templates.

## Common mistakes

- A vague description such as “helps with code.” Copilot cannot route reliably if the trigger is unclear.
- Repeating universal repository rules in every skill.
- Putting all reference content into `SKILL.md` instead of linking supporting files.
- Treating natural-language steps as deterministic enforcement.
- Bundling executable scripts without reviewing trust, permissions and portability.
- Creating overlapping skills whose descriptions all match the same task.

## Design checklist

- Give the skill one recognisable job.
- Say what it does and when it should be used in the description.
- Keep the core workflow in `SKILL.md` concise.
- Move large examples and references into separate files.
- Prefer scripts for repeatable mechanical work.
- State preconditions, validation and expected output.
- Test both automatic selection and explicit invocation.
- Measure false-positive invocation and context usage.

## Sources

- [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Adding agent skills for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Adding agent skills for Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
- [Comparing Copilot CLI customization features](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/comparing-cli-features)
