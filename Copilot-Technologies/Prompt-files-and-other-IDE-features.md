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

The [technology chooser](Choose-the-right-technology.md#prompt-file-or-skill) covers the boundary between prompt files and skills.

### Run a prompt file

- **VS Code:** type `/` followed by the prompt filename, use **Chat: Run Prompt** from the Command Palette, or open the `.prompt.md` file and select the play button
- **Visual Studio:** type `#prompt:` in chat or select **+** to choose the prompt file
- **JetBrains:** prompt files are currently a preview feature. Open the **Agent Customizations** editor from the settings icon in Copilot Chat and use the prompt controls provided by your plugin version

Repository prompt files normally live at `.github/prompts/NAME.prompt.md`. Check the [current IDE support table](Choose-the-right-technology.md#current-ide-support) if an option is missing.

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

- **VS Code:** type `#`, select **Add Context**, or drag files and folders into Chat
- **Visual Studio:** select **+** in the chat box to attach or reference context
- **JetBrains:** select or open the relevant code and use the context controls available in your Copilot Chat version

Large attachments can crowd the context window. Start with the smallest useful scope and let the agent retrieve more if it needs it.

## Content exclusion

Content exclusion is an organisation or repository control that can prevent some Copilot features from using selected files. GitHub documents important exceptions, including limited support in some Edit and Agent experiences. Most users will not configure this themselves.

Treat content exclusion as one part of repository governance. Use ordinary access controls and secret-management practices for information that must remain protected.

## Sources

- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Use prompt files and context in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-chat-context?view=vs-2022)
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Providing context to GitHub Copilot](https://docs.github.com/en/copilot/concepts/context)
- [Context assembly and workspace indexing in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Adding context to VS Code chat](https://code.visualstudio.com/docs/chat/copilot-chat-context)
- [Content exclusion for GitHub Copilot](https://docs.github.com/en/copilot/concepts/context/content-exclusion)
