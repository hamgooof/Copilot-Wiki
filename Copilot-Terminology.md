# Copilot terminology without the headache

_For new and regular Copilot users who want plain-English definitions · Last reviewed 11 August 2026_

Copilot terminology becomes confusing because several products reuse the same words differently. You do not need to memorise everything on this page. Start with the fundamentals, then return to the extended terms when you encounter them.

> **If you remember only one idea:** the model writes and decides; the IDE supplies context, tools and the loop that turns those decisions into useful work.

[[_TOC_]]

## Start here: seven ideas worth knowing

1. A **model** reads the information supplied to it and decides what to return. Models differ in capability, speed, context size and cost
2. **Context** is everything the model can see for its current call
3. A **context window** is the capacity shared by the input, answer and any supported reasoning
4. A **tool** gives the model a way to act in your workspace, such as reading a file or running a test
5. An **agent** is a model working inside a harness with context and tools
6. A **turn** runs from your message to Copilot's final response
7. A **round** is one pass through the internal model-and-tools loop. One turn can contain many rounds

Tokens are the units used to measure how much of the context window is being used. That is enough detail to begin; the [token section](#tokens-in-more-depth) explains the different counts later.

The worked visual and example are on [One prompt, many rounds](One-Prompt-Many-Rounds.md).

## Fundamental terminology

| Term | What it means here | Analogy |
| --- | --- | --- |
| **Token** | One unit in the count used to measure model input and output. Text is split into pieces, and each piece maps to a number the model can process. | A numbered magnetic tile representing a word, word fragment, punctuation mark or special symbol. |
| **Model** | A particular large language model offered through Copilot. Models differ in capability, speed, context size and cost. | Vehicles are designed for different jobs. A car is useful for travel; a forklift is better for lifting a pallet. Some are faster, stronger or more efficient than others. |
| **Prompt** | The effective input assembled for the model. It can include your message, system instructions, history, supplied file content and tool results. | A briefing pack prepared before a meeting. |
| **Context** | The task-specific information available to the model during its current call. It is separate from general knowledge learned during training. | The numbered tiles currently placed on a whiteboard, alongside the model's existing training. |
| **Context window** | The combined token capacity available for one model call. The input prompt, generated output and any supported thinking tokens share that capacity. | The whole whiteboard, including the room needed for the answer. |
| **Session** | One independent conversation with its own history and context. | A project room. A new session opens a fresh room without the boxes from the previous task. |

## Extended agent terminology

| Term | Plain-English meaning | Analogy |
| --- | --- | --- |
| **Agent harness** | The product layer that assembles context, exposes and executes tools, manages the loop and adapts different models to the editor. | The vehicle around an engine: controls, dashboard, gearbox and safety systems. |
| **Agent** | A model operating through a harness with context and tools so it can generate text and perform work. | A worker equipped with a brief, tools and a working process. |
| **Tool** | A callable ability such as reading a file, editing code, running a command or querying an external service. | A tool on the worker's bench. |
| **Tool call** | A model's structured request for the harness to execute a tool with particular arguments. | A work order saying which tool to use and how. |
| **Tool result** | The output returned after the harness executes a tool. It can become context in the next round. | Evidence brought back to the workbench. |
| **Agent loop** | The repeated “think → act → observe → think again” process used to complete a turn. | Diagnose, try, inspect the result and adjust. |
| **Custom agent** | A reusable specialist definition with its own instructions, tools and optional model choice. | A named specialist role with a particular toolkit and operating brief. |
| **Subagent** | A separate agent invoked to handle delegated work, usually with an isolated context. | A specialist called in to investigate one part of the job. |
| **Orchestrator or parent agent** | The agent coordinating the overall task and delegating work. | A lead coordinating several specialists. |
| **Handoff** | Passing a task or a later stage of work to another agent or environment. | Transferring a job with a handover note. |
| **Autonomy** | How much an agent may do before it must pause for approval. | The spending and decision authority assigned to a role. |

## Customisation terminology

| Term | Plain-English meaning | Best mental shortcut |
| --- | --- | --- |
| **Custom instructions** | Guidance automatically applied within a defined scope, such as a repository or file path. | Standing rules. |
| **`AGENTS.md`** | A file format for persistent repository guidance used by supporting AI agents. Discovery rules vary by product. | A local operating manual. |
| **Skill** | A task-specific package containing `SKILL.md` and optionally scripts, templates or reference material. It is loaded when relevant. | A playbook with supporting equipment. |
| **Prompt file** | A reusable prompt template that a person deliberately invokes. | A reusable job-request form. |

For help choosing between these, use [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md).

## Context, state and memory terminology

| Term | Meaning used in this wiki |
| --- | --- |
| **Conversation history** | Earlier user messages, assistant responses and relevant tool activity retained in the current session. |
| **Compaction** | Replacing older conversation detail with a shorter summary to create room in the context window. Some detail can be lost. |
| **Checkpoint** | A VS Code restore point used to roll back file changes made during an agent session. |
| **Prompt cache** | A provider mechanism that reuses a matching prefix from earlier model input, potentially reducing cost and latency. |

## Tokens in more depth

Copilot splits text into pieces and maps each piece to a number called a **token ID**. You rarely need to inspect those numbers. The useful part is the count, because it affects context limits, usage and sometimes cost.

Tokenisation can vary between models, so the same text may not always produce exactly the same count.

| Token type | What it represents | Simple analogy |
| --- | --- | --- |
| **Input tokens** | Everything counted in the input for the current round: system instructions, tool definitions, customisations, your message, history, supplied file content and earlier tool results. | Items placed into the briefing pack. |
| **Output tokens** | Everything counted while the model produces text or structured tool requests. | The material produced in the response. |
| **Cached input tokens** | An unchanged prefix of input that the provider can reuse from a previous model call. Exact pricing and eligibility depend on the model and product. | Reusing the unchanged first chapters of the briefing instead of processing them again at full cost. |
| **Thinking or reasoning tokens** | Tokens used by supported reasoning models while working through a problem. They may not appear in the final response but can affect usage, latency and context. | Working space used before presenting the answer. |

See [Tokens and context windows](Tokens-and-Context-Windows.md) for practical context management.

## Usage and cost terminology

| Term | Plain-English meaning |
| --- | --- |
| **GitHub AI Credit** | GitHub's usage-based billing unit. One credit corresponds to USD $0.01, while token prices differ by model. |
| **Usage-based billing** | Billing derived from the model used and tokens consumed, converted into AI credits. |
| **Premium request** | A legacy request-based unit still applicable to certain existing annual individual plans. Do not mix it with AI-credit accounting. |
| **Rate limit** | A temporary restriction on request frequency or capacity. It is different from exhausting a budget. |

## Four distinctions that prevent confusion

### Model versus agent

The **model** predicts the response. The **agent** is the complete working system: model, harness, context and tools. Switching the model changes the engine while the surrounding system remains.

### Context versus memory

**Context** is what the model can see now. **Memory** is information stored for possible retrieval later. A fact can exist in memory or a repository file without being present in the current context.

### Skill versus tool

A **skill** supplies a workflow, guidance and supporting resources. A **tool** performs an operation. A debugging skill might instruct the agent to use search, terminal and file-editing tools.

### Instructions versus policy

**Instructions** influence model behaviour. Use permissions, approvals, branch rules and platform policy for controls that must be enforced.

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Context assembly in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Language models, context windows and thinking tokens in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Microsoft Learn: tokens, token IDs and embeddings](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
- [Microsoft.ML.Tokenizers: encoding text to token IDs and decoding them](https://learn.microsoft.com/en-us/dotnet/ai/how-to/use-tokenizers)
- [Trust and safety in VS Code: checkpoints and rollback](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Usage-based billing for organisations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Legacy request-based billing](https://docs.github.com/en/copilot/reference/copilot-billing/request-based-billing-legacy/github-copilot-premium-requests)
