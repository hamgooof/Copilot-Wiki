# GitHub Copilot: work smarter, not noisier

_For people who already use GitHub Copilot and want to get more from it · Last reviewed 11 August 2026_

You already know how to ask Copilot a question or generate some code. This wiki is about the next step: giving it better context, choosing the right kind of customization and avoiding setups that quietly waste tokens and attention.

You do not need to read it front to back. Pick the route that matches what you are trying to solve.

> **Our focus:** day to day we use Copilot in VS Code, Visual Studio and JetBrains, so that is what this wiki prioritises. CLI and cloud-agent notes appear where they help GH-600 study and are clearly labelled.

## I want the mental model first

1. [How Copilot actually works](How-Copilot-Works.md)
2. [Tokens and context windows](Tokens-and-Context-Windows.md)
3. [One prompt, many rounds](One-Prompt-Many-Rounds.md)
4. [Copilot terminology without the headache](Copilot-Terminology.md) — use this as a reference when a term is unfamiliar

## I want to improve how I work

- [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
- [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
- [Context, memory and models (advanced)](Copilot-Technologies/Context-memory-and-models.md)

## I need to choose a customization

Open [Copilot technologies](Copilot-Technologies.md) for the full section, or jump directly to:

- [Custom instructions and `AGENTS.md`](Copilot-Technologies/Custom-instructions.md)
- [Agent skills](Copilot-Technologies/Agent-skills.md)
- [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
- [Prompt files, Model Context Protocol (MCP), hooks, plugins, Spaces and Memory](Copilot-Technologies/Other-useful-technologies.md)

## I am studying GH-600

[The GH-600 study map](GH-600-Certification-Map.md) connects these pages to the official exam domains, implementation artefacts and practical exercises. It also identifies the areas this wiki does not cover yet.

## I want evidence, not folklore

- [To Test](To-Test.md) contains reproducible experiments for product behaviour that is uncertain, version-specific or poorly documented.
- [Sources and maintenance](Sources-and-Maintenance.md) records the primary-source policy and review process.

Statements marked **To Test** are hypotheses, not product guarantees. Product behaviour changes; each substantive page links to the GitHub, Microsoft or VS Code material used to support it.

## Sources

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Publish a Git repository to an Azure DevOps wiki](https://learn.microsoft.com/en-us/azure/devops/project/wiki/publish-repo-to-wiki?view=azure-devops)
