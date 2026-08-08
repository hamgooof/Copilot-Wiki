# Other useful Copilot technologies

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Draft technology overview | Developers extending or governing Copilot | 8 August 2026 |

> If tools and agent execution are new, begin with [One prompt, many rounds](../One-Prompt-Many-Rounds.md).

Instructions, skills and agents are only part of the design. Tools, MCP, hooks, prompt files, plugins, Spaces and Memory solve different problems.

## On this page

- [Prompt files](#prompt-files)
- [Tools and MCP servers](#tools-and-mcp-servers)
- [Hooks](#hooks)
- [Plugins](#plugins)
- [Copilot Spaces](#copilot-spaces)
- [Copilot Memory](#copilot-memory)
- [Repository indexing and explicit context](#repository-indexing-and-explicit-context)
- [Content exclusion](#content-exclusion)

## Prompt files

Prompt files are reusable prompts that a person invokes for a specific interaction. They are useful for tasks such as generating unit tests, explaining code or applying a review checklist with new inputs each time.

- Stored as `.prompt.md` files, commonly under `.github/prompts`.
- Manually selected or invoked rather than automatically applied to every task.
- Can reference workspace files for additional context.
- Currently documented as public preview and available in VS Code, Visual Studio and JetBrains IDEs, not every Copilot surface.

Use a prompt file for **“run this prompt now.”** Use a skill for **“when this kind of work occurs, follow this workflow.”**

## Tools and MCP servers

A tool is an operation the agent can call, such as reading a file, running a command or querying another system. Model Context Protocol (MCP) servers add collections of tools and data sources.

Use MCP when Copilot needs to:

- Read or update issues, tickets and pull requests.
- Query a database or internal API.
- Drive a browser or test system.
- Access domain-specific live data.

Context considerations:

- Tool definitions must be made available to the model.
- Tool results enter the working context unless summarised or isolated.
- Large tool catalogues can make selection harder and increase prompt overhead.
- Large results should be filtered, summarised or handled by a subagent.

Security considerations:

- Grant least privilege.
- Separate read and write capabilities when practical.
- Treat third-party MCP servers and their outputs as part of the trust boundary.
- Require approval for high-impact or irreversible actions.
- Keep credentials out of instructions and source files.

## Hooks

Hooks run configured shell commands at lifecycle events such as before or after a tool call, session start/end, errors or subagent completion. They are appropriate for deterministic checks and observability.

Examples:

- Block edits to protected paths without a ticket identifier.
- Run secret scanning before a tool action is accepted.
- Record trace or audit information.
- Validate a subagent result before it returns to the parent.
- Run a formatter after an edit.

The difference is important: an instruction asks the model to behave a certain way; a hook runs code at the configured event. Hooks can still fail or be misconfigured, so CI and platform controls remain the final enforcement layer for critical policy.

## Plugins

In Copilot CLI, a plugin is an installable bundle that can contain skills, custom agents, hooks and MCP configurations. Plugins help distribute a cohesive capability and update it without manually copying each part.

Use a plugin when a team needs a versioned bundle such as:

- Incident-response skills.
- A read-only production-diagnostics agent.
- Internal MCP tools.
- Audit and guardrail hooks.

Do not create a plugin just to hold one short instruction file. Packaging creates an update and trust lifecycle that must be owned.

## Copilot Spaces

Spaces organise shared context for Copilot Chat. They can contain repositories, code, pull requests, issues, notes, images and uploads, plus Space-specific instructions.

Context behaviour matters:

- Adding a repository does not load the whole repository into memory; Copilot retrieves relevant content for the question.
- Attaching an individual file loads its full contents into context for every query in that Space.

Spaces are useful for shared project Q&A, onboarding and curated knowledge. They are not a replacement for agent instructions or executable workflows.

## Copilot Memory

Memory lets supported Copilot agents reuse validated repository facts and user preferences across sessions. It is useful when Copilot learns conventions through work, but its public-preview status, retention and governance rules mean it should complement—not replace—version-controlled documentation and instructions.

## Repository indexing and explicit context

Copilot can search indexed repositories to find relevant code. Users can also attach files, folders, symbols, terminal output and other context explicitly. Prefer retrieval over pasting large codebases into instructions.

## Content exclusion

Content exclusion can prevent Copilot features from using selected files, but support has important exceptions. Current GitHub documentation notes that content exclusion is not supported in Edit and Agent modes in VS Code and other editors. Treat exclusion as one layer of control, not a universal security boundary.

## Sources

- [Prompt files](https://docs.github.com/en/copilot/tutorials/customization-library/prompt-files)
- [Use prompt files in VS Code: `.prompt.md` format and locations](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Comparing Copilot CLI customization features: tools, MCP, hooks and plugins](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/comparing-cli-features)
- [About GitHub Copilot Spaces](https://docs.github.com/en/copilot/concepts/context/spaces)
- [About GitHub Copilot Memory](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/copilot-memory)
- [Content exclusion for GitHub Copilot](https://docs.github.com/en/copilot/concepts/context/content-exclusion)
- [Concepts for providing context to GitHub Copilot](https://docs.github.com/en/copilot/concepts/context)
