# Advanced: context across skills, agents and models

_For regular Copilot users ready for the advanced detail · Last reviewed 11 August 2026_

This page builds on [Tokens and context windows](../Tokens-and-Context-Windows.md) and focuses on skills, agents, context transfer and model changes. **Context** is the task information available to a model for the current request. Repository access, saved files and the model's trained knowledge are separate sources. The basic context breakdown lives on [How Copilot works in your IDE](../How-Copilot-Works.md#what-reaches-the-model).

[[_TOC_]]

## Effective context versus transport

Each round is evaluated against an effective prompt and conversation state. The provider may receive or reference that state differently depending on its API; the model still processes the effective context.

VS Code's Cache Explorer compares consecutive requests and their matching prompt prefix. Treat its delta as a cache diagnostic rather than a record of network traffic. Provider-specific transport and cache time-to-live belong in dated test evidence; see [T17](../To-Test.md#t17-provider-transport-effective-context-and-cache-views).

## Where task information can live

| Concept | Lifetime | Typical contents |
| --- | --- | --- |
| Current request context | One model request | Prompt, instructions, selected/retrieved material, recent history |
| Conversation history | Current chat/session, subject to compaction | Earlier messages and tool activity |
| Subagent context | One delegated worker/session | Delegated task and child-specific working material |
| Repository files | Persistent source of truth | Code, docs and instruction files retrieved when needed |

## Do skills maintain context?

A skill's instructions join the invoking agent's context. The skill itself has no separate memory store.

**Documented:** when used, its `SKILL.md` is injected into the invoking agent's context. The agent can use the resources bundled with the skill.

Persistence across later rounds, later turns and compaction is not fully documented across IDEs. Write task state and important outputs to files rather than leaving them only inside a skill-driven conversation. [T04](../To-Test.md#t04-skill-activation-and-persistence) covers the missing detail.

## Do custom agents maintain context?

There are two cases:

1. **Selected as the active agent in a chat:** its profile is applied to that chat execution. Whether switching agents preserves every client-side attachment and agent-specific state is surface- and version-dependent
2. **Invoked as a subagent:** it has a separate context window. It returns a selected result to the parent while its working transcript remains in the child context

Persist important outputs in files or explicitly pass them during handoffs.

## What happens when the model changes?

Changing models in an IDE can preserve the visible conversation while changing the model's capability, context-window size, tool use, speed and cost. The exact context retained and the effect of moving to a smaller window are covered by [T11](../To-Test.md#t11-model-change-within-a-chat).

Practical consequences:

- The conversation can continue, but the new model may interpret the same history differently
- The selected model changes the available context-window capacity in VS Code
- A smaller window may trigger compaction sooner
- Prompt caching can be affected when model or context changes
- Chat model selection and inline-completion model configuration are separate controls
- Extensions or agents can override model selection on some surfaces

## Parent and child models

A parent can orchestrate while a different model handles a subtask on supported surfaces. Delegation creates a new context boundary:

- The child has its own window
- The parent must provide or make accessible the relevant constraints
- Child usage is separate model work and contributes to overall usage
- The returned result becomes new context for the parent
- Availability, plan, cost tier and automatic model selection can override the intended routing

Questions that still need measured, versioned evidence, including skill persistence, parent/child transfer and model routing, live in [To Test](../To-Test.md). Everyday context habits are collected in [Working efficiently and managing cost](../Working-Efficiently-and-Managing-Cost.md).

## Sources

- [VS Code February 2026 release notes: context compaction](https://code.visualstudio.com/updates/v1_110)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Changing the model used for code completion](https://docs.github.com/en/copilot/how-tos/use-ai-models/change-the-completion-model)
