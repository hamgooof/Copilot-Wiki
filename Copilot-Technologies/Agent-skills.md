# Agent skills

_For developers creating repeatable Copilot workflows · Last reviewed 11 August 2026_

> This page assumes the basic terms from [Copilot terminology](../Copilot-Terminology.md). The short version: a skill gives an existing agent a reusable procedure for a particular type of work.

An agent skill is a folder containing a required `SKILL.md` and optional scripts, examples and reference material. Copilot loads it when the task is relevant, giving the current agent specialist instructions without making the whole workflow permanently active.

[[_TOC_]]

## Why skills matter

Skills solve two problems at once:

- They standardise a repeatable workflow
- They defer detailed instructions until they are needed

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

Project skills can currently be placed under `.github/skills`, `.claude/skills` or `.agents/skills`. VS Code also documents personal locations under `~/.copilot/skills`, `~/.claude/skills` and `~/.agents/skills`. Support differs by IDE, so prefer the location explicitly supported by the team's target client.

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

**Documented in VS Code:** Copilot first uses the skill name and description to decide whether it is relevant. When selected, the `SKILL.md` body is loaded into the agent's context. Referenced scripts, examples and other files are accessed as the workflow needs them.

Copilot loads supporting files when the workflow references them. Link them from `SKILL.md` so the agent can retrieve only what it needs.

**To Test:** behaviour still differs between IDEs and versions. Test automatic selection, compaction and context use in the clients supported by your team.

## Skills versus other options

Use a skill for an occasional detailed workflow and its supporting resources. The [customisation chooser](Choose-the-right-technology.md) covers the boundaries with instructions, prompt files, custom agents and deterministic controls.

## Good skill candidates

- Diagnose a GitHub Actions failure using summary-first log inspection
- Prepare release notes using the organisation's format
- Perform a database migration readiness review
- Produce an architecture decision record
- Validate documentation with organisation-specific checks
- Run an incident triage process using bundled scripts and templates

## Common mistakes

- A vague description such as “helps with code.” Copilot cannot route reliably if the trigger is unclear
- Repeating universal repository rules in every skill
- Filling `SKILL.md` with reference material that could live in linked supporting files
- Treating natural-language steps as deterministic enforcement
- Bundling executable scripts without reviewing trust, permissions and portability
- Creating overlapping skills whose descriptions all match the same task

## Design checklist

- Give the skill one recognisable job
- Say what it does and when it should be used in the description
- Keep the core workflow in `SKILL.md` concise
- Move large examples and references into separate files
- Prefer scripts for repeatable mechanical work
- State preconditions, validation and expected output
- Test both automatic selection and explicit invocation
- Measure false-positive invocation and context usage

## Sources

- [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
