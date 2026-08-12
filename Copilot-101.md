# Copilot 101

_A first-day guide to working with Copilot. Last reviewed 12 August 2026._

GitHub Copilot can help with small questions, focused edits and larger tasks. Start with a request you can easily check, then give it only as much access and scope as the work needs.

[[_TOC_]]

## What Copilot can help with

Choose the smallest interaction that fits the job:

| Interaction | Useful for | What happens |
| --- | --- | --- |
| Inline suggestion | Completing the line or nearby code | A suggestion appears as you type |
| Ask or read-only chat | Explanations, questions and investigation | Copilot answers without editing your code |
| Inline chat | A question or edit tied to selected code | The conversation stays beside that code |
| Agent mode | Investigation, multi-file work and validation | Copilot can use enabled tools, edit files, run commands and respond to results |
| Plan (VS Code) | Researching and agreeing an approach before edits | Copilot investigates and proposes a plan. In other IDEs, ask for a read-only implementation plan instead |

Agent mode is useful whenever investigation, changes and validation match the task. Keep the task bounded and review what it does.

## Open Copilot in your IDE

- **VS Code:** open the Chat view and use the picker to choose Ask, Plan or Agent mode
- **Visual Studio:** select the Copilot badge or use **View > GitHub Copilot Chat**, then use the controls in the chat box
- **JetBrains:** open the Copilot Chat tool window and use the controls offered by your installed plugin version

Controls and names can vary by version. If a particular mode is unavailable, use a chat request that states whether you want an explanation only or permission to make edits.

## Try a read-only first request

Open a file you recognise and ask:

```text
Explain what this file does and identify its main dependencies. Do not change anything.
```

Give Copilot useful, explicit context. Refer to the current file or selected code when that is the relevant evidence; name another file or folder when the answer depends on it. Start with the smallest useful scope rather than asking about the whole repository.

In every IDE, make the boundary clear in the request: say "do not change anything" for an explanation, or name the files you want it to consider. The answer is easier to review when you have asked one checkable question.

## Try your first Agent task

Here is a bounded first task that has a clear finish line:

```text
Find the test project for this service. Do not edit files. Run the focused tests for this service if you can, then report the command used, the result and any blockers.
```

This gives Copilot an outcome, a boundary and the evidence you want back. In Agent mode you may see status updates, tool activity, proposed edits or approval requests before the final response. Read those updates as progress information, not as hidden reasoning. Approve a command or edit only when you understand its purpose and scope.

For your first editing task, keep the same pattern: name one small outcome, identify the allowed area, and ask for tests or another check. For example, ask it to update one validation message in a named file and run the focused test. Review the resulting diff before accepting it.

## Review the result

Copilot can be useful and still be wrong. Compare its answer or changes with the request, the relevant code and the evidence it reports.

- Check that it stayed within the requested files and behaviour
- Read the diff, especially generated or broadly formatted changes
- Run or inspect the relevant tests yourself when the change matters
- Ask a follow-up when the evidence is incomplete: "What did you check, and what remains uncertain?"

## What happens underneath

The short version is:

```text
Copilot experience = model + harness + context + tools
```

The model generates output. The harness connects it to your IDE and manages the work. Context is the information available for the current model call, and tools let the model request actions such as searching, reading, editing or running commands. [How Copilot works](How-Copilot-Works.md) explains this in full; [One request, many rounds](One-Request-Many-Rounds.md) shows why an Agent task can involve several internal cycles.

## Where to go next

- Learn the [mental model behind Copilot](How-Copilot-Works.md)
- Learn [how to work efficiently and manage cost](Working-Efficiently-and-Managing-Cost.md)
- Choose a shared [Copilot customisation](Copilot-Technologies.md) when a useful behaviour should be reused by a team

## Sources

- [GitHub Copilot quickstart for supported IDEs](https://docs.github.com/en/copilot/get-started/quickstart)
- [GitHub Copilot feature matrix](https://docs.github.com/en/copilot/reference/copilot-feature-matrix)
- [Get started with Copilot in VS Code](https://code.visualstudio.com/docs/copilot/getting-started)
- [Get started with Copilot in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/visual-studio-github-copilot-get-started?view=visualstudio)
- [Get started with Copilot in JetBrains](https://docs.github.com/en/copilot/get-started/getting-started-with-github-copilot?tool=jetbrains)
