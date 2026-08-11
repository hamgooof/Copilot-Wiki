# Prompt files and other useful IDE features

_For Copilot users in VS Code, Visual Studio and JetBrains · Last reviewed 11 August 2026_

Instructions, skills and agents cover most reusable Copilot setups. A few built-in IDE features are also worth knowing about.

[[_TOC_]]

## Prompt files

A prompt file is a reusable request that a person starts deliberately. It suits tasks such as generating unit tests, explaining unfamiliar code or applying a review checklist to a selected file.

- Stored as a `.prompt.md` file, commonly under `.github/prompts`
- Selected manually when you want to run it
- Can contain instructions, variables and references to workspace context
- Useful when the task is repeatable but should only run on demand

The [customisation chooser](Choose-the-right-technology.md#prompt-file-or-skill) covers the boundary between prompt files and skills.

Support and invocation differ between IDEs, so check the current documentation for the client used by your team.

## Repository indexing

Workspace indexing helps Copilot find relevant code without loading the whole repository into every model call. The agent can also use search and read tools when it needs more detail.

Indexing and tools solve different parts of the same problem:

| Capability | What it provides |
| --- | --- |
| Workspace index | Fast retrieval of likely relevant snippets |
| Search tool | A deliberate search requested during an agent round |
| Read tool | The contents of a selected file or range |
| Explicit reference | Context you deliberately attach or mention |

Point Copilot towards a known file or symbol when you have one. This reduces discovery work and gives you more control over the starting context.

## Explicit context

IDE chat can accept files, folders, symbols, terminal output, source-control changes and other references. Use explicit context when a particular item must be considered.

Large attachments can crowd the context window. Start with the smallest useful scope and let the agent retrieve more if it needs it.

## Content exclusion

Content exclusion can prevent some Copilot features from using selected files. GitHub documents important exceptions, including limited support in some Edit and Agent experiences.

Treat content exclusion as one part of repository governance. Use ordinary access controls and secret-management practices for information that must remain protected.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Providing context to GitHub Copilot](https://docs.github.com/en/copilot/concepts/context)
- [Context assembly and workspace indexing in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Adding context to VS Code chat](https://code.visualstudio.com/docs/chat/copilot-chat-context)
- [Content exclusion for GitHub Copilot](https://docs.github.com/en/copilot/concepts/context/content-exclusion)
