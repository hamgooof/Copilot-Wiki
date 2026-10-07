# Repository knowledge for people and agents

> **[DEFERRED, not in 2.0]** This page is not published yet. Do not copy it to Confluence.

_For developers and repository maintainers - Last reviewed 4 October 2026_

Give the agent a short index that points each kind of task to the two or three documents it needs. It reads those, checks them against the code, and skips the rest. The aim is fewer rounds spent re-reading code to rediscover how an area is meant to work.

This costs effort to build and maintain, so try it on one area first and see whether it helps.

[[_TOC_]]

## The 30-second explanation

Repository knowledge is not one large briefing that every agent should read. It is a navigable tree:

1. A tiny standing instruction tells the agent where the root index is
2. The root index classifies the current task by activity and application area
3. Child indexes route to a small number of relevant documents
4. Those documents provide expected behaviour, boundaries and rules before code inspection begins
5. The repository remains the evidence for how the system is actually implemented

![A small standing instruction points to the repository knowledge index, which routes the task to two relevant documents while an unrelated guide stays unloaded](Media/repository-knowledge-selective-context.svg =900x)

In one small internal trial the agent read fewer files before its first edit when the routes were present, and passed the same acceptance checks. The cost difference was too small and the sample too small to call a saving.

The index is the building directory in the lobby. It tells you which floor to go to, not what is in every office. The agent still has to walk in and look, by verifying against the code.

You do not need [Spec-Driven Development](Spec-Driven-Development.md) to use this, though SDD can use it.

## Put each kind of guidance in the right place

| Information | Best home | Why |
| --- | --- | --- |
| Root pointer, canonical build/test/lint commands and genuinely universal boundaries | Repository-wide [custom instructions](Copilot-Technologies/Custom-instructions.md) | Short, high-use guidance can apply automatically |
| Area-specific coding rules | Path-specific instructions or an indexed rules document | Load or apply them only where they matter |
| Routes from a task or area to useful knowledge | Root and child indexes | Helps the agent decide what to read |
| Stable subsystem behaviour, boundaries, interfaces and easy-to-miss constraints | Focused knowledge documents | Avoids rediscovering these facts from many files |
| Detailed command variants and troubleshooting | Existing developer runbook, linked from the index | Keeps standing instructions concise |
| Enforceable requirements | Tests, analysers, schemas or policy | Deterministic checks are stronger than prose reminders |
| Rationale and history | Existing ADRs, work items or other maintained team records | Avoids creating a second, AI-only history that quickly drifts |

Do not copy information merely to make the knowledge tree look complete. Prefer links to maintained sources already used by the team.

## Design a routing tree

Use one consistent documentation location that fits the repository. It might be `.docs/`, `docs/`, `.knowledge/` or a small set of existing root documents. The name matters less than everyone knowing where it is.

For a larger repository, an illustrative shape is:

```text
.docs/
  index.md
  architecture/
    index.md
    frontend.md
    persistence.md
  rules/
    index.md
    angular.md
    dotnet-persistence.md
  process/
    index.md
    change-and-verify.md
  evidence/
    index.md
```

This is an example, not a required taxonomy. Add a child index only when it makes the root easier to scan. A small repository may need one index and two or three existing documents.

### Keep the root index small

The root should answer: **where should I look for this task?** It can route in more than one dimension.

```markdown
# Repository knowledge

## By activity

| When you are... | Read |
| --- | --- |
| Planning or changing code | [Change and verification process](process/change-and-verify.md) |
| Investigating expected subsystem behaviour | [Architecture index](architecture/index.md) |
| Diagnosing a known production symptom | [Evidence index](evidence/index.md) |

## By application area

| Target area | Read |
| --- | --- |
| Angular front end | [Front-end architecture](architecture/frontend.md) and [Angular rules](rules/angular.md) |
| Back-end persistence | [Persistence architecture](architecture/persistence.md) and [.NET persistence rules](rules/dotnet-persistence.md) |
```

The root does not need to list every leaf document. It can link to a child index when an area has several focused resources.

### From the agent's point of view

For a task such as **change the persistence retry behaviour**, a good route is:

