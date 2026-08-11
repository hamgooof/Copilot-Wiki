# Context, memory and models (advanced)

_For regular Copilot users and platform teams ready for the advanced detail · Last reviewed 11 August 2026_

> This builds on [Tokens and context windows](../Tokens-and-Context-Windows.md) and focuses on advanced behaviour across skills, agents, memory and model changes.

“Context” is the information available to a model for the current request. It is not the same as durable memory, repository access or a model's trained knowledge.

[[_TOC_]]

## What may occupy a context window

- System and product instructions.
- Personal, organisation, repository, path and agent instructions.
- The user's current prompt.
- Conversation history or a compacted summary of it.
- Selected files, symbols, terminal output and source-control changes.
- Retrieved repository content.
- Tool definitions, tool calls and tool results.
- An invoked skill's `SKILL.md`.
- A custom agent's profile instructions.
- Results returned by subagents.

The product assembles this context; it does not simply include the entire repository on every round. Input, generated output and supported thinking tokens share the model's context-window capacity.

## Effective context versus transport

At the user level, each round is evaluated against an effective prompt and conversation state. That does **not** guarantee that every provider integration sends the same complete message array over the network on every call.

Clients and providers can implement transport, provider-side state and prompt caching differently. VS Code's Cache Explorer compares consecutive model requests and their matching prompt prefix; it is a diagnostic comparison, not a network packet capture. Do not use a displayed diff—or the absence of repeated text in a diagnostic view—as proof of what was physically transmitted or newly billed.

Provider-specific behaviour, including cache time-to-live and stateful request APIs, belongs in dated test evidence rather than universal foundation guidance. See T17 in [To Test](../To-Test.md#t17-provider-transport-effective-context-and-cache-views).

## Context is not memory

| Concept | Lifetime | Typical contents |
| --- | --- | --- |
| Current request context | One model request | Prompt, instructions, selected/retrieved material, recent history |
| Conversation history | Current chat/session, subject to compaction | Earlier messages and tool activity |
| Subagent context | One delegated worker/session | Delegated task and child-specific working material |
| Copilot Memory | Across supported sessions until deleted/expired | Repository facts and user preferences |
| Repository files | Persistent source of truth | Code, docs and instruction files retrieved when needed |

## What happens as a chat grows

Model context windows are finite. Current Copilot CLI documentation says it automatically compresses history near the context limit and offers `/context` and `/compact`. VS Code's February 2026 release notes document a context-window control together with automatic and manual compaction.

Compaction summarises older history. It lets the session continue, but a summary cannot be assumed to preserve every detail. For long-running work, record key decisions, constraints and current state in a durable repository document.

## Do skills maintain context?

A skill does not have its own independent memory simply because it is a skill.

**Documented:** when used, its `SKILL.md` is injected into the invoking agent's context. The agent can use the resources bundled with the skill.

**Not fully documented across surfaces:** whether that injected text is resent unchanged on later rounds, represented in a compacted summary, or discarded when the task changes. Do not use a skill as the only store for state produced during a task.

## Do custom agents maintain context?

There are two cases:

1. **Selected as the active agent in a chat:** its profile is applied to that chat execution. Whether switching agents preserves every client-side attachment and agent-specific state is surface- and version-dependent.
2. **Invoked as a subagent:** it has a separate context window. It returns a result to the parent, but its full internal transcript is not the parent's context.

Persist important outputs in files or explicitly pass them during handoffs.

## What happens when the model changes?

For Copilot Chat on GitHub.com, GitHub documents that regenerating a response with a different model maintains the full conversation context. IDE surfaces do not currently document the same guarantee; [T11](../To-Test.md#t11-model-change-within-a-chat) covers that test. A new model can also have different capabilities, context-window size, tool support, speed and cost.

Practical consequences:

- The conversation can continue, but the new model may interpret the same history differently.
- The selected model changes the available context-window capacity in VS Code.
- A smaller window may trigger compaction sooner.
- Prompt caching can be affected when model or context changes.
- Chat model selection and inline-completion model configuration are separate controls; do not assume changing one changes the other.
- Extensions or agents can override model selection on some surfaces.

## Parent and child models

A parent can orchestrate while a different model handles a subtask on supported surfaces. This is not a free context transfer:

- The child has its own window.
- The parent must provide or make accessible the relevant constraints.
- Child usage is separate model work and contributes to overall usage.
- The returned result becomes new context for the parent.
- Availability, plan, cost tier and automatic model selection can override the intended routing.

## Copilot Memory

Copilot Memory is a separate public-preview feature. Current GitHub documentation says it stores:

- Repository-level facts, cited to supporting code and validated against the current branch before use.
- User-level preferences used across repositories, subject to billing-entity rules.

It is currently used by Copilot cloud agent, Copilot code review and Copilot CLI. Stored items that go unused are automatically deleted after 28 days, with the timer potentially reset after successful validation and use.

Memory is useful for learned conventions, but it should not replace reviewed instruction files for mandatory standards. Memory can be inferred, expire and change; policy should remain explicit and version controlled.

## Reducing context bloat

Keep always-on guidance short, retrieve large material only when it is needed, and give subagents bounded questions with concise return requirements. The practical habits are collected in [Tokens and context windows](../Tokens-and-Context-Windows.md#practical-context-habits) and [Working efficiently and managing cost](../Working-Efficiently-and-Managing-Cost.md).

Questions that still need measured, versioned evidence—including skill persistence, parent/child transfer and model routing—live in [To Test](../To-Test.md).

## Sources

- [VS Code February 2026 release notes: context compaction](https://code.visualstudio.com/updates/v1_110)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Using GitHub Copilot CLI: context management](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview)
- [Changing the AI model for Copilot Chat](https://docs.github.com/en/copilot/how-tos/use-ai-models/change-the-chat-model)
- [Changing the model used for code completion](https://docs.github.com/en/copilot/how-tos/use-ai-models/change-the-completion-model)
- [About GitHub Copilot Memory](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/copilot-memory)
- [Creating GitHub Copilot Spaces: repository retrieval versus full-file context](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/copilot-spaces/create-copilot-spaces)
