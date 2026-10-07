# Copilot technologies

_A directory for shaping Copilot's behaviour. Last reviewed 4 October 2026._

Instructions, skills, prompt files and custom agents each change a different part of what Copilot does. Start with the smallest one that fixes a problem your team already has: a build command Copilot keeps getting wrong, rules applied to the wrong part of the code, or a review checklist you paste into every chat. Try it, see what changes, and refine it with the team.

New to Copilot? Read [Copilot 101](Copilot-101.md) first.

Every customisation is text that ends up in the [model input](How-Copilot-Works.md#what-reaches-the-model). Some of it is sent with every model call, so keep that part short. The rest is sent only when it is needed.

![The model input stack with a tag on each layer: repository instructions and skill names and descriptions are sent with every model call, path instructions when a matching file is involved, a skill body when selected, a selected custom agent replaces the agent layer, a prompt file becomes your request when run, and a subagent runs a separate stack whose result alone comes back](Media/customisation-entry-points.svg =760x)

## Choose and use a technology

1. [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
2. [Custom instructions](Copilot-Technologies/Custom-instructions.md)
3. [Agent skills](Copilot-Technologies/Agent-skills.md)
4. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
5. [Prompt files](Copilot-Technologies/Prompt-files.md)

Most teams start with repository instructions, add one skill for a procedure they repeat, and only later create a custom agent. Skip prompt files for new work: VS Code is replacing them with skills ([details](Copilot-Technologies/Prompt-files.md#vs-code-deprecation)).

## Try ideas without changing the shared repository

Review and commit shared customisations like any other code. While experimenting, add an untracked local file to `.git/info/exclude` to keep it out of Git status. This only affects your clone. It does not change the team's `.gitignore` or hide files Git already tracks, and Copilot still reads the file. Move a useful experiment into the repository and review it before the team relies on it.

## Current IDE support

Checked against vendor documentation on 4 October 2026. Your extension or plugin version also matters.

| Feature | VS Code | Visual Studio | JetBrains |
| --- | --- | --- | --- |
| Custom instructions | Supported | Supported | Preview |
| Prompt files | Supported, deprecated for Agent Host sessions† | Supported | Preview |
| Custom agents | Supported | Visual Studio 2026 18.4+* | Preview |
| Subagents | Supported | Not documented as supported | Preview |
| Agent skills | Supported | Visual Studio 2026 18.5+* | Preview |

\* Microsoft documents custom agents from Visual Studio 2026 18.4 and agent skills from 18.5. Visual Studio 2022 17.14 does not provide every feature listed for Visual Studio 2026.

† VS Code is replacing prompt files with skills; see [Prompt files](Copilot-Technologies/Prompt-files.md#vs-code-deprecation).

## Sources

- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Agent customisation in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Git ignore patterns and local exclude files](https://git-scm.com/docs/gitignore)
