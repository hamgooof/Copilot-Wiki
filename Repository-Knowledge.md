# Repository knowledge for people and agents

_For developers and repository maintainers - Last reviewed 27 August 2026_

A concise repository index and a few focused documents can give people and agents a reusable map of a codebase. The aim is to reduce repeated orientation and mistaken assumptions without loading the whole repository into every request.

> **Hypothesis, not a saving claim:** repository knowledge has an upfront creation and maintenance cost. Test whether it reduces later searches, rounds, corrections and review time across ordinary planning, implementation, debugging and review tasks.

[[_TOC_]]

## The 30-second explanation

Repository knowledge records the facts and decisions that are difficult or wasteful to rediscover from code alone. It might explain major boundaries, supported commands, important data flows, external contracts and the reasons behind non-obvious choices.

The documents are stored knowledge. They become model context only when the Copilot harness reads and supplies them to a model. A useful structure therefore has two layers:

1. A very small index that helps an agent decide where to look
2. Focused documents that are read only when they are relevant to the task

![A small instruction pointer leading through a repository knowledge index to only the documents relevant to the current task](Media/repository-knowledge-selective-context.svg =820x)

This resource is independently useful. [Spec-Driven Development](Spec-Driven-Development.md) can consume it, but SDD is neither a prerequisite nor the source of any saving the knowledge base might produce.

## What belongs in it

Prioritise information that changes a decision:

- The repository's purpose and main boundaries
- How major parts interact at a stable, architectural level
- The supported build, test, lint and local-run commands
- Important business rules and external interface constraints
- Established patterns that are easy to miss from one file
- Decisions, rationale and material alternatives
- Known traps, generated areas and places that should not be edited manually

Leave these elsewhere:

- Exhaustive file or symbol inventories that search can recreate
- Copied source code or long command output
- Rules already enforced by formatters, analysers or tests
- Brittle source-line references
- A fixed taxonomy copied from an unrelated repository
- Guesses presented as architecture

Use stable project, directory, file, module, symbol and command references where they help. Record an uncertainty when the repository does not establish an answer.

## Choose a location that fits the repository

The location is less important than a clear index and consistent links.

| Option | When it fits |
| --- | --- |
| `.docs/` | A reasonable preference when repository knowledge should look like versioned repository infrastructure |
| `docs/` | A conventional choice when developer or product documentation already lives there |
| Concise root files | Suitable for a small repository that needs only files such as `ARCHITECTURE.md` and `CONTRIBUTING.md` |

`.docs` is not a Copilot requirement. Do not move established documentation merely to match an example.

A small starting shape might be:

```text
.docs/
  index.md
  architecture-overview.md
  development-and-validation.md
  <repository-specific documents chosen during bootstrap>
```

The final documents should follow the repository's real boundaries. A service-oriented system, library, data pipeline and monorepo will need different divisions.

## Use the index for smart lookup

An index should answer two questions quickly: what knowledge exists, and when should it be read?

```markdown
# Repository knowledge

| Document | Read when |
| --- | --- |
| [Architecture overview](architecture-overview.md) | Identifying system boundaries or planning cross-cutting work |
| [Development and validation](development-and-validation.md) | Building, testing or diagnosing a local change |
| [<Repository-specific area>](<area>.md) | Working in or across that area |
```

Keep the always-applied instruction small. For `.github/copilot-instructions.md`, a pointer can be enough:

```markdown
Repository knowledge is indexed at [`.docs/index.md`](../.docs/index.md).
Before planning or changing an unfamiliar area, read the index, then only the linked documents relevant to the task.
If implementation evidence contradicts a document, report the conflict and propose a focused update.
```

A Markdown link is a direction to read the target, not a promise that every linked file is eagerly added to every request. Loading behaviour depends on the client and settings. In VS Code, inspect the response's references or customisation diagnostics when this matters. See [Custom instructions](Copilot-Technologies/Custom-instructions.md) for instruction locations and scope.

## Bootstrap it in bounded phases

The first pass should decide how to investigate the repository, not attempt to document everything.

### 1. Plan the investigations

For a first trial in VS Code, select the built-in **Plan** agent, deliberately choose the planning model, and paste this request:

```text
Plan a concise repository-knowledge base for this repository.

Inspect only far enough to identify the repository's real high-level boundaries, existing documentation and the supported development workflow. Do not document each subsystem and do not edit files in this planning phase.

Propose the smallest useful index and a set of coherent, bounded documentation investigations. For each investigation record its scope, questions, dependencies, expected artefact, validation, state and material uncertainties. Choose repository-specific boundaries rather than imposing a framework or architecture taxonomy. Treat `.docs/` as a preference, not a requirement.

Stop reconnaissance when the investigations can be bounded without inventing boundaries or relying on unexplored architectural assumptions.
```

The prompt deliberately leaves the model unset. A person can select a reasoning-capable model for this difficult decomposition without pinning that choice into a reusable file.

