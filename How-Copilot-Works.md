# How Copilot works in your IDE

_For new and regular Copilot users - Last reviewed 4 October 2026_

GitHub Copilot is a coding experience built around a language model, with software connecting that model to your IDE. In this wiki, that surrounding software is called the **agent harness**.

[[_TOC_]]

## The 30-second explanation

Each time the harness sends model input to the model is a **model call**.

1. You send Copilot a request
2. The harness assembles the model input
3. The model generates output: prose, one or more tool requests, or both where the model integration supports it
4. The harness validates and executes requested tools, asking you for approval where required
5. Tool results become available as context for the next model call
6. The loop continues until Copilot returns its final response

Your **request** is the message you type. The **model input** (the **assembled prompt** in the diagram below) is the complete package the harness sends for one model call. **Context** is the information contained in that package and therefore available to the model.

> **A word about "turn".** Different documentation uses it differently. Some count every step as a turn: a *user turn* when you send a message, an *LLM turn* each time the model is called, a *tool turn* when a tool runs. VS Code's Cache Explorer lists each model call as a "model turn", and several SDKs count model calls in their `max_turns` limits. **In this wiki a turn is the whole exchange: your message, all the work Copilot does, and its final answer. Each model call inside it is a round.** When you read "turn" elsewhere, check which meaning is in use.

A quick question might need one round (one model call) and no tools. Fixing a feature can require several rounds of searching, reading, editing, testing and correcting.

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

Irrelevant context tends to hurt rather than merely take up space. Research has found that accuracy drops when the relevant passage sits in the middle of a long input ([Liu et al., *Lost in the Middle*, 2023](https://arxiv.org/abs/2307.03172)), and that performance degrades as input grows and as distractors are added, even when the needed fact is present ([Chroma, *Context Rot*, 2025](https://www.trychroma.com/research/context-rot)). Architectures differ in how they are built and trained; next-token prediction is the common core.

## What reaches the model

The request in the chat box is only the part you typed. Before calling the model, the harness can assemble a larger model input containing instructions, conversation history, tool descriptions and task information.

![Illustrative categories that can form the model input](Media/context-assembly.svg =760x)

Depending on the IDE, mode and task, that input can contain:

- Built-in system instructions and descriptions of available tools
- Repository, path-specific or personal instructions
- The selected custom agent and any skill loaded for the task
- Your current request and earlier messages in the session
- Editor signals such as the active file, selection, visible errors and Git state
- File content or other material you explicitly reference
- Search, file, terminal and editing results from earlier rounds

These categories explain what may be present. They are not shown in assembly order, and not every category appears in every model call.

The whole repository is not automatically copied into the model input. File content can be supplied as editor context, attached explicitly or retrieved when the model requests search and read tools.

## The four parts worth remembering

### 1. The model generates the next output

The language model processes the current input and generates output one token at a time. A token represents a piece of text and has a numeric token ID. The output can be prose, one or more tool requests, or both where the model integration supports it.

Models differ in capability, speed, context-window size, tool use and cost. Think of the model as the engine: changing it can alter how the same surrounding Copilot experience performs.

### 2. The harness runs the experience

The harness is the software around the model. It:

- Assembles the model input
- Describes the available tools
- Validates and executes tool requests
- Returns tool results as context for later calls
- Manages the agent loop, approvals and limits
- Adapts the experience for different model families: for example, Claude models edit files with `replace_string_in_file` and GPT models with `apply_patch`, and the harness selects different system prompts for different models

The VS Code team puts it this way: *the model is the engine; the harness is the car.* Swapping the engine changes performance, but the car decides where the engine's power goes.

### 3. Context gives the model information for this call

Context can include your request, instructions, relevant files or editor selections, earlier conversation, tool descriptions and results from searches, file reads or terminal commands.

A good request helps by stating:

- The outcome you want
- Where Copilot should work
- Which constraints matter
- How success should be checked

The request is one source of context, not the whole of it. The context window has a finite capacity. Useful evidence can improve the next decision; irrelevant material still occupies space and can distract the model.

### 4. Tools give the model hands

The model cannot directly open a file, edit code or run a test. Tools give it hands to work in your local workspace.

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

Writing these details into the request or an accessible repository file reduces discovery work and guessing.

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
