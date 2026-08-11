# GitHub Copilot at work

_A practical guide for new and regular Copilot users · Last reviewed 11 August 2026_

This wiki is for people receiving a Copilot licence for the first time, as well as developers who have used it for a while but mostly rely on code completion or chat.

It explains how Copilot works inside the IDE, how to give it useful context and how features such as instructions, skills and custom agents fit together.

Our workplace guidance covers VS Code, Visual Studio and JetBrains. The examples are VS Code-first where the products differ; the [technology support table](Copilot-Technologies/Choose-the-right-technology.md#current-ide-support) shows what is available in each IDE.

Read in whatever order helps. Choose the route that matches what you need today.

## My first ten minutes

Copilot offers several ways to work. Start with the smallest one that fits what you are doing:

| Interaction | Use it for | What happens |
| --- | --- | --- |
| Inline suggestion | Completing the line or nearby code | A suggestion appears in the editor as you type |
| Chat | Explanations, questions and advice | Copilot replies without taking control of a larger task |
| Inline chat | A question or edit tied to selected code | The conversation stays beside the code you are working on |
| Focused edit | A bounded change you want to review | Copilot proposes an edit or diff for the selected scope |
| Agent task | Investigation, multi-file work and validation | Copilot can use tools, make changes, run commands and react to results |

Open chat from the IDE you use:

- **VS Code:** open the Chat view in the right sidebar, then use the agent picker when you want Ask, Plan or Agent behaviour
- **Visual Studio:** select the Copilot badge in the upper-right corner, or use **View > GitHub Copilot Chat**. Select **Ask** in the chat box to switch to Agent when needed
- **JetBrains:** select the Copilot Chat icon in the right tool window

For a safe first request, open a file you recognise and ask:

```text
Explain what this file does, identify its main dependencies and do not change anything.
```

You can move to Agent work once you are comfortable reviewing context, tool activity and proposed changes.

## I want the mental model first

Start here if terms such as model, context, token, tool or agent are still a little vague:

1. [How Copilot works in your IDE](How-Copilot-Works.md)
2. [Tokens and context windows](Tokens-and-Context-Windows.md)
3. [One prompt, many rounds](One-Prompt-Many-Rounds.md)
4. [Copilot terminology without the headache](Copilot-Terminology.md) - keep this page nearby as a reference

## I want to improve how I work

- [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
- [Custom instructions and `AGENTS.md`](Copilot-Technologies/Custom-instructions.md)
- [Prompt files and other useful IDE features](Copilot-Technologies/Prompt-files-and-other-IDE-features.md)

## I need to choose a Copilot technology

Open [Copilot technologies](Copilot-Technologies.md) for the full section, or jump directly to:

- [Custom instructions and `AGENTS.md`](Copilot-Technologies/Custom-instructions.md)
- [Agent skills](Copilot-Technologies/Agent-skills.md)
- [Prompt files and other useful IDE features](Copilot-Technologies/Prompt-files-and-other-IDE-features.md)
- [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
- [Advanced: context across skills, agents and models](Copilot-Technologies/Context-and-models.md)

## How reliable is this guidance?

Each main page links to the GitHub, Microsoft or VS Code documentation used to support it. Product behaviour that remains uncertain or version-specific is marked **To Test**.

- [To Test](To-Test.md) contains the experiments we still need to run
- [Sources and maintenance](Sources-and-Maintenance.md) explains how claims and diagrams are checked

## Sources

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [GitHub Copilot quickstart for supported IDEs](https://docs.github.com/en/copilot/get-started/quickstart)
- [Get started with Copilot in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/visual-studio-github-copilot-get-started?view=visualstudio)
- [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
