# Choose the right Copilot technology

_For regular Copilot users and repository maintainers · Last reviewed 11 August 2026_

> New to terms such as context window, tool or subagent? Read [Copilot terminology](../Copilot-Terminology.md) first.

The choice is mainly about **activation**, **scope**, **capability** and **context isolation**.

[[_TOC_]]

## Decision guide

Ask these questions in order:

1. **Must this happen deterministically?** Use a hook where your Copilot surface supports it, or use an existing CI/policy control. Instructions are guidance to a model, not guaranteed program execution.
2. **Does Copilot need a new external capability or live data?** Add a tool, usually through a Model Context Protocol (MCP) server—a standard plug for another system's tools or data.
3. **Should the guidance apply to almost every request?** Use a short custom instruction.
4. **Should it apply only while working on matching files?** Use path-specific instructions.
5. **Is it a detailed workflow that is relevant only sometimes?** Use a skill.
6. **Should a person explicitly start a reusable task?** Use a prompt file where supported.
7. **Does the task need a specialist role, a restricted toolset or a different model?** Use a custom agent.
8. **Would the work benefit from an isolated context or parallel execution?** Delegate it to a subagent.
9. **Is this a set of customizations that must be distributed and updated together?** Package it as a plugin where supported.

## Comparison

| Technology | Activation | Best for |
| --- | --- | --- |
| Repository/custom instructions | Automatic | Compact standards, build commands and repository facts that matter often |
| Path-specific instructions | Automatic for matching paths | Language, component or directory-specific rules |
| `AGENTS.md` | Automatic for supporting agents | Cross-tool repository conventions; discovery varies by surface |
| Skill | Automatically selected when relevant, or explicitly invoked | Detailed repeatable workflows, scripts and supporting resources |
| Prompt file | Manually invoked | Reusable one-shot prompts with variable inputs |
| Custom agent | Selected manually or delegated to | Reviewers, auditors, test writers and roles needing different tools or models |
| Subagent | Delegated at runtime | Isolated exploration, testing, review and independent subtasks |
| MCP server | Tools selected as needed | GitHub, tickets, databases, browsers and internal APIs |
| Hook | Automatic at a configured event, where supported | Deterministic checks, logging, validation and mandatory commands |
| Copilot Memory | Automatic when supported and relevant | Repository facts or preferences that may help across conversations |
| Copilot Space | Used when chatting through a Space | Shared, curated question-and-answer context |

Avoid using broad instructions for rare runbooks, skills for universal rules, or custom agents and subagents for tiny tasks. MCP is for external tools or live data rather than static guidance. Hooks are for deterministic commands rather than subjective judgement, while Memory and Spaces should not replace formal policy or coding-agent workflow definitions.

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
