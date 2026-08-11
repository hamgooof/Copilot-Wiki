# GitHub Copilot at work

_A practical guide for new and regular Copilot users - Last reviewed 12 August 2026_

This wiki is for people receiving a Copilot licence for the first time and for developers who already use completion or chat but want to get more from Copilot.

It explains what happens under the hood, how to work efficiently and how instructions, skills, prompt files and custom agents fit together. Workplace guidance covers VS Code, Visual Studio and JetBrains. Examples are VS Code-first where the products differ.

[[_TOC_]]

## Choose where to start

### I want the mental model first

1. [How Copilot works in your IDE](How-Copilot-Works.md)
2. [Tokens and context windows](Tokens-and-Context-Windows.md)
3. [One prompt, many rounds](One-Prompt-Many-Rounds.md)
4. [Copilot terminology without the headache](Copilot-Terminology.md)

### I want to improve how I work

- [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
- [Custom instructions](Copilot-Technologies/Custom-instructions.md)
- [Agent skills](Copilot-Technologies/Agent-skills.md)

### I need to choose a customisation

Open [Copilot technologies](Copilot-Technologies.md) for the guided route, or use the [quick technology chooser](Copilot-Technologies/Choose-the-right-technology.md).

## My first ten minutes

Copilot offers several ways to work. Pick the smallest interaction that suits the task.

| Interaction | Useful for | What happens |
| --- | --- | --- |
| Inline suggestion | Completing the line or nearby code | A suggestion appears as you type |
| Chat or Ask | Explanations, questions and investigation | Copilot answers without editing your code |
| Inline chat | A question or edit tied to selected code | The conversation stays beside that code |
| Agent | Investigation, multi-file work and validation | Copilot can use tools, edit files, run commands and respond to results |

Open chat in your IDE:

- **VS Code:** open the Chat view and use the agent picker to choose Ask, Plan or Agent
- **Visual Studio:** select the Copilot badge or use **View > GitHub Copilot Chat**, then use the mode selector in the chat box
- **JetBrains:** open the Copilot Chat tool window and use the controls provided by your installed plugin version

For a safe first request, open a file you recognise and ask:

```text
Explain what this file does and identify its main dependencies. Do not change anything.
```

Agent is useful when a task needs investigation, edits and validation. You can use it whenever that matches the work you want Copilot to perform.

## A simple mental model

Copilot is more than the language model shown in the model picker:

```text
Copilot agent experience = model + harness + context + tools
```

- The **model** generates text and requests actions
- The **harness** connects the model to the IDE and manages the work
- **Context** is the information available for the current model call
- **Tools** give the model hands to search, read, edit and run commands

[How Copilot works in your IDE](How-Copilot-Works.md) turns that shorthand into a practical explanation.

## How reliable is this guidance?

Each page includes its review date and direct links to GitHub, Microsoft or VS Code documentation. Availability changes quickly, so check the support links when a feature is missing from your IDE or behaves differently from the examples.

## Sources

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [GitHub Copilot quickstart for supported IDEs](https://docs.github.com/en/copilot/get-started/quickstart)
- [Get started with Copilot in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/visual-studio-github-copilot-get-started?view=visualstudio)
- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
