# Copilot technologies

_A directory for shaping Copilot's behaviour. Last reviewed 4 October 2026._

Instructions, skills, prompt files and custom agents change different parts of the Copilot experience. Start with the smallest shared setup that solves a real team problem.

For first-day guidance, start with [Copilot 101](Copilot-101.md). The cross-IDE support summary is below; the linked pages explain how each technology works.

Every customisation is text that the harness places in a known slot of the [model input](How-Copilot-Works.md#what-reaches-the-model). Some slots are filled on every model call, so they cost something every time; others are filled only when needed.

![The model input stack with a tag on each layer: repository instructions and skill names and descriptions are sent with every model call, path instructions when a matching file is involved, a skill body when selected, a selected custom agent replaces the agent layer, a prompt file becomes your request when run, and a subagent runs a separate stack whose result alone comes back](Media/customisation-entry-points.svg =760x)

## Choose and use a technology

1. [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
2. [Custom instructions](Copilot-Technologies/Custom-instructions.md)
3. [Agent skills](Copilot-Technologies/Agent-skills.md)
4. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
5. [Prompt files](Copilot-Technologies/Prompt-files.md)

The chooser helps with the decision. The other pages show where each option earns its place in a repository.

Most teams start with repository instructions, add one skill for a procedure they repeat, and only later create a custom agent. Prompt files are optional: VS Code has deprecated them for Agent Host sessions and recommends converting them to skills, so prefer a skill for anything new.

## Use these building blocks in wider practices

Two team practices build on these technologies:

- [Repository knowledge for people and agents](Repository-Knowledge.md) is reusable information, with instructions providing a small entry point
- [Spec-Driven Development](Spec-Driven-Development.md) is an intent-first development process that can use Plan, custom agents, handoffs, saved files and subagents

Keep the concepts separate. A team can use repository knowledge without SDD, and it can practise SDD without creating a custom agent for every phase.

## Try ideas without changing the shared repository

Team-owned customisations should normally be reviewed and committed like other repository changes. While experimenting, add an untracked local file to `.git/info/exclude` to keep it out of Git status. This affects only your clone: it does not update the team's `.gitignore`, hide an already tracked file or prevent Copilot reading that file in your workspace. Move a useful experiment into the repository and review it before the team relies on it.

## Current IDE support

The following reflects the official documentation reviewed on 27 August 2026. Support depends on the installed IDE and Copilot extension or plugin version.

| Feature | VS Code | Visual Studio | JetBrains |
| --- | --- | --- | --- |
| Custom instructions | Supported | Supported | Preview in GitHub's lifecycle table |
| Prompt files | Supported, deprecated for Agent Host sessions† | Supported | Preview |
| Custom agents | Supported | Visual Studio 2026 18.4+* | Preview |
| Subagents | Supported | Not documented as supported | Preview |
| Agent skills | Supported | Visual Studio 2026 18.5+* | Preview |

\* Microsoft documents custom agents from Visual Studio 2026 18.4 and agent skills from 18.5. Visual Studio 2022 17.14 does not provide every feature listed for Visual Studio 2026.

† VS Code's prompt-file documentation (checked 4 October 2026) says prompt files are not loaded by Agent Host and continue to work with the Local agent for now, which will be removed in a future release. See [Prompt files](Copilot-Technologies/Prompt-files.md#vs-code-deprecation).

## Keep the first setup useful

A useful first customisation fixes a problem people already recognise: a repeated wrong build command, rules applying to the wrong code area, a review checklist repeated in every chat, or a recurring task with the same inputs and output. Add the smallest option that addresses it, observe the result and refine it with the team.

## Sources

- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Prompt files in VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Agent customisation in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
- [Agent skills in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-skills?view=visualstudio)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
- [Git ignore patterns and local exclude files](https://git-scm.com/docs/gitignore)
