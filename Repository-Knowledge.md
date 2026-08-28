# Repository knowledge for people and agents

_For developers and repository maintainers - Last reviewed 28 August 2026_

A small, task-driven knowledge map can help people and agents find the repository information that matters before they start exploring code. The aim is to reduce repeated orientation and avoid agent rounds spent ingesting and processing code merely to rediscover existing behaviour, boundaries and rules.

The agent should load only the knowledge relevant to the task, then verify it against the repository.

> **Hypothesis, not a saving claim:** creating and maintaining repository knowledge has a cost. Test whether a small routing structure reduces later searches, model rounds, mistaken assumptions, corrections and review time without reducing result quality.

[[_TOC_]]

## The 30-second explanation

Repository knowledge is not one large briefing that every agent should read. It is a navigable tree:

1. A tiny standing instruction tells the agent where the root index is
2. The root index classifies the current task by activity and application area
3. Child indexes route to a small number of relevant documents
4. Those documents provide expected behaviour, boundaries and rules before code inspection begins
5. The repository remains the evidence for how the system is actually implemented

![An observed Field Visits implementation comparing orientation without repository knowledge against a small pointer, root index and two focused guides](Media/poc-repository-knowledge-routing.svg =900x)

The diagram reports one matched pilot, not a general benchmark. A useful route can help the agent start closer to the relevant behaviour and rules; it does not remove the need to inspect implementation evidence, make the change and verify it.

### What happened in the first bounded pilot

The same Sonnet 4.5 model implemented a protected React and .NET Field Visits feature from matched repository states. The documented condition added one 298-character standing pointer, a root index and two focused guides.

| Observed measure | Without route | With route |
| --- | ---: | ---: |
| Successful reads before the first edit | 34 | 26 |
| Model requests before the first edit | 15 | 11 |
| Total model requests | 30 | 28 |
| Total input tokens | 1,095,614 | 1,055,217 |
| External acceptance checks | 24/24 | 24/24 |
| Observed billed AIC | 83.65 | 92.55 |
| Cache-normalised AIC estimate | 83.65 | 80.44 |

The routed result also added stronger API and front-end tests. Its observed bill was distorted by one 47,446-token request that unexpectedly received no cache read even though the system prompt, tools and conversation prefix were unchanged; caching resumed on the next request. Making both initial requests cold and treating that isolated miss as a normal prefix hit estimates 80.44 rather than 92.55 AIC. Pricing every input token as uncached gives the same direction, 3.6% lower for the routed condition. This is evidence from one pair, not a general saving guarantee.

The index is a router, not a manifest to ingest. A link tells the agent where to look when a question makes that document relevant. It does not mean that the whole documentation tree should become model input at the start of every request.

Repository knowledge is independently useful. [Spec-Driven Development](Spec-Driven-Development.md) can consume it, but SDD is neither a prerequisite nor the source of any benefit the knowledge map might produce.

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

Use one consistent documentation location that fits the repository. It might be `.docs/`, `docs/`, `.knowledge/` or a small set of existing root documents. The name is less important than discoverability, ownership and consistent links.

For a larger repository, an illustrative shape is:

```text
.docs/
  index.md
  architecture/
    index.md
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

1. Use the small standing instructions already supplied to the request
2. Read the root index and identify the activity as a code change and the area as back-end persistence
3. Follow the architecture route to learn the expected persistence behaviour and boundaries
4. Follow the rules route for implementation constraints specific to that area
5. Follow the process route for the required verification workflow
6. Inspect the relevant code, configuration and tests to confirm the documents still match reality

Unrelated front-end guidance, historical decisions and broad repository summaries remain unloaded unless the task creates a reason to read them.

The same idea can be applied to a cross-stack React feature. The route starts with two small knowledge reads, then expands into the connected code surfaces only after the task requires them:

![A protected React page task selecting front-end and domain knowledge before inspecting and changing route, navigation, permissions, API hook, schema, page, styles and tests](Media/repository-knowledge-frontend-change-surface.svg =900x)

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

A Markdown link is a direction to read the target, not a promise that every linked file is eagerly added to every request. Loading behaviour depends on the client and settings. In VS Code, inspect response references or customisation diagnostics when this matters.

## Maintain it without an AI documentation tax

The team should not need to spend AI credits updating a knowledge document after every ticket.

Update a focused document when a change materially alters behaviour, boundaries, commands or constraints that the document actually describes. Otherwise leave it alone. A manual commit-range review can identify likely impact without automatically rewriting anything.

| Trigger | Proportionate response |
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

## Test the hypothesis separately

Do not count the bootstrap alone as a success or failure. Its proposed benefit is amortised across later work.

Emerging studies of always-loaded `AGENTS.md` and similar context files report mixed results: some found higher inference cost without a measurable correctness gain, while another found lower output and elapsed time but did not evaluate correctness. Those treatments are not the indexed, on-demand pattern described here. The bounded pilot above reduced orientation measures and produced a modest cache-normalised cost estimate. Together, they reinforce the need to test task outcomes rather than the presence of documentation or one usage measure in isolation.

Compare representative tasks from the same repository state with and without the reviewed routes. Record first-pass correctness, acceptance-test results, exploratory searches, model rounds, tool failures, human corrections, review time, latency and total AI credits. Include the pilot creation and maintenance cost when judging longer-term value.

Control the model, repository commit, task, tools and success criteria where practical. Record cache reads per request, not only in aggregate, and show a cache-normalised estimate when an isolated miss would reverse the result. Counterbalance condition order when repeats are affordable. A lower token or tool-call count is useful only when the result still meets the quality bar.

> **Test separately:** evaluate [SDD and model routing](Spec-Driven-Development.md#evaluate-the-workflow) in different comparisons so that any repository-orientation benefit is not attributed to SDD.

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
