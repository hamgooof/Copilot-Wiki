# Copilot 101

_A first-day guide to working with Copilot. Last reviewed 4 October 2026._

Start with something you can check in a minute. Give Copilot more room only once you trust what it does with less.

[[_TOC_]]

## What Copilot can help with

Use the lightest option that does the job:

| Interaction | Useful for | What happens |
| --- | --- | --- |
| Inline suggestion | Completing the line or nearby code | A suggestion appears as you type |
| Ask or read-only chat | Explanations, questions and investigation | Copilot answers without editing your code |
| Inline chat | A question or edit tied to selected code | The conversation stays beside that code |
| Agent mode | Investigation, multi-file work and validation | Copilot can use enabled tools, edit files, run commands and respond to results |
| Plan | Researching and agreeing an approach before edits | Copilot investigates and proposes a plan |

Inline suggestions are free. Chat, Plan and Agent all cost credits, and Agent costs the most because one message can trigger many model calls. See [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md).

Use Agent mode when the task needs Copilot to look around, change files and check its own work. Keep the task bounded and review what it does.

## Open Copilot in your IDE

- **VS Code:** open the Chat view and use the picker to choose Ask, Plan or Agent mode
- **Visual Studio:** select the Copilot badge or use **View > GitHub Copilot Chat**, then use the controls in the chat box
- **JetBrains IDEs:** open the Copilot Chat tool window and pick the mode from the chat box

If you cannot find a mode, say what you want in the message instead: "explain only" or "you may edit these files".

## Try a read-only first request

Open a file you recognise and ask:

```text
Explain what this file does and identify its main dependencies. Do not change anything.
```

Point Copilot at what matters: the open file, a selection, or a named file or folder. Ask about one thing, not the whole repository. Say "do not change anything" when you only want an explanation. One checkable question gives you one checkable answer.

## Try your first Agent task

Here is a bounded first task that has a clear finish line:

```text
Find the test project for this service. Do not edit files. Run the focused tests for this service if you can, then report the command used, the result and any blockers.
```

The prompt has an outcome, a limit and the evidence you want back. While it works you will see progress messages, tool activity and approval prompts. These are summaries of what it is doing, not its reasoning. Approve a command or edit only when you understand its purpose and scope.

Your first editing task should follow the same shape. For example, ask it to update one validation message in a named file and run the focused test. Review the resulting diff before accepting it.

## Review the result

Copilot can be wrong while sounding sure. Check its work against what you asked for.

- Check that it stayed within the requested files and behaviour
- Read the diff, especially generated or broadly formatted changes
- Run or inspect the relevant tests yourself when the change matters
- Exercise the change itself (call the endpoint, open the page). Tests that Copilot wrote can pass while the feature is broken
- Ask a follow-up when the evidence is incomplete: "What did you check, and what remains uncertain?"

## What happens underneath

Copilot combines a language model with software in your IDE. That surrounding software is the harness: it prepares the information the model sees and runs requested tools such as search, file editing and tests.

[How Copilot works](How-Copilot-Works.md) explains the model, harness, context and tools in full. [One turn, many rounds](One-Turn-Many-Rounds.md) shows why an Agent task can involve several internal cycles.

## Where to go next

- Learn the [mental model behind Copilot](How-Copilot-Works.md)
- Learn [how to work efficiently and manage cost](Working-Efficiently-and-Managing-Cost.md)
- Build your first customisation: [Custom instructions](Copilot-Technologies/Custom-instructions.md)
- Choose a shared [Copilot customisation](Copilot-Technologies.md) when a useful behaviour should be reused by a team

## Sources

- [GitHub Copilot quickstart for supported IDEs](https://docs.github.com/en/copilot/get-started/quickstart)
- [GitHub Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Get started with Copilot in VS Code](https://code.visualstudio.com/docs/copilot/getting-started)
- [Get started with Copilot in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/visual-studio-github-copilot-get-started?view=visualstudio)
- [Get started with Copilot in JetBrains](https://docs.github.com/en/copilot/get-started/getting-started-with-github-copilot?tool=jetbrains)
