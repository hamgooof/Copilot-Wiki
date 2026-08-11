# Tokens and context windows

_For existing Copilot users who are new to context management · Last reviewed 11 August 2026_

The context window is the working capacity for one model call. The assembled input, generated output and any supported thinking tokens share that capacity. If that sentence means nothing yet, start with the workbench analogy below; the technical detail can wait.

[[_TOC_]]

## The workbench analogy

Imagine the model is working at a bench:

- **Tokens** are the counted pieces used for the input, the model's work and its answer.
- The **assembled prompt** is the incoming material laid out for the job.
- The **context window** is the whole bench: input uses some space, and the model still needs room to work and produce output.
- **Compaction** replaces a pile of detailed notes with a shorter summary.
- A **new session** clears the bench for a different job.

A bigger bench can help, but filling it with irrelevant material still makes the work harder. The goal is not maximum context. The goal is the smallest complete set of relevant context.

## What a token is

For day-to-day Copilot use, a token is the counted unit used to measure model input and output. A token can correspond to a whole word, a word fragment, punctuation or a special symbol.

Different models can use different tokenizers, so the same text can produce a different token count on another model. This matters when comparing model context limits or usage.

> **Optional technical precision:** a tokenizer maps vocabulary items to integer token IDs. The model operates on numerical representations of those IDs and predicts output IDs; a decoder converts the result back into readable text or structured output. Product documentation usually shortens this whole pipeline to “tokens”.

You normally do not need to count tokens manually. Remember three things:

1. Everything sent to the model counts as **input tokens**.
2. Everything the model produces counts as **output tokens**.
3. The product may reuse a matching input prefix as **cached input tokens**, often at a different price.

