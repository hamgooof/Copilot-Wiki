# Choose the right Copilot technology

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Draft decision guide | Regular Copilot users and repository maintainers | 8 August 2026 |

> New to terms such as context window, tool or subagent? Read [Copilot terminology](../Copilot-Terminology.md) first.

The choice is mainly about **activation**, **scope**, **capability** and **context isolation**.

## Decision guide

Ask these questions in order:

1. **Must this happen deterministically?** Use a hook or an existing CI/policy control. Instructions are guidance to a model, not guaranteed program execution.
2. **Does Copilot need a new external capability or live data?** Add a tool, usually through an MCP server.
3. **Should the guidance apply to almost every request?** Use a short custom instruction.
4. **Should it apply only while working on matching files?** Use path-specific instructions.
5. **Is it a detailed workflow that is relevant only sometimes?** Use a skill.
6. **Should a person explicitly start a reusable task?** Use a prompt file where supported.
7. **Does the task need a specialist role, a restricted toolset or a different model?** Use a custom agent.
8. **Would the work benefit from an isolated context or parallel execution?** Delegate it to a subagent.
9. **Is this a set of customizations that must be distributed and updated together?** Package it as a plugin where supported.

## Comparison

| Technology | Activation | Context behaviour | Best for | Avoid using it for |
| --- | --- | --- | --- | --- |
| Repository/custom instructions | Automatic | Added broadly to model requests or loaded at session start, depending on surface | Compact standards, build commands, non-negotiable repository facts | Long runbooks and rare workflows |
| Path-specific instructions | Automatic on matching paths | Added when relevant file paths match | Language, component or directory rules | Cross-repository workflows |
| `AGENTS.md` | Automatic for supporting agents | Standing agent guidance; discovery and precedence vary by surface | Cross-tool repository conventions | A catalogue of optional workflows |
| Skill | Automatic when relevant, or explicitly invoked on supporting surfaces | `SKILL.md` is injected when chosen | Detailed repeatable workflows, scripts and supporting resources | Universal rules |
| Prompt file | Manual | Added to the specific interaction | Reusable one-shot prompts with variable inputs | Automatic policy or non-IDE portability |
| Custom agent | Manual selection and/or automatic delegation | Applies a specialist prompt, tools and optional model; subagent execution is isolated | Reviewers, auditors, test writers, constrained roles | Guidance text that a skill could handle |
| Subagent | Delegated at runtime | Separate context window from parent and sibling agents | Exploration, testing, review, parallel subtasks | Small tasks where coordination costs exceed the benefit |
| MCP server | Tools selected as needed | Tool definitions and returned data consume some context when used | GitHub, tickets, databases, browsers and internal APIs | Static guidance or built-in capabilities |
| Hook | Automatic at configured lifecycle event | Does not depend on the model remembering an instruction | Guardrails, logging, validation and mandatory commands | Subjective judgement or rich reasoning |
| Copilot Memory | Automatic when supported and relevant | Retrieves stored, validated facts/preferences into supported sessions | Knowledge that should outlive one conversation | Formal policy or facts that must never drift |
| Copilot Space | Used when chatting in a Space or through supported integration | Searches attached repositories; fully loads specifically attached files for every Space query | Shared, curated Q&A context | Coding-agent workflow definitions |

## Skill versus custom agent

This is the most common source of confusion.

Use a **skill** when the default agent has the right tools and general role, but needs a reusable procedure, template, examples or scripts for a type of task.

Use a **custom agent** when the worker itself should be different: a specialist prompt, a constrained toolset, extra MCP servers, a selected model, or isolated execution as a subagent.

Example:

- “Follow our release-note format, inspect these files and run this validation script” is a skill.
- “Act as a read-only security auditor, use only search/read tools and report findings in this structure” is a custom agent.
- A security-auditor agent may itself use a vulnerability-triage skill.

## Instructions versus skills

GitHub's guidance is to use custom instructions for simple information relevant to almost every task, and skills for detailed instructions that should be accessed only when relevant.

Poor instruction file:

```text
2,000 lines covering releases, incident response, UI testing,
database migration, documentation and every team process.
```

Better design:

```text
.github/copilot-instructions.md       # short, universal repository facts
.github/instructions/*.instructions.md # path-specific rules
.github/skills/release/SKILL.md       # loaded for release work
.github/skills/incident/SKILL.md      # loaded for incident work
.github/agents/security-review.agent.md # specialist execution profile
```

## A useful rule of thumb

> If deleting a paragraph would harm most Copilot tasks, it belongs in always-on instructions. If it would harm only one class of task, move it to a skill, prompt file or agent.

## Surface support changes the answer

As of the review date, support is not uniform. GitHub's current matrix shows, for example, that prompt files are an IDE feature rather than a GitHub.com or Copilot CLI feature, while subagents are supported in VS Code and Copilot CLI but not every surface. Treat the [current customization support matrix](https://docs.github.com/en/copilot/reference/customization-cheat-sheet) as the source of truth.

## Sources

- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Comparing Copilot CLI customization features](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/comparing-cli-features)
- [Adding agent skills for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [About GitHub Copilot code review: choosing customization types](https://docs.github.com/en/copilot/concepts/agents/code-review)
- [Custom agents in VS Code: format and file locations](https://code.visualstudio.com/docs/agent-customization/custom-agents)
