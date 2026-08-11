# Custom instructions and AGENTS.md

_For developers and repository maintainers · Last reviewed 11 August 2026_

> [Tokens and context windows](../Tokens-and-Context-Windows.md) explains why always-loaded instructions can affect focus and usage.

Custom instructions are persistent guidance that Copilot applies automatically. They are ideal for concise facts and expectations that are useful across a broad scope.

[[_TOC_]]

## Common forms

| Form | Typical location | Intended scope |
| --- | --- | --- |
| Repository-wide Copilot instructions | `.github/copilot-instructions.md` | Most Copilot work in the repository |
| Path-specific instructions | `.github/instructions/NAME.instructions.md` | Files matching the `applyTo` pattern in the YAML header |
| Agent instructions | `AGENTS.md` | Standing guidance shared across supporting AI agents |
| Organisation instructions | Configured at organisation level | Supported Copilot interactions for organisation members |

Support varies by client and Copilot feature. Check [GitHub's custom-instructions support table](https://docs.github.com/en/copilot/reference/custom-instructions-support) before standardising a layout.

## What belongs in always-on instructions

- The repository's purpose and a compact architecture map
- The actual build, test and lint commands
- Rules that genuinely apply to nearly every change
- A few critical safety boundaries
- Pointers to authoritative documentation, not copies of every document

Example:

```markdown
# Repository guidance

- This is a TypeScript monorepo managed with pnpm workspaces
- Run `pnpm test` for unit tests and `pnpm lint` before completion
- Do not change public API contracts without calling it out explicitly
- Prefer existing components from `packages/ui` over creating duplicates
```

## What should move elsewhere

Long runbooks, reusable one-off requests, specialist roles and enforceable checks do not belong in a general instruction file. The [technology chooser](Choose-the-right-technology.md) shows the better home for each of these.

## Context and usage impact

**Documented:** in IDE chat, repository instructions are automatically added to relevant requests and can appear in the response's References list.

**Practical consequence:** every always-on line competes with the user message, history, code and tool results for context. It may also be sent repeatedly across rounds, though provider caching and product-specific prompt assembly affect billed usage and latency.

A 1,000-token instruction file is eligible to occupy that much context repeatedly, although caching and prompt assembly affect the newly billed amount. [T01](../To-Test.md#t01-always-on-instruction-cost) measures the exact impact.

## Path-specific instructions

Use path-specific files when guidance is important but only for a component, language or content type.

```markdown
---
applyTo: "docs/**/*.md"
---

- Write for a developer audience
- Use sentence case headings
- Include a tested example for every public command
```

If a matching path-specific file and repository-wide instructions both apply, both may be used. Keep the universal file genuinely universal and put local detail in the scoped file.

## `AGENTS.md` requires surface awareness

`AGENTS.md` is useful when the same repository guidance should work across different agents and tools. Discovery differs between Copilot clients:

- VS Code automatically applies a root-level `AGENTS.md` to workspace chat requests
- Nested `AGENTS.md` discovery is experimental in VS Code: when enabled, the editor adds the discovered paths to chat context and lets the agent decide which instructions are relevant
- Support and discovery behaviour can differ in Visual Studio and JetBrains

Avoid relying on an undocumented universal precedence rule. [T02](../To-Test.md#t02-instruction-discovery-and-precedence) tests the client and version used by the team.

To check whether instructions were applied, open the references attached to a Copilot response. Visual Studio shows a **References** section below the response. In VS Code, Chat Diagnostics and the request details expose applied customisations for supported sessions.

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
- [Your first custom instructions](https://docs.github.com/en/copilot/tutorials/customization-library/custom-instructions/your-first-custom-instructions)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