1. Use the small standing instructions already included in the model input
2. Read the root index and identify the activity as a code change and the area as back-end persistence
3. Follow the architecture route to learn the expected persistence behaviour and boundaries
4. Follow the rules route for implementation constraints specific to that area
5. Follow the process route for the required verification workflow
6. Inspect the relevant code, configuration and tests to confirm the documents still match reality

Unrelated front-end guidance, historical decisions and broad repository summaries remain unloaded unless the task creates a reason to read them.

## What belongs in focused documents

Prioritise stable information that changes a decision or narrows investigation:

- The purpose and boundary of the documented subsystem
- Expected behaviour and important business invariants
- How major parts connect at a stable architectural level
- External interface and compatibility constraints
- Established patterns that are difficult to infer from one file
- Generated areas, ownership boundaries and places that should not be edited manually
- Pointers to the authoritative configuration, tests, schemas or operational evidence
- Material uncertainty where the repository does not establish an answer

Leave these elsewhere:

- Exhaustive file, class or symbol inventories that search can recreate
- Copied source code, logs or long command output
- Rules already enforced by formatters, analysers or tests
- Ticket-by-ticket history and volatile project status
- Duplicated ADRs, work items or runbooks
- Brittle source-line references
- A fixed architecture taxonomy copied from another repository
- Guesses presented as expected behaviour

If a fact changes frequently and nobody owns its documentation, it is a poor candidate for durable repository knowledge. Link to its maintained source or let the agent inspect it when needed.

## Keep the entry point small

For `.github/copilot-instructions.md`, the root pointer can remain concise:

```markdown
Repository knowledge is indexed at [`.docs/index.md`](../.docs/index.md).
Before planning or changing an unfamiliar area, read the index, then only the routes relevant to the task.
Treat the repository as implementation evidence. If it contradicts a document, report the conflict and propose a focused update.
```

Include canonical build, test and lint commands alongside this pointer when they are short and apply broadly. Link to a runbook when the workflow has many variants or troubleshooting steps. Do not duplicate the same commands in several knowledge documents.

To check what the agent actually read, open the response references in VS Code.

## Maintain it without an AI documentation tax

The team should not need to spend AI credits updating a knowledge document after every ticket.

Update a document only when a change breaks something it says. Otherwise leave it alone. The [maintenance review prompt](Repository-Knowledge/Bootstrap-and-Evaluate.md#focused-maintenance-review) will tell you which documents a set of commits affects.

| Trigger | What to do |
| --- | --- |
| A documented contract or boundary changes | Update the affected leaf and its route if necessary |
| A new subsystem or recurring task cannot be routed | Add the smallest useful route or document |
| Existing ADR, runbook or team documentation changes | Keep linking to that source rather than copying it |
| A formatter, analyser or test can enforce the rule | Move enforcement out of prose |
| A document repeatedly conflicts with implementation | Assign ownership, narrow its scope or remove it |

Periodic review should concentrate on the root routes, high-use commands and known volatile documents. It does not need to re-document the repository.

## Try it as a bounded pilot

Start with one representative task area and its normal change workflow. Do not bootstrap an encyclopaedia.

[Bootstrap and evaluate a repository-knowledge pilot](Repository-Knowledge/Bootstrap-and-Evaluate.md) contains copy-ready planning, implementation, review and maintenance prompts for the trial.

## Did it help?

After the pilot, try two or three ordinary tasks in that area. Did Copilot find the right files sooner and need fewer corrections? If not, remove the routes. Studies of always-loaded `AGENTS.md` files have found mixed results, so judge by your own tasks.

## What to read next

- Run the bounded [repository-knowledge pilot](Repository-Knowledge/Bootstrap-and-Evaluate.md)
- Use [Custom instructions](Copilot-Technologies/Custom-instructions.md) for the small always-on pointer and universal commands
- Learn how [Spec-Driven Development](Spec-Driven-Development.md) can use durable knowledge without owning it
- Review the cost foundations in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)

## Sources

- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Improving agent quality to optimize AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)

### Community and emerging evidence

- [Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?](https://arxiv.org/abs/2602.11988)
- [Do Context Files Help Coding Agents?](https://arxiv.org/abs/2607.27250)
- [On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents](https://assets.empirical-software.engineering/pdf/jaws26-agents.md-efficiency.pdf)
- [Aider repository map](https://aider.chat/docs/repomap.html)
