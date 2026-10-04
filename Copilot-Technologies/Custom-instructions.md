# Custom instructions

_For developers and repository maintainers - Last reviewed 4 October 2026_

Custom instructions are Markdown files that Copilot adds to your chat automatically. Use them for the short facts Copilot needs most of the time: build commands, where things live, team conventions. Think of them as house rules taped to the dashboard: every driver sees them on every trip, so keep them short.

[[_TOC_]]

## The two repository forms

| Form | Location | Scope |
| --- | --- | --- |
| Repository-wide instructions | `.github/copilot-instructions.md` | Work across the repository |
| Path-specific instructions | `.github/instructions/NAME.instructions.md` | Files matching the YAML `applyTo` pattern |

Both forms work in VS Code, Visual Studio and JetBrains IDEs (preview in JetBrains IDEs).

## Create your first file

1. Create `.github/copilot-instructions.md` in the repository root
2. Paste the example from the next section and change it to match your repository
3. Ask Copilot a question about the repository in chat
4. Expand **References** in the response and confirm the file is listed

In VS Code, the Agent Customizations editor can draft this file for you from the repository. Review the draft before you commit it.

## What belongs in repository-wide instructions

- A compact map of the repository
- The actual build, test and lint commands
- Conventions that apply everywhere, not just in one project
- Things Copilot must not do, such as breaking public API contracts
- Pointers to detailed local documentation

Example:

```markdown
# Repository guidance

- The Angular front end is under `src/client`
- The .NET API is under `src/api`
- Run the focused project tests before completing a change
- Preserve existing public API contracts unless the task explicitly changes them
- Prefer existing shared components and services over creating duplicates
```

Avoid copying every coding standard into this file. Formatters, analysers and tests are better at enforcing deterministic rules.

## Path-specific instructions

Use scoped files for rules that matter only to one language, framework or project area.

```markdown
---
applyTo: "src/api/**/*.cs"
---

- Follow the existing controller, application and domain boundaries
- Use the repository's established test naming convention
- Preserve the current problem-details response format
```

A separate front-end file could apply Angular component, state-management and testing conventions under `src/client`.

When you work on a file that matches a path-specific pattern, Copilot uses both files. Keep the repository-wide file for rules that apply everywhere.

## Where coding conventions belong

Ask three questions:

1. **Can a formatter or analyser enforce it?** Configure that tool instead
2. **Does it apply across the repository?** Keep a short statement in repository-wide instructions
3. **Does it apply to one language, project or folder?** Use path-specific instructions

A one-line example helps, such as `Method_Condition_Result` for C# test names. Long code samples go out of date and are sent every time the instruction applies.

## Link to detail instead of copying it

An instruction can direct Copilot to a repository document when a task needs fuller guidance. For a larger codebase, point to a small index and let the agent select relevant detail; [Keep the entry point small](../Repository-Knowledge.md#keep-the-entry-point-small) shows the pointer to use.

Link to a file in the repository so Copilot can open it. The detail stays out of every model call, and the team can still review it. Any folder works: `.docs`, `docs/` or a file in the root.

A link does not mean Copilot reads the file. In VS Code Local sessions, linked instruction files are added automatically only if the `chat.includeReferencedInstructions` setting allows it. Other documents are read only when the agent opens them, for example because your request tells it to. Check **References** in the response to see what was used.

See [Repository knowledge for people and agents](../Repository-Knowledge.md) for the task-routing pattern, and use its [bounded pilot](../Repository-Knowledge/Bootstrap-and-Evaluate.md) when trialling the approach.

## Context and usage

Copilot adds your repository instructions to the model input on every relevant model call. That saves you repeating yourself, but every line takes space your code and tool results could use.

Keep instructions short enough to review regularly. Put longer, occasional workflows in a [skill](Agent-skills.md).

## Advanced note: `AGENTS.md`

VS Code reads `AGENTS.md`, but Visual Studio and JetBrains IDEs do not list it for Copilot Chat. If your team uses more than VS Code, stick to the `.github` files.

## Review checklist

- Can the file be understood in under two minutes?
- Does every rule apply to most work in its scope?
- Are any rules duplicated or contradictory?
- Are build and test commands still correct?
- Can a formatter, analyser or test enforce a rule instead?
- Could a long workflow become a skill or linked document?
- Does the file appear under **References** in a chat response?

## Sources

- [Adding repository custom instructions in an IDE](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)
- [Support for different types of custom instructions](https://docs.github.com/en/copilot/reference/custom-instructions-support)
- [About customising Copilot responses](https://docs.github.com/en/copilot/concepts/prompting/response-customization)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
