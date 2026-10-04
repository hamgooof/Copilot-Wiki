# Custom instructions

_For developers and repository maintainers - Last reviewed 4 October 2026_

Custom instructions are standing guidance that Copilot applies automatically within a defined scope. They are a good home for concise repository facts, commands and conventions that matter frequently. Think of them as house rules taped to the dashboard: every driver sees them on every trip, so keep them short.

[[_TOC_]]

## The two repository forms

| Form | Location | Scope |
| --- | --- | --- |
| Repository-wide instructions | `.github/copilot-instructions.md` | Work across the repository |
| Path-specific instructions | `.github/instructions/NAME.instructions.md` | Files matching the YAML `applyTo` pattern |

GitHub currently lists both forms for Copilot Chat in VS Code, Visual Studio and JetBrains. JetBrains custom instructions are marked preview in GitHub's lifecycle table.

## Create your first file

1. Create `.github/copilot-instructions.md` in the repository root
2. Paste the example from the next section and change it to match your repository
3. Ask Copilot a question about the repository in chat
4. Expand **References** in the response and confirm the file is listed

VS Code can also generate a first draft from the repository: the Agent Customizations editor can start a chat that analyses the repository and creates instructions. Review the draft like any other change before committing it.

## What belongs in repository-wide instructions

- A compact map of the repository
- The actual build, test and lint commands
- Choices that genuinely apply across the repository
- A few important boundaries
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

When repository-wide and matching path-specific instructions both apply, both can be used. Keep the universal file genuinely universal and local detail in the scoped file.

## Where coding conventions belong

Ask three questions:

1. **Can a formatter or analyser enforce it?** Configure that tool instead
2. **Does it apply across the repository?** Keep a short statement in repository-wide instructions
3. **Does it apply to one language, project or folder?** Use path-specific instructions

Small examples can clarify a team decision, such as whether C# tests use `Method_Condition_Result` names. Long code samples are costly to maintain and occupy context whenever the instruction applies.

## Link to detail instead of copying it

An instruction can direct Copilot to a repository document when a task needs fuller guidance. For a larger codebase, point to a small index and let the agent select relevant detail; [Keep the entry point small](../Repository-Knowledge.md#keep-the-entry-point-small) shows the pointer to use.

The target must be a file the local harness can access. This keeps detailed examples out of every model call while leaving the source reviewable by the team. `.docs` is one reasonable location, not a requirement; `docs/` or concise root documents can use the same pattern.

A link is not a promise that every target is injected automatically. In VS Code, automatic inclusion of referenced instruction files depends on `chat.includeReferencedInstructions`, while an ordinary repository document can be read with tools when your request directs the agent to it. Check the response references or customisation diagnostics when loading behaviour matters.

See [Repository knowledge for people and agents](../Repository-Knowledge.md) for the task-routing pattern, and use its [bounded pilot](../Repository-Knowledge/Bootstrap-and-Evaluate.md) when trialling the approach.

## Context and usage

Repository instructions are automatically added to the model input for relevant model calls. That saves repetition, but every always-applied line also competes with the current task, code and tool results for context.

Keep instructions short enough to review regularly. Detailed occasional workflows belong in [skills](Agent-skills.md), and manually invoked requests can belong in [prompt files](Prompt-files.md).

## Advanced note: `AGENTS.md`

`AGENTS.md` is a portable instruction format that VS Code recognises, but Visual Studio and JetBrains do not list it for normal Copilot Chat. For a team using all three IDEs, use Copilot's `.github` instruction files.

## Review checklist

- Can the file be understood in under two minutes?
- Does every rule apply to most work in its scope?
- Are any rules duplicated or contradictory?
- Are build and test commands still correct?
- Can a formatter, analyser or test enforce a rule instead?
- Could a long workflow become a skill or linked document?
- Has someone verified the instructions appear in the client's references or diagnostics?

## Sources

- [Adding repository custom instructions in an IDE](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)
- [Support for different types of custom instructions](https://docs.github.com/en/copilot/reference/custom-instructions-support)
- [About customising Copilot responses](https://docs.github.com/en/copilot/concepts/prompting/response-customization)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