Some reasoning models also use **thinking tokens** while solving a problem. See [Tokens in more depth](Copilot-Terminology.md#tokens-in-more-depth) for the full comparison.

Input, output and thinking tokens share the same context-window capacity. A prompt that consumes most of the window leaves less room for reasoning and the final answer.

## What enters the assembled prompt

The message you type is only one layer of the prompt:

![System instructions, customizations, user messages, history and tool results assembled into the input prompt](Media/context-assembly.svg =900x)

Depending on the surface and task, context can include:

- Built-in system instructions and harness guidance.
- Definitions of the tools available for this round.
- Personal, organisation, repository and path-specific instructions.
- A selected custom-agent profile and an invoked skill.
- Your current message.
- Earlier user and assistant messages in the session.
- Active-editor signals and explicitly referenced files.
- Retrieved code, terminal output, search results and other tool results.

Any task-specific information outside the assembled prompt—your files, current errors, decisions or workspace state—is unavailable to the model for that round. The model still has its trained knowledge, which may be incomplete or outdated.

## How context grows during a turn

Suppose an agent is investigating a failing test:

1. Your message, instructions and tool definitions form the initial context.
2. A search returns matching files.
3. A file-reading tool adds relevant code.
4. A test command adds terminal output.
5. The next round rebuilds the prompt with the useful history accumulated so far.
6. More tools can add more material before the final answer.

![Four rounds with growing accumulated input, followed by a shorter compacted prompt](Media/context-growth-across-rounds.svg =900x)

This is why a ten-word message can lead to thousands of input tokens. Later rounds may process earlier results again, although prompt caching and context management can change the effective usage.

In the tidy example above, each round adds one equal block while keeping the earlier blocks. The cumulative input is `1 + 2 + 3 + 4 = 10` block-rounds rather than four. Extend that pattern and it grows roughly with the square of the number of rounds. This is a useful warning about repeated rounds and verbose tool output—not a universal Copilot billing formula.

### What this looked like in a measured run

The tidy diagram explains the pattern. Our internal test data shows how uneven it can be in practice. One successful implementation run made 21 model calls, passed all 21 hidden checks and recorded 43.07 billed credits. Input grew from 16.5K to 40.5K tokens, with an 89.6% cache-hit rate across the run.

![Cached and fresh input tokens, billed credits and the next action across 21 model calls](Media/measured-context-cost-across-rounds.svg =900x)

Read the two charts together:

- The upper bars show the input processed by each model call, split into cached and fresh tokens.
- The lower bars show billed credits for that same call. The action label is what the model asked the harness to do next; it is **not** a standalone price for using Read, Write or Terminal.
- Calls 5 and 8 cost more largely because the model generated 2,752 and 1,908 output tokens respectively.
- At call 20, the matching cached prefix dropped to 14.8K tokens and fresh input jumped to 25.2K. That call recorded 7.31 credits. The cache match recovered on the next call.

This is observed evidence from one run, not a universal cost curve or a model comparison. It shows why total context alone does not explain the bill: fresh input, cached input, generated output and the selected model's rates all matter.

_Evidence: `Copilot-model-comparison.xlsx` (`Runs` and `Rounds`) and the matching raw OTEL JSONL for `rt-gpt-5.4-T1-rep3`, captured 5 August 2026 with VS Code 1.131.0 and Copilot Chat 0.59.0. The [chart data](Media/measured-run-gpt-5.4-T1-rep3.csv) is retained alongside the image._

How that state travels to the provider varies. For everyday use, the useful point is that the model's next decision depends on the effective prompt and history; see [Effective context versus transport](Copilot-Technologies/Context-memory-and-models.md#effective-context-versus-transport) for the technical caveat.

## What happens when the window fills

Copilot can **compact** older conversation history: detailed earlier material is replaced by a shorter summary. This creates room but cannot guarantee that every minor decision, exact command or edge case survives.

Current product behaviour differs by surface:

- VS Code documents automatic summarisation as the context window fills and supports `/compact` for manual compaction.
- Copilot CLI documentation currently describes background compaction beginning at approximately 80% of capacity and waiting if usage reaches approximately 95% before compaction finishes.

Those thresholds and interfaces can change. If information must survive exactly, store it in a file, issue or pull request rather than relying on old chat history.

## Good context beats lots of context

Vague request:

```text
Fix the authentication.
```

The agent has to discover which authentication flow, desired behaviour and validation command you meant.

Focused request:

```text
Fix expired-refresh-token handling in src/auth/refresh.ts.
Preserve the existing error response, add a regression test in the matching
test file, and run the auth unit-test command from package.json.
```

The second request is longer but may consume less overall. It reduces unproductive searching, false starts and corrective rounds.

## Common sources of context bloat

- Large instruction files full of rarely relevant workflows.
- Several instruction files repeating the same rules.
- Attaching a whole folder when two files would do.
- Reusing one session for unrelated tasks.
- Commands that return thousands of irrelevant log lines.
- Enabling every Model Context Protocol (MCP) server and tool for every agent.
- Repeatedly pasting documentation that could be retrieved when needed.
- Multiple subagents duplicating the same investigation.
- Letting a stuck agent repeat an approach without learning anything new.

Context bloat is not just a cost concern. Irrelevant material competes for the model's attention and can reduce answer quality.

## Practical context habits

### Start a fresh session for a new task

Each session has its own history. Starting a new one for unrelated work is routine context hygiene, not a failure.

### Reference the smallest useful scope

Name the component, file, symbol, issue or error. Let repository search retrieve related code instead of attaching everything pre-emptively.

### Make important information durable

- Put short, broadly applicable rules in custom instructions.
- Put occasional detailed workflows in skills.
- Put task state and decisions in a working file, issue or pull request.

### Control tool output

Filter test output, search results and logs when practical. Retrieve the full result only when the detail is genuinely useful.

### Compact at phase boundaries

After a large discovery or planning phase, compaction can create room for implementation. Review the summary when the task contains critical constraints.

### Delegate noisy independent work

A subagent can keep extensive exploration outside the main context. Give it a bounded question and specify what evidence to return, or you may simply pay for duplicated investigation.

## How to inspect rather than guess

- VS Code exposes context information and supports automatic or manual compaction in agent sessions.
- Copilot CLI provides `/context` for context composition and `/usage` for session statistics.
- VS Code's [Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer) compares consecutive model requests and shows where a matching prompt prefix diverges.
- Agent Debug Logs can show prompts, tool calls and results for supported VS Code sessions.

Use measurements before claiming that an installed skill, instruction file or MCP server is expensive. Discovery metadata and fully loaded content are not necessarily the same thing.

## Sources

- [Context assembly and practical context guidance in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Language models, context windows and thinking tokens in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Microsoft Learn: tokens, token IDs and embeddings](https://learn.microsoft.com/en-us/dotnet/ai/conceptual/understanding-tokens)
- [Microsoft.ML.Tokenizers: encoding text to token IDs and decoding them](https://learn.microsoft.com/en-us/dotnet/ai/how-to/use-tokenizers)
- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Diagnose prompt caching with the Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [Managing context in Copilot CLI](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/copilot-cli/context-management)
- [Using GitHub Copilot CLI: `/context`, `/usage` and `/compact`](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview)
- [Creating Copilot Spaces: retrieved repositories versus attached files](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/copilot-spaces/create-copilot-spaces)
