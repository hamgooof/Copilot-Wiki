# Copilot terminology without the headache

_For new and regular Copilot users who want plain-English definitions · Last reviewed 11 August 2026_

Copilot terminology becomes confusing because several products reuse the same words differently. You do not need to memorise everything on this page. Start with the fundamentals, then return to the extended terms when you encounter them.

The most useful mental model is simple: the model writes and decides; the IDE supplies context, tools and the loop that turns those decisions into useful work.

[[_TOC_]]

## Start here: eight ideas worth knowing

1. A **model** reads the information supplied to it and decides what to return. Think of it as the engine inside the Copilot experience: engines differ in power, speed, efficiency and running cost
2. **Context** is the task information the model can see for its current call. Picture the relevant information as tiles placed on a whiteboard
3. A **context window** is the whole whiteboard: the input, answer and any supported reasoning share its capacity
4. A **token** is the numbered unit used to measure that capacity. One tile can represent a word, word fragment, punctuation mark or special symbol
5. A **tool** gives the model a way to act in your workspace, such as reading a file or running a test
6. An **agent** is a model working inside a harness with context and tools
7. A **turn** runs from your message to Copilot's final response
8. A **round** is one pass through the internal model-and-tools loop. One turn can contain many rounds

The worked visual and example are on [One prompt, many rounds](One-Prompt-Many-Rounds.md).

## Two more useful terms

| Term | What it means here | Analogy |
| --- | --- | --- |
| **Prompt** | The effective input assembled for the model. It can include your message, instructions, history, supplied file content and tool results. | The complete set of incoming tiles placed on the whiteboard. |
| **Session** | One independent conversation with its own history and context. | A project room. A new session opens a fresh room without the boxes from the previous task. |

## Extended agent terminology

| Term | Plain-English meaning |
| --- | --- |
| **Agent harness** | The product layer around the model. It assembles context, exposes and executes tools, manages the loop and connects the model to the editor. |
| **Agent** | A model operating through a harness with context and tools so it can generate text and perform work. |
| **Agent mode** | An IDE interaction in which Copilot can use tools and continue through several rounds to complete a task. |
| **Tool** | A callable ability such as reading a file, editing code or running a command. |
| **Tool call** | A model's structured request for the harness to execute a tool with particular arguments. |
| **Tool result** | The output returned after the harness executes a tool. It can become context in the next round. |
| **Agent loop** | The repeated "think, act, observe and adjust" process used to complete a turn. |
| **Custom agent** | A reusable specialist definition with its own instructions, tools and optional model choice. |
| **Subagent** | A separate agent invoked to handle delegated work, usually with an isolated context. |

## Customisation terminology

| Term | Plain-English meaning |
| --- | --- |
| **Custom instructions** | Standing guidance automatically applied within a defined scope, such as a repository or file path. |
| **`AGENTS.md`** | A local operating manual used by supporting AI agents. Discovery rules vary by product. |
| **Skill** | A playbook containing task-specific guidance and, optionally, scripts, templates or reference material. It is loaded when relevant. |
| **Prompt file** | A reusable job request that a person deliberately invokes. |

For help choosing between these, use [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md).

## Context, model settings and state

| Term | Meaning used in this wiki |
| --- | --- |
| **Conversation history** | Earlier user messages, assistant responses and relevant tool activity retained in the current session. |
| **Compaction** | Replacing older conversation detail with a shorter summary to create room in the context window. Some detail can be lost. |
| **Prompt cache** | A model provider's mechanism for reusing a matching prefix from earlier input, potentially reducing cost and latency. |
| **Provider** | The company supplying the model, such as OpenAI, Anthropic or Google. GitHub and the IDE connect that model to Copilot. |
| **Reasoning effort** | A setting available for some models that controls how much internal reasoning they use. Higher effort can increase time and token use. |
| **Context size** | The amount of context-window capacity offered by a model. In VS Code this is shown with the model details rather than being a separate everyday setting. |

The [Tokens and context windows](Tokens-and-Context-Windows.md#token-counts-in-more-detail) page explains token IDs and the different token counts.

## Usage and cost terminology

| Term | Plain-English meaning |
| --- | --- |
| **GitHub AI Credit** | GitHub's usage-based billing unit. One credit corresponds to USD $0.01, while token prices differ by model. |
| **Usage-based billing** | Billing derived from the model used and tokens consumed, converted into AI credits. |

## Five distinctions that prevent confusion

### Model versus agent

The **model** predicts the response. The **agent** is the complete working system: model, harness, context and tools. Switching the model changes the engine while the surrounding system remains.

### Agent versus Agent mode versus custom agent

An **agent** is the working system. **Agent mode** is the IDE interaction that lets it use tools and iterate. A **custom agent** is a reusable definition that changes the role, instructions, tools or model used by that system.

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
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
