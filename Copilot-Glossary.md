# Copilot glossary

_Plain-English definitions for new and regular Copilot users - Last reviewed 4 October 2026_

The first three sections cover the terms this wiki uses most.

[[_TOC_]]

## Text and working space

| Term | Plain-English meaning | Whiteboard analogy |
| --- | --- | --- |
| **User request / user message** | The message you type and send to Copilot. This wiki normally calls it your **request** | The task you bring to the board |
| **Model input (assembled prompt)** | The complete package prepared and sent to the model. Documentation may shorten this to *prompt* | The incoming tiles placed on the board for this call |
| **Context** | Everything the model can see on this call | The information currently available at the board |
| **Token** | A piece of text used as a counted model unit; each token has a numeric token ID | One numbered magnetic tile |
| **Cached input / prompt cache** | The start of the model input that matches a recent earlier call, which the provider can reuse at a lower price | Tiles already on the board from last time, so they do not need placing again |
| **Context window** | The maximum token capacity shared by model input, generated output and supported reasoning | The fixed size of the whole board |

**Prompt** is used inconsistently across products and documentation. In this wiki, **request** means what you type, **model input** means the full package prepared for a model call, and **prompt file** means a saved request that a person deliberately runs. Bare *request* never means a model call; "tool request" and "approval request" keep their own meanings.

See [Tokens and context windows](Tokens-and-Context-Windows.md) for the fuller explanation of tokenisation, capacity and compaction.

## Who does what

| Term | Plain-English meaning | Analogy |
| --- | --- | --- |
| **Model** | Generates output one token at a time by predicting the next token from the current context | The engine: models differ in power, speed, efficiency and suitability |
| **Tool** | A callable ability such as searching, reading, editing or running a command | The hands and equipment used to perform work |
| **Agent harness** | Software around the model that supplies context and tools, validates and executes tool requests, manages approvals and limits, and repeats the loop | The vehicle around the engine: controls, steering and connection to the environment |
| **Agent** | A working system that uses a model through a harness to pursue a task | The complete working setup |

In this wiki, the harness is GitHub Copilot.

## Conversation and agent terms

> **"Turn" means different things in different tools.** In this wiki a turn is your message, all the work Copilot does, and its final answer; each model call inside it is a round. See [How Copilot works](How-Copilot-Works.md#the-30-second-explanation) for the full note.

| Term | Meaning used in this wiki | Also called |
| --- | --- | --- |
| **Agent mode** | An IDE chat mode in which Copilot can use tools and continue through several rounds | |
| **Plan agent** | A built-in agent that researches without editing, asks clarifying questions and prepares an implementation plan | Plan mode |
| **Turn** | Everything from one message you send to Copilot's final answer, including any visible progress, tools, approvals and edits. It can contain many rounds | VS Code blog: *turn*. Codex: *turn*. Cache Explorer: *user request*. Copilot SDK: *turn* in its steering docs, *user message* in its agent-loop docs. Legacy GitHub billing: one *premium request* |
| **Round** | One pass through the agent loop: build model input, make **one model call**, run any requested tools, record results. One turn can contain many rounds | VS Code docs: *step*. Codex and Copilot SDK: *iteration*. .NET (Microsoft.Extensions.AI): *iteration* (`MaximumIterationsPerRequest`). **Copilot SDK agent-loop docs, Claude Agent SDK, OpenAI Agents SDK and VS Code Cache Explorer: *turn*.** The word is used both ways, sometimes by the same vendor, so this wiki avoids it for the inner loop |
| **Model call** | One model input sent to the model and one response back. Each round makes exactly one | VS Code debug logs: *model request* or *model interaction*. Token-efficiency blog: *API request*. Anthropic API: one *Messages API response* |
| **Session** | One conversation with its own history; cached context and AI-credit session limits apply per session | Codex: *thread*. Claude Code, Copilot CLI and VS Code: *session* |
| **Run** (VS Code term) | All the rounds in one turn. Not used elsewhere in this wiki | OpenAI Agents SDK: *run*. Claude Code: *agentic loop* |
| **AI credit** | GitHub's billing unit (USD 0.01), charged on tokens in each model call. GitHub does not bill a fixed amount per turn or per round | Legacy: *premium request* (one per user prompt, multiplied by the model's rate; tool calls not counted) |
| **Handoff** | A suggested switch from one custom agent to another in the same conversation; the next agent receives the visible conversation so far | |
| **Model output** | Prose, one or more tool requests, or both, generated by the model | |
| **Tool request** | Model output asking the harness to execute a tool with particular arguments | |
| **Tool result** | Evidence returned after the harness executes a tool; it may become context for a later call | |
| **Compaction** | Replacing some older conversation detail with a shorter summary to create context-window space | |

[One turn, many rounds](One-Turn-Many-Rounds.md) shows the turn, round and tool terms in a worked example.

## How the model chooses its output

| Term | Plain-English meaning |
| --- | --- |
| **Next-token prediction** | How a model generates: it scores every possible next token given the whole window, picks one, appends it and repeats |
| **Attention** | How the model decides which tokens in the window matter for the next token |
| **Distractor** | Material in the window that is not needed for the task and can pull the answer off course, even when the needed fact is present. See [Lost in the Middle](https://arxiv.org/abs/2307.03172) |
| **Context rot** | The tendency for answers to get worse as the window grows, independent of whether the needed fact is there. See [Context Rot](https://www.trychroma.com/research/context-rot) |

[How Copilot works](How-Copilot-Works.md#how-the-model-works) explains these in a short section.

## Customisation terms

| Term | Plain-English meaning |
| --- | --- |
| **Custom instructions** | Standing guidance automatically applied within a repository or path scope |
| **Skill** | A reusable playbook for performing one kind of task, optionally with scripts, examples and references |
| **Prompt file** | A saved job request that a person deliberately invokes |
| **Custom agent** | A reusable worker configuration with a role, instructions, tools and optional model choice |
| **Subagent** | A separate worker invoked to handle a delegated task and return a result |
| **[DEFERRED, not in 2.0]** **Spec-Driven Development (SDD)** | Agreeing a specification and a reviewed plan before the agent edits code, then reviewing the result against the specification. A ticket's acceptance criteria can be the specification |
| **Repository-knowledge index** | Short index pages that route a task to the few documents that matter, so the agent reads only what is relevant. Like the lobby directory: it tells you which floor, not what is in every office |

See [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md) for the practical skill, custom-agent and prompt-file comparisons.

## Token-count quick reference

| Count | Meaning |
| --- | --- |
| **Input tokens** | The model input processed for a model call |
| **Output tokens** | Text and tool requests generated by the model |
| **Cached input tokens** | A matching input prefix that the provider can reuse |
| **Reasoning tokens** | Internal working reported by supported reasoning models |

See [Tokens and context windows](Tokens-and-Context-Windows.md) for how tokenisers, counts and context growth fit together.

## Useful distinctions

- **Skill and tool:** a skill describes how to perform a workflow; a tool performs an operation
- **Instructions and enforcement:** instructions influence model behaviour; formatters, analysers, tests, permissions and repository controls enforce deterministic rules

## What to read next

- Start with the mental model in [How Copilot works](How-Copilot-Works.md)
- Compare customisations in [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Microsoft Learn: tokens and token IDs](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
- [Copilot customisation cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)
- [Agent customisation decision matrix in VS Code](https://code.visualstudio.com/docs/agents/concepts/customization)