> **Human review point:** check the proposed boundaries, omissions, document sizes and uncertainties before any worker writes repository knowledge. Save the approved plan as a normal repository or workspace file if it must survive the planning conversation.

VS Code's built-in Plan agent also keeps a plan in session memory, but that memory is cleared when the conversation ends. Use **Open in Editor** and save the plan when a durable artefact or fresh session is wanted.

### 2. Run the bounded investigations

After approving the plan, either select **Start Implementation** or begin a fresh Agent session from the saved plan. Use this short instruction:

```text
Implement the approved repository-knowledge plan. Keep the coordination shallow.
Delegate independent investigations to bounded subagents where that improves isolation or parallelism. Each worker must verify its claims against the repository, write only its assigned concise artefact, and return material uncertainty. Do not force one worker's taxonomy onto another area.
```

The built-in Plan handoff carries the plan and conversation context. A custom-agent handoff is documented as moving with relevant context. A fresh session does not inherit the planning conversation; it must read the saved plan as ordinary input. [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) explains the transition and model choices.

### 3. Review before relying on it

Use a fresh reviewer or review subagent to check:

- Each claim against source, configuration or executable evidence
- Missing boundaries, commands or cross-links
- Duplication and excessive implementation detail
- Contradictions with existing documentation or instructions
- Accidental framework-specific assumptions
- Whether the index sends a reader to the right document

Correct confirmed issues without turning the reviewer into another documentation generator.

### 4. Add the small entry point

Only after review, add or refine the concise pointer in repository instructions. This prevents an unverified draft from becoming standing context for later work.

## Maintain it without constant rewriting

No single freshness mechanism fits every team. Start with a manual baseline.

| Trigger | Useful action | Trade-off |
| --- | --- | --- |
| End of a material task | Ask whether changed behaviour, commands or boundaries invalidate a linked document | Timely, but depends on the task worker noticing the impact |
| Commit range or pre-review | Run a focused documentation-impact review | Repeatable and easy to inspect |
| Periodic review | Check the index, commands, ownership and known volatile areas | Catches drift outside feature work, but consumes review time |
| Deterministic check | Check links, expected index entries or generated-file rules | Cheap and reliable for structure, not meaning |
| Hook | Signal a possible review after a relevant lifecycle event | VS Code Preview feature that runs local commands and can create noise or risk |

A copy-ready manual review request is:

```text
Review the documentation impact of changes in <base>...<head>.
Read the repository-knowledge index, the changed files and only the linked documents that might be affected.
Do not edit files. Report: affected document, evidence from the change, the focused update needed, and material uncertainty. If no update is needed, say why. Ignore incidental implementation changes that do not alter documented behaviour, commands, boundaries or decisions.
```

Use a hook only after the manual review produces a stable signal that code can detect. Prefer a reminder or deterministic check over automatic semantic rewrites. Hooks are Preview in VS Code, can be disabled by an organisation, and execute commands with the editor's permissions.

## Test the repository-knowledge hypothesis separately

Do not count the bootstrap alone as a success or failure. Its proposed benefit is amortised across later work.

Emerging studies of always-loaded `AGENTS.md` and similar context files report mixed results: some found higher inference cost without a measurable correctness gain, while another found lower output and elapsed time but did not evaluate correctness. Those treatments are not the indexed, on-demand pattern on this page. They reinforce the need to test useful task outcomes, not the presence of documentation or one usage measure in isolation.

1. Choose several representative planning, implementation, debugging and review tasks
2. Run matched baselines without the knowledge index
3. Repeat from the same repository state with the reviewed index available
4. Record first-pass correctness, acceptance-test results, orientation searches, model rounds, tool failures, human corrections, review time, latency and total AI credits
5. Include the bootstrap and maintenance cost when judging longer-term value

Control the model, repository commit, task, tools and success criteria where practical. A lower token or tool-call count is useful only when the result still meets the quality bar.

> **To test separately:** compare direct work with and without repository knowledge. Evaluate [SDD and model routing](Spec-Driven-Development.md#evaluate-the-workflow) in different comparisons so that any repository-orientation benefit is not attributed to SDD.

## What to read next

- Use [Custom instructions](Copilot-Technologies/Custom-instructions.md) for the small always-on pointer
- Learn how [Spec-Driven Development](Spec-Driven-Development.md) uses durable intent and staged work
- Review the cost foundations in [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)

## Sources

- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/agents/best-practices)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Agent hooks in VS Code](https://code.visualstudio.com/docs/agent-customization/hooks)
- [Improving agent quality to optimize AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)

### Community and emerging evidence

- [Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?](https://arxiv.org/abs/2602.11988)
- [Do Context Files Help Coding Agents?](https://arxiv.org/abs/2607.27250)
- [On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents](https://assets.empirical-software.engineering/pdf/jaws26-agents.md-efficiency.pdf)
- [Aider repository map](https://aider.chat/docs/repomap.html)
