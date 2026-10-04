# How Copilot works in your IDE

_For new and regular Copilot users - Last reviewed 4 October 2026_

GitHub Copilot is a language model plus the software in your IDE that feeds it and acts for it. This wiki calls that software the **agent harness**, or **harness** for short.

[[_TOC_]]

## The 30-second explanation

1. You type your request
2. The harness builds the model input: your request plus instructions, history and anything else it thinks the model needs
3. The model replies with text, tool requests, or both
4. The harness runs the tools, asking you first where approval is needed
5. The tool results go into the next model input
6. Steps 2 to 5 repeat until the model replies with no tool requests. That reply is the final response

Each pass through steps 2 to 5 is one **model call**.

> **A word about "turn".** Different documentation uses it differently. Some count every step as a turn: a *user turn* when you send a message, an *LLM turn* each time the model is called, a *tool turn* when a tool runs. VS Code's Cache Explorer lists each model call as a "model turn", and several SDKs count model calls in their `max_turns` limits. **In this wiki a turn is the whole exchange: your message, all the work Copilot does, and its final answer. Each model call inside it is a round.** We never qualify "turn"; for the steps inside a round we say *your request*, *model input*, *model call*, *model output* (which may contain *tool requests*), *tool result* and *final response*. When you read "turn" elsewhere, check which meaning is in use.

A quick question takes one round. Fixing a bug can take several rounds of searching, reading, editing and testing.

![A user request passing through the harness and model, with an optional tool loop, before a final response returns](Media/agent-loop.svg =760x)

The model keeps no memory between calls. Every call, including the first call of your next message, is sent the whole conversation again by the harness. That is why long sessions get slower and dearer, and why a new session "forgets". The [whiteboard analogy](Tokens-and-Context-Windows.md#the-whiteboard-analogy) pictures this.

![Two stacked model inputs: the second turn's input contains everything from the first turn again, plus the tool results, the reply and your new message](Media/turn-two-resends-turn-one.svg =760x)

## How the model works

A language model is trained on an enormous amount of text to do one job: given the text so far, predict the next token. If the training text were only "It is a good morning" and "They shouted good morning!", then after the input "He shouted good" the model would put almost all of its probability on "morning", because that is the only continuation it has ever seen. Real models are trained on far more text, so they learn patterns rather than memorising sentences, and the same word gets different predictions depending on everything that came before it.

That is the whole generation step: tokens in, a probability for every possible next token out. One token is chosen and appended, and the model runs again, until it produces a stop token. Prose, tool requests and any reasoning text are all produced this way.

![Measured next-token probabilities from GPT-2 for three short inputs. "It is a good" is followed by idea 18 percent, thing 13 percent, time 6 percent. "He shouted good" is followed by a hyphen 30 percent, night 17 percent, morning 12 percent. "public static void" is followed by main 62 percent, Main 12 percent, set 1 percent](Media/next-token-probabilities.svg =760x)

The numbers in the diagram are real: they come from GPT-2, the smallest public GPT model, run locally. Current models are far larger and are tuned further after training, so their numbers differ, but the mechanism is the same.

Three things follow:

- **No state between calls.** The only "memory" is whatever text the harness puts back in the window.
- **Everything in the window counts.** The prediction depends on the whole input, not just the last few words. "good" on its own predicts punctuation; after "He shouted" it predicts "night" and "morning". An irrelevant file in the window is not harmless padding: it is part of what the next token is predicted from.
- **Reasoning is more tokens.** Text the model writes becomes part of what it predicts from next, so good intermediate text makes good later text more likely, and junk makes junk more likely.

Irrelevant context does harm, not just take up space. Models get worse at using a fact when it sits in the middle of a long input, and as the input grows longer and noisier, even when the fact is there ([Liu et al., 2023](https://arxiv.org/abs/2307.03172); [Chroma, 2025](https://www.trychroma.com/research/context-rot)).

## What reaches the model

What you type is a small part of what the model sees.

![Illustrative categories that can form the model input](Media/context-assembly.svg =760x)

That input can include:

- Built-in system instructions and descriptions of available tools
- Repository, path-specific or personal instructions
- The selected custom agent and any skill loaded for the task
- Your current request and earlier messages in the session
- Editor signals such as the active file, selection, visible errors and Git state
- File content or other material you explicitly reference
- Search, file, terminal and editing results from earlier rounds

Copilot does not send your whole repository. A file reaches the model only if it is open, you attach it, or the model asks to search or read it.

## The four parts worth remembering

### 1. The model generates the next output

This is the next-token predictor described above. Models differ in quality, speed, context-window size, tool use and price, and you can switch between them in the model picker.

### 2. The harness runs the experience

The harness is the software around the model. It:

- Assembles the model input
- Describes the available tools
- Validates and executes tool requests
- Returns tool results as context for later calls
- Manages the agent loop, approvals and limits
- Gives each model family its own tools and system prompt, because models are trained differently: Claude models edit files with `replace_string_in_file`, GPT models with `apply_patch`, and Gemini models get reminders to call tools instead of narrating them

The VS Code team puts it this way: *the model is the engine; the harness is the car.* Swapping the engine changes performance, but the car decides where the engine's power goes.

### 3. Context gives the model information for this call

A good request helps by stating:

- The outcome you want
- Where Copilot should work
- Which constraints matter
- How success should be checked

### 4. Tools give the model hands

The model cannot open a file, edit code or run a test by itself. It asks the harness to, using tools.

Common tools can:

- Search for files, symbols or text
- Read and edit files
- Run terminal commands and tests
- Inspect errors and source-control changes

The model requests one or more tools and supplies their arguments. The harness checks and performs the actions, asking you to approve them where required, then returns the results. A failed test can therefore guide the next edit in another round.

## What Copilot does not automatically know

You may still need to supply:

- A team decision that was never written down
- Details from another independent chat
- The exact component or behaviour you mean
- A current external requirement
- The command or evidence that proves the work is correct
- A clear boundary when words such as "improve" could cover half the repository

Put them in your message, or in a file in the repository, and Copilot stops guessing.

## Where customisations fit

Instructions, skills, custom agents and prompt files change the guidance or working setup available to Copilot. The [technology chooser](Copilot-Technologies/Choose-the-right-technology.md) compares them in one place.

## What to read next

- See [One turn, many rounds](One-Turn-Many-Rounds.md) for a worked agent loop
- Learn about capacity and compaction in [Tokens and context windows](Tokens-and-Context-Windows.md)
- Look up a term in the [Copilot glossary](Copilot-Glossary.md)

## Sources

- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [Agent harnesses in VS Code](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Language models in VS Code](https://code.visualstudio.com/docs/agents/concepts/language-models)
- [Agents and the agent loop](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context)
- [Liu et al., Lost in the Middle: How Language Models Use Long Contexts (2023)](https://arxiv.org/abs/2307.03172)
- [Chroma, Context Rot: How Increasing Input Tokens Impacts LLM Performance (2025)](https://www.trychroma.com/research/context-rot)
