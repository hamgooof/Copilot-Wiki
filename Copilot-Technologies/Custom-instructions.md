# Custom instructions and AGENTS.md

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Draft technology page | Developers and repository maintainers | 8 August 2026 |

> Start with [Tokens and context windows](../Tokens-and-Context-Windows.md) if it is not yet clear why always-loaded instructions can affect focus and usage.

Custom instructions are persistent guidance that Copilot applies automatically. They are ideal for concise facts and expectations that are useful across a broad scope.

## Common forms

| Form | Typical location | Intended scope |
| --- | --- | --- |
| Repository-wide Copilot instructions | `.github/copilot-instructions.md` | Most Copilot work in the repository |
| Path-specific instructions | `.github/instructions/NAME.instructions.md` | Files matching the frontmatter `applyTo` pattern |
| Agent instructions | `AGENTS.md` | Standing guidance shared across supporting AI agents |
| Personal CLI instructions | `~/.copilot/copilot-instructions.md` and `~/.copilot/instructions/**/*.instructions.md` | That user's Copilot CLI sessions |
| Organisation instructions | Configured at organisation level | Supported Copilot interactions for organisation members |

Support varies by client and Copilot feature. Check [GitHub's custom-instructions support table](https://docs.github.com/en/copilot/reference/custom-instructions-support) before standardising a layout.

## What belongs in always-on instructions

- The repository's purpose and a compact architecture map.
- The actual build, test and lint commands.
- Rules that genuinely apply to nearly every change.
- A few critical safety boundaries.
- Pointers to authoritative documentation, not copies of every document.

Example:

```markdown
# Repository guidance

- This is a TypeScript monorepo managed with pnpm workspaces.
- Run `pnpm test` for unit tests and `pnpm lint` before completion.
- Do not change public API contracts without calling it out explicitly.
- Prefer existing components from `packages/ui` over creating duplicates.
```

## What should move elsewhere

| Content | Better home |
| --- | --- |
| A detailed release or migration runbook | Skill |
| A one-off code review prompt | Prompt file |
| Rules only for `docs/**` | Path-specific instructions |
| A security reviewer persona with read-only tools | Custom agent |
| A validation command that must always run | Hook or CI |
| A large architecture reference | Repository documentation retrieved when needed |

## Context and usage impact

**Documented:** Copilot CLI loads custom instruction files at session start. Current CLI documentation says supported instruction locations are merged simultaneously. In IDE chat, repository instructions are automatically added to relevant requests and can appear in the response's References list.

**Practical consequence:** every always-on line competes with the user request, history, code and tool results for context. It may also be sent repeatedly across turns, though provider caching and product-specific request assembly affect billed usage and latency.

This does **not** mean a 1,000-token instruction file always creates exactly 1,000 newly billed tokens on every turn. It means the file is eligible to occupy context repeatedly. Exact credit and cache behaviour is a **To Test** item.

## Path-specific instructions

Use path-specific files when guidance is important but only for a component, language or content type.

```markdown
---
applyTo: "docs/**/*.md"
---

- Write for a developer audience.
- Use sentence case headings.
- Include a tested example for every public command.
```

If a matching path-specific file and repository-wide instructions both apply, both may be used. Keep the universal file genuinely universal and put local detail in the scoped file.

## `AGENTS.md` requires surface awareness

`AGENTS.md` is useful when the same repository guidance should work across different agents and tools. Do not assume every Copilot surface discovers the same files in the same way:

- GitHub documents `AGENTS.md` support for the cloud coding agent, code review, VS Code agent use and Copilot CLI, with client-specific differences.
- VS Code automatically applies a root-level `AGENTS.md` to workspace chat requests. Nested `AGENTS.md` discovery is experimental: when enabled, VS Code adds the paths of recursively discovered files to chat context and lets the agent decide which instructions are relevant.
- Current Copilot CLI documentation says multiple supported instruction files are merged and lists both Git-root and current-working-directory locations.

Therefore, avoid relying on an undocumented universal precedence rule. Test the client and version used by the team.

## Review checklist

- Can the file be read in under two minutes?
- Does every rule apply to most work in its scope?
- Are any rules duplicated or contradictory?
- Are build and test commands still correct?
- Could long examples become a linked document or skill resource?
- Are path-specific rules separated from repository-wide rules?
- Is deterministic enforcement implemented outside the prompt?
- Has someone verified the file appears in Copilot's References/context view?

## Sources

- [Adding repository custom instructions in an IDE](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)
- [Support for different types of custom instructions](https://docs.github.com/en/copilot/reference/custom-instructions-support)
- [Copilot CLI command reference: custom instruction locations](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)
- [Your first custom instructions](https://docs.github.com/en/copilot/tutorials/customization-library/custom-instructions/your-first-custom-instructions)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
