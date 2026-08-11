# Copilot terminology without the headache

_For existing Copilot users who want plain-English definitions · Last reviewed 11 August 2026_

Copilot terminology becomes confusing because several products reuse the same words differently. You do not need to memorise everything on this page. Start with the fundamentals, then return to the extended terms when you encounter them.

> **If you remember only one idea:** Copilot is not just a model. A harness assembles context, gives the model tools and keeps calling it until the work is finished.

[[_TOC_]]

## Start here: seven terms worth knowing

1. A **token** is the unit Copilot uses to count model input and output. Under the hood it is a number representing a word, word fragment, punctuation mark or special symbol; you rarely need the number itself.
2. A **model** reads the assembled input and predicts a response. Models differ in capability, speed, context size and cost.
3. **Context** is everything the model can see for its current call.
4. A **context window** is one shared capacity: the input prompt, the model's answer and—on some models—its internal reasoning all need room.
5. A **tool** lets Copilot do something outside the model, such as read a file or run a test.
6. A **turn** is the exchange from your message to Copilot's final response.
7. A **round** is one internal pass through the model-and-tools loop. One turn can contain many rounds.

That is enough vocabulary to understand most of the rest of this wiki.

## Fundamental terminology

| Term | What it means here | Analogy |
| --- | --- | --- |
| **Token** | The unit used to count model input and output. Technically, it is a number representing an item in the model's vocabulary; everyday Copilot work is concerned with the count rather than the number itself. | A numbered catalogue: each word fragment or symbol has an entry, and Copilot counts the entries used. |
| **Model** | A particular large language model offered through Copilot. Models differ in capability, speed, context size and cost. | Vehicles are designed for different jobs. A car is useful for travel; a forklift is better for lifting a pallet. Some are faster, stronger or more efficient than others. |
| **Prompt** | The effective input assembled for the model—not just the words you typed. It can include system instructions, history, files and tool results. | A briefing pack prepared before a meeting. |
| **Context** | The task-specific information available to the model during its current call. It is separate from general knowledge learned during training. | The material currently laid out on a workbench, plus the worker's existing training. |
| **Context window** | The combined token capacity available for one model call. The input prompt, generated output and any supported thinking tokens share that capacity. | The whole workbench: incoming material occupies part of it, while the model still needs room to work and produce the answer. |
| **Session** | One independent conversation with its own history and context. | A project room. Starting a new session gives you a clean room rather than dragging in every old box. |

## One turn can contain many rounds

![A turn containing three agent-loop rounds](Media/turns-rounds-agent-loop.svg =900x)

For the user-facing VS Code experience, this wiki uses the terminology from Microsoft's coding-harness explanation:

- A **turn** starts when you submit one message and ends when Copilot returns its final response.
- A **round** is one pass through the loop: assemble the prompt, call the model, process its response, run any requested tools, record the results and decide whether to continue.
- The **agent loop** is the mechanism that keeps performing rounds until there is a final answer or a stop condition is reached.

For example, “find the failing test, fix it and verify the result” is one turn. Searching, reading, editing, testing and finally answering may take several rounds inside it.

**Source scope:** turn, round and run are vocabulary from Microsoft's VS Code engineering blog. The current VS Code reference documentation describes the harness and agent loop but does not formally define these three terms. This wiki adopts the blog's wording as a consistent teaching convention for our IDE-first audience.

### A genuine product terminology clash

The Copilot SDK documentation uses **turn** for what the VS Code harness article calls a **round**: one LLM call and its consequences. That SDK definition is useful when reading `assistant.turn_start` events, but it is not how the newer user-facing harness article distinguishes turns and rounds.

This wiki therefore uses:

| When discussing | Use |
| --- | --- |
| The experience from user message to final response | **Turn** |
| One internal model/tool iteration | **Round** |
| Copilot SDK event names or logs | Use the SDK's own **turn** wording and say that it is SDK-specific |

Always define the unit when reporting measurements. “Five turns” is otherwise impossible to interpret reliably.

## Extended agent terminology

| Term | Plain-English meaning | Analogy |
| --- | --- | --- |
| **Agent harness** | The product layer that assembles context, exposes and executes tools, manages the loop and adapts different models to the editor. | The vehicle around an engine: controls, dashboard, gearbox and safety systems. |
| **Agent** | A model operating through a harness with context and tools so it can perform work rather than only generate text. | A worker equipped with a brief, tools and a working process. |
| **Tool** | A callable ability such as reading a file, editing code, running a command or querying an external service. | A tool on the worker's bench. |
| **Tool call** | A model's structured request for the harness to execute a tool with particular arguments. | A work order saying which tool to use and how. |
| **Tool result** | The output returned after the harness executes a tool. It can become context in the next round. | Evidence brought back to the workbench. |
| **Agent loop** | The repeated “think → act → observe → think again” process used to complete a turn. | Diagnose, try, inspect the result and adjust. |
| **Custom agent** | A reusable specialist definition with its own instructions, tools and optional model choice. | A named specialist role with a particular toolkit and operating brief. |
| **Subagent** | A separate agent invoked to handle delegated work, usually with an isolated context. | A specialist called in to investigate one part of the job. |
| **Orchestrator or parent agent** | The agent coordinating the overall task and delegating work. | A lead coordinating several specialists. |
| **Handoff** | Passing a task or a later stage of work to another agent or environment. | Transferring a job with a handover note. |
| **Autonomy** | How much an agent may do before it must pause for approval. | The spending and decision authority assigned to a role. |

