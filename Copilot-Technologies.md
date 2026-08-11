# Copilot technologies

_For people familiar with everyday Copilot use but new to customization and agents · Last reviewed 11 August 2026_

Copilot can follow persistent guidance, load specialised workflows, use purpose-built agents and connect to external systems. These features overlap, but they are not interchangeable.

The aim is not to enable everything. Too much irrelevant context or capability can make Copilot slower, more expensive and less focused.

> Give Copilot the smallest amount of relevant context and capability needed to complete the task well.

## Before starting

If these concepts are unfamiliar, read:

- [Copilot terminology without the headache](Copilot-Terminology.md)
- [Tokens and context windows](Tokens-and-Context-Windows.md)
- [One prompt, many rounds: turns, tools and the agent loop](One-Prompt-Many-Rounds.md)

## Technology breakdown

1. [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)
2. [Custom instructions and AGENTS.md](Copilot-Technologies/Custom-instructions.md)
3. [Agent skills](Copilot-Technologies/Agent-skills.md)
4. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
5. [Context, memory and models (advanced)](Copilot-Technologies/Context-memory-and-models.md)
6. [Other useful technologies](Copilot-Technologies/Other-useful-technologies.md)

## Quick orientation

| Need | Start with |
| --- | --- |
| A short rule that applies broadly | Custom instructions |
| A detailed workflow used occasionally | Skill |
| A specialised role, model or restricted toolset | Custom agent |
| A focused task that should use a separate context window | Subagent |
| Access to an external service | MCP tool/server |
| A command that must run at a lifecycle event | Hook, where the Copilot surface supports it |
| A reusable prompt a person invokes | Prompt file |

## Sources

- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Comparing Copilot CLI customization features](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/comparing-cli-features)
- [Agent customization in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
