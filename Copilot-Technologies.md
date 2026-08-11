# Copilot technologies

_For people learning how to shape Copilot's behaviour - Last reviewed 12 August 2026_

Copilot can follow shared guidance, load repeatable workflows and use purpose-built agents. These options overlap, but they change different parts of the experience.

Start small. A short, relevant setup is easier for teams to maintain and easier for Copilot to apply.

[[_TOC_]]

## Before starting

If the underlying ideas are unfamiliar, read:

- [How Copilot works in your IDE](How-Copilot-Works.md)
- [One prompt, many rounds](One-Prompt-Many-Rounds.md)
- [Copilot terminology without the headache](Copilot-Terminology.md)

## Technology breakdown

1. [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
2. [Custom instructions](Copilot-Technologies/Custom-instructions.md)
3. [Agent skills](Copilot-Technologies/Agent-skills.md)
4. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
5. [Prompt files and explicit context](Copilot-Technologies/Prompt-files-and-other-IDE-features.md)

The [chooser](Copilot-Technologies/Choose-the-right-technology.md) compares the options. The remaining pages show what they look like in a repository and where each one earns its place.

## Try ideas without changing the shared repository

Team-owned customisations should normally be reviewed and committed like other repository changes. While experimenting, you can keep an untracked local file out of Git status by adding its path to:

```text
.git/info/exclude
```

This exclusion belongs to your local clone. It does not update the team's `.gitignore`, hide an already tracked file or prevent Copilot from reading the file in your workspace. Move a useful experiment into the repository and review it before the team relies on it.

## Keep the first setup useful

A good first customisation solves a problem people already recognise:

- The wrong build command is used repeatedly
- Front-end rules are being applied to back-end code
- The team repeats the same review checklist in every chat
- A recurring task needs the same inputs and output format

Add the smallest option that addresses the problem, observe how it behaves, and refine it with the team.

## Sources

- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Git ignore patterns and local exclude files](https://git-scm.com/docs/gitignore)