## Customization terminology

| Term | Plain-English meaning | Best mental shortcut |
| --- | --- | --- |
| **Custom instructions** | Guidance automatically applied within a defined scope, such as a repository or file path. | Standing rules. |
| **`AGENTS.md`** | A file format for persistent repository guidance used by supporting AI agents. Discovery rules vary by product. | A local operating manual. |
| **Skill** | A task-specific package containing `SKILL.md` and optionally scripts, templates or reference material. It is loaded when relevant. | A playbook with supporting equipment. |
| **Prompt file** | A reusable prompt template that a person deliberately invokes. | A reusable job-request form. |
| **MCP server** | A Model Context Protocol server that exposes external tools or data to an AI application. | A standard adaptor to another system. |
| **Hook** | On supporting surfaces, a command run at a configured lifecycle event, such as before a tool call or after an agent stops. | An automatic gate or trigger. |
| **Plugin** | An installable bundle that can package agents, skills, hooks and MCP configuration. | A toolbox delivered as one package. |

For help choosing between these, use [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md).

## Context, state and memory terminology

| Term | Meaning used in this wiki |
| --- | --- |
| **Conversation history** | Earlier user messages, assistant responses and relevant tool activity retained in the current session. |
| **Compaction** | Replacing older conversation detail with a shorter summary to create room in the context window. Some detail can be lost. |
| **Checkpoint** | An overloaded term. In Copilot CLI it can refer to a saved compaction summary. In VS Code it is a restore point used to roll back a session's file changes. |
| **Prompt cache** | A provider mechanism that reuses a matching prefix from earlier model input, potentially reducing cost and latency. |
| **Copilot Memory** | A public-preview mechanism for storing and later retrieving repository facts or user preferences. It is separate from one chat's context. |
| **Copilot Space** | A curated collection of instructions and sources used to ground Copilot Chat around a project or topic. |

## Tokens in more depth

The word **token** is often used loosely for two related things:

1. A vocabulary item that can correspond to text such as a word fragment, punctuation or a special control symbol.
2. The integer **token ID** assigned to that vocabulary item.

A tokenizer encodes text into a sequence of token IDs. The model maps those IDs to numerical vectors called embeddings, performs its calculations and predicts output token IDs. A decoder converts the predicted IDs back into readable text or structured output.

Different models can use different tokenizers. The same sentence can therefore produce a different token count when encoded for another model.

You normally do not need to inspect those IDs yourself. You do need to understand which token counts are input, output, cached or reasoning-related, because that affects context limits, latency and cost.

| Token type | What it represents | Simple analogy |
| --- | --- | --- |
| **Input tokens** | Everything counted in the input for the current round: system instructions, tool definitions, customizations, your message, history, files and earlier tool results. | Items placed into the briefing pack. |
| **Output tokens** | Everything counted while the model produces text or structured tool requests. | The material produced in the response. |
| **Cached input tokens** | An unchanged prefix of input that the provider can reuse from a previous model call. Exact pricing and eligibility depend on the model and product. | Reusing the unchanged first chapters of the briefing instead of processing them again at full cost. |
| **Thinking or reasoning tokens** | Tokens used by supported reasoning models while working through a problem. They may not appear in the final response but can affect usage, latency and context. | Private working notes used before presenting the answer. |

See [Tokens and context windows](Tokens-and-Context-Windows.md) for practical context management.

## Usage and cost terminology

| Term | Plain-English meaning |
| --- | --- |
| **GitHub AI Credit** | GitHub's usage-based billing unit. One credit corresponds to USD $0.01, while token prices differ by model. |
| **Usage-based billing** | Billing derived from the model used and tokens consumed, converted into AI credits. |
| **Premium request** | A legacy request-based unit still applicable to certain existing annual individual plans. Do not mix it with AI-credit accounting. |
| **Rate limit** | A temporary restriction on request frequency or capacity. It is different from exhausting a budget. |
| **Budget or session limit** | A control used to cap or stop further AI-credit consumption. Availability depends on plan and surface. |

## Four distinctions that prevent confusion

### Model versus agent

The **model** predicts the response. The **agent** is the useful working system: model plus harness, context and tools. Switching the model changes the engine; it does not replace the whole vehicle.

### Context versus memory

**Context** is what the model can see now. **Memory** is information stored for possible retrieval later. A fact can exist in memory or a repository file without being present in the current context.

### Skill versus tool

A **skill** supplies a workflow, guidance and supporting resources. A **tool** performs an operation. A debugging skill might instruct the agent to use search, terminal and file-editing tools.

### Instructions versus policy

**Instructions** influence model behaviour; they are not an enforcement boundary. Use permissions, approvals, supported hooks, branch rules and platform policy for controls that must not be bypassed.

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Language models, context windows and thinking tokens in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Microsoft Learn: tokens, token IDs and embeddings](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
- [Microsoft.ML.Tokenizers: encoding text to token IDs and decoding them](https://learn.microsoft.com/en-us/dotnet/ai/how-to/use-tokenizers)
- [Copilot SDK agent loop and its SDK-specific definition of a turn](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/agent-loop)
- [Managing context in Copilot CLI](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/copilot-cli/context-management)
- [Trust and safety in VS Code: checkpoints and rollback](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Legacy request-based billing](https://docs.github.com/en/copilot/reference/copilot-billing/request-based-billing-legacy/github-copilot-premium-requests)
