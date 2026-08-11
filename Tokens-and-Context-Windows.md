# Tokens and context windows

_For new and regular Copilot users · Last reviewed 11 August 2026_

The context window is the working capacity for one model call. The assembled input, generated output and any supported thinking tokens share that capacity. Start with the whiteboard analogy below if those terms are new to you.

[[_TOC_]]

## The whiteboard analogy

Imagine the model working with a whiteboard of a fixed size.

Before each model call, the IDE prepares the information the model can see. Picture that information written on numbered magnetic tiles and placed on the board:

- Each **token** is one numbered tile representing a piece of text, punctuation or a special symbol
- The complete set of incoming tiles is the **assembled prompt**
- The **context window** is the whole board, including the space needed for the model's answer and any supported reasoning
- Later rounds can add conversation and tool-result tiles, so the board gradually fills
- **Compaction** removes some older detail and replaces it with a shorter summary
- A **new session** starts without the previous conversation history

A larger board holds more, but useful information still needs to be easy to find. Give the model the smallest complete set of context for the task.

## What a token is

For day-to-day Copilot use, a token is one unit in the count used to measure model input and output. Text is split into pieces, and each piece is mapped to a number that the model can process. A piece might be a whole word, a word fragment, punctuation or a special symbol.

Different models can use different tokenizers, so the same text can produce a different token count on another model. This matters when comparing model context limits or usage.

> **Optional technical precision:** the numbers are called token IDs. Most Copilot users only need the token count, because that is what affects context capacity and usage.

You normally do not need to count tokens manually. Remember three things:

1. Everything sent to the model counts as **input tokens**.
2. Everything the model produces counts as **output tokens**.
3. The product may reuse a matching input prefix as **cached input tokens**, often at a different price.

Some reasoning models also use **thinking tokens** while solving a problem. See [Tokens in more depth](Copilot-Terminology.md#tokens-in-more-depth) for the full comparison.

Input, output and thinking tokens share the same context-window capacity. A prompt that consumes most of the window leaves less room for reasoning and the final answer.

## What enters the assembled prompt

The message you type is only one layer of the prompt. Instructions, tool definitions, conversation history, editor signals, referenced files and earlier tool results can also consume input tokens.

The diagram and full breakdown live on [How Copilot works in your IDE](How-Copilot-Works.md#what-reaches-the-model). This page focuses on how that material is counted and grows.

## How context grows during a turn

Suppose an agent is investigating a failing test:

1. Your message, instructions and tool definitions form the initial context.
2. A search returns matching files.
3. A file-reading tool adds relevant code.
4. A test command adds terminal output.
5. The next round carries the useful history forward and adds the new result.
6. More tools can add more material before the final answer.

![Four rounds with growing accumulated input, followed by a shorter compacted prompt](Media/context-growth-across-rounds.svg =900x)

This is why a ten-word message can lead to thousands of input tokens. Later rounds may process earlier results again, although prompt caching and context management can change the effective usage.

In the tidy example above, each round adds one equal block while keeping the earlier blocks. The cumulative input is `1 + 2 + 3 + 4 = 10` block-rounds. Extend that pattern and it grows roughly with the square of the number of rounds. This illustrates context growth; Copilot billing also depends on the model, caching, output and product implementation.

How that state travels to the provider varies. For everyday use, the useful point is that the model's next decision depends on the effective prompt and history; see [Effective context versus transport](Copilot-Technologies/Context-and-models.md#effective-context-versus-transport) for the technical caveat.

## What happens when the window fills

Copilot can **compact** older conversation history: detailed earlier material is replaced by a shorter summary. This creates room but cannot guarantee that every minor decision, exact command or edge case survives.

VS Code can summarise older conversation history automatically as the window fills and supports `/compact` for manual compaction. The exact behaviour can change between versions.

Store important decisions in a file, issue or pull request when they need to survive exactly.

## Good context beats lots of context

A useful prompt can be longer than a vague one while costing less overall. Naming the relevant component, required behaviour and validation command can prevent searching, false starts and corrective rounds. See [Define the finish line before starting](Working-Efficiently-and-Managing-Cost.md#2-define-the-finish-line-before-starting) for a worked example.

## Common sources of context bloat

- Large instruction files full of rarely relevant workflows
- Several instruction files repeating the same rules
- Attaching a whole folder when two files would do
- Reusing one session for unrelated tasks
- Commands that return thousands of irrelevant log lines
- Leaving many unrelated tools enabled for every agent
- Repeatedly pasting documentation that could be retrieved when needed
- Letting a stuck agent repeat an approach without learning anything new

Context bloat affects both cost and quality. Irrelevant material competes for the model's attention and can reduce answer quality.

## Put this into practice

The practical actions are collected in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md): starting clean sessions, keeping instructions lean, controlling tool output, choosing models and steering broad investigations.

## Inspect context and usage

- VS Code exposes context information and supports automatic or manual compaction in agent sessions
- VS Code's [Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer) compares consecutive model requests and shows where a matching prompt prefix diverges
- Agent Debug Logs can show prompts, tool calls and results for supported VS Code sessions

Use measurements before claiming that an installed skill, instruction file or tool is expensive. Discovery metadata and fully loaded content are different parts of the prompt.

## Sources

- [Context assembly and practical context guidance in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Language models, context windows and thinking tokens in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Microsoft Learn: tokens, token IDs and embeddings](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
- [Microsoft.ML.Tokenizers: encoding text to token IDs and decoding them](https://learn.microsoft.com/en-us/dotnet/ai/how-to/use-tokenizers)
- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
