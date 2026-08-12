# Copilot technologies

_A directory for shaping Copilot's behaviour. Last reviewed 12 August 2026._

Instructions, skills, prompt files and custom agents change different parts of the Copilot experience. Start with the smallest shared setup that solves a real team problem.

For first-day guidance, start with [Copilot 101](Copilot-101.md). The cross-IDE support summary is below; the linked pages explain how each technology works.

## Choose and use a technology

1. [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
2. [Custom instructions](Copilot-Technologies/Custom-instructions.md)
3. [Agent skills](Copilot-Technologies/Agent-skills.md)
4. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
5. [Prompt files](Copilot-Technologies/Prompt-files.md)

The chooser helps with the decision. The other pages show where each option earns its place in a repository.

## Try ideas without changing the shared repository

Team-owned customisations should normally be reviewed and committed like other repository changes. While experimenting, add an untracked local file to `.git/info/exclude` to keep it out of Git status. This affects only your clone: it does not update the team's `.gitignore`, hide an already tracked file or prevent Copilot reading that file in your workspace. Move a useful experiment into the repository and review it before the team relies on it.

## Current IDE support

The following reflects the official documentation reviewed on 12 August 2026. Support depends on the installed IDE and Copilot extension or plugin version.

| Feature | VS Code | Visual Studio | JetBrains |
| --- | --- | --- | --- |
| Custom instructions | Supported | Supported | Preview in GitHub's lifecycle table |
| Prompt files | Public preview | Public preview | Public preview |
| Custom agents | Supported | Visual Studio 2026 18.4+* | Preview |
| Subagents | Supported | Not supported | Preview |
| Agent skills | Supported | Visual Studio 2026 18.5+* | Preview |

\* Microsoft documents custom agents from Visual Studio 2026 18.4 and agent skills from 18.5. Visual Studio 2022 17.14 does not provide every feature listed for Visual Studio 2026.

## Keep the first setup useful

A useful first customisation fixes a problem people already recognise: a repeated wrong build command, rules applying to the wrong code area, a review checklist repeated in every chat, or a recurring task with the same inputs and output. Add the smallest option that addresses it, observe the result and refine it with the team.

## Sources

- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Agent customisation in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Git ignore patterns and local exclude files](https://git-scm.com/docs/gitignore)
