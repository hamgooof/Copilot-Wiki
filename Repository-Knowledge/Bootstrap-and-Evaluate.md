# Bootstrap and evaluate a repository-knowledge pilot

_For developers trialling the pattern - Last reviewed 27 August 2026_

This runbook creates a small task-driven knowledge router for one representative area. It deliberately avoids generating a repository encyclopaedia.

[[_TOC_]]

## Before starting

Choose:

- One representative application area, such as back-end persistence
- One normal activity, such as planning and implementing a bounded change
- Two or three later tasks that can test whether the routes reduce rediscovery
- A repository commit and success criteria that can be reused for matched comparisons

Record the AI credits and human review time spent creating the pilot. Those costs belong in the eventual evaluation.

## 1. Plan the routing structure

In VS Code, select the built-in **Plan** agent, deliberately choose an available reasoning-capable model, and paste:

```text
Plan a small repository-knowledge pilot for one representative task area in this repository.

The goal is to reduce later agent rounds spent reading code merely to rediscover expected behaviour, boundaries, rules and the normal verification path. The result must be a task-driven router, not a repository encyclopaedia and not a manifest that agents read in full.

Do not edit files. Inspect existing repository instructions, documentation and code only far enough to establish:
- one representative application area and one normal change activity for the pilot
- the questions an agent repeatedly has to answer before working in that area
- existing maintained documents, runbooks, ADRs, tests, schemas or configuration that should be linked rather than copied
- stable knowledge gaps that genuinely require a new focused document

Propose the smallest useful structure:
- a tiny standing pointer, plus canonical universal commands only if they are short and not already present
- a small root index that routes by activity and application area
- a child index only when the root would otherwise become difficult to scan
- the minimum focused documents needed for the selected pilot

For every proposed route or document, state:
- the task question it answers
- when an agent should read it
- its authoritative evidence
- why the information is stable enough to maintain
- what should remain in instructions, code, tests, analysers, existing team records or on-demand inspection instead
- the event that would require a future update

Choose repository-specific boundaries. Do not impose a generic architecture taxonomy. Record material uncertainty instead of filling gaps with assumptions.

Stop when the pilot can route the selected task area without documenting unrelated parts of the repository.
```

The prompt leaves the model unset so that the person running the trial can choose from the models available to their plan.

### Human review point

Before implementation, check:

- Is the pilot limited to one useful area and activity?
- Does the root route a task rather than enumerate every document?
- Are canonical commands in standing instructions or a linked runbook rather than duplicated?
- Are existing team records linked instead of copied?
- Does every proposed leaf contain stable information that changes a decision?
- Is there an explicit list of things that will not be documented?

Save the approved plan as a normal repository or workspace file. VS Code Plan session memory is cleared when the conversation ends.

## 2. Implement from a clean boundary

For the clearest pilot, open a fresh Agent chat and supply the saved plan as ordinary input. This avoids carrying the whole planning conversation and its tool traffic into implementation.

Use:

```text
Implement only the approved repository-knowledge pilot in <saved-plan-path>.

Create a task-driven routing structure for the approved application area and activity. Keep the root index quick to scan. Add child indexes only where the approved plan requires them, and write only the minimum focused documents needed by the pilot.

For every factual claim, verify the current repository evidence. Link to existing maintained documentation, ADRs, runbooks, tests, schemas and configuration instead of duplicating them. Treat implementation as evidence and report any conflict with existing prose.

Keep canonical universal build, test and lint commands in repository-wide instructions when concise; otherwise link to the maintained runbook. Do not repeat commands in several documents. Do not copy volatile ticket history or create an AI-only decision log.

Do not add the standing instruction pointer yet. First return:
- files created or changed
- the task routes now supported
- evidence checked for each focused document
- material uncertainty or suspected stale documentation
- commands or checks used to validate links and structure
```

Use bounded subagents only when an investigation is genuinely independent and their context isolation is useful. A small pilot may be clearer with one agent.

## 3. Review the routes before enabling them

Use a fresh reviewer or review subagent. Give it the approved plan and exact change range:

```text
Review the repository-knowledge pilot in <base>...<head> against the approved plan at <saved-plan-path>.

Do not edit files. Read the proposed root index, then behave like an agent receiving each of the representative tasks in the plan. Follow only the routes that the task selects and record which documents become necessary.

Verify every material claim against source, configuration, tests, schemas or other executable evidence. Check for:
- a root index that enumerates documents instead of routing tasks
- routes that cause unrelated material to be read
- missing expected behaviour, boundaries or area-specific rules
- duplicated build commands, ADRs, runbooks or ticket history
- volatile facts without a maintained source or owner
- deterministic requirements left only as prose
- contradictions with existing instructions or implementation
- links, anchors and ordering that will fail in the published wiki or repository

Report confirmed defects by severity, then unresolved questions. For each defect give the route or file, evidence, why it matters and the smallest correction. “Not established” is an acceptable conclusion.
```

Correct confirmed defects without expanding the pilot into unrelated areas.

## 4. Add the small entry point

Only after the review passes, add or refine the concise repository-wide pointer:

```markdown
Repository knowledge is indexed at [`.docs/index.md`](../.docs/index.md).
Before planning or changing an unfamiliar area, read the index, then only the routes relevant to the task.
Treat the repository as implementation evidence. If it contradicts a document, report the conflict and propose a focused update.
```

Add short canonical build, test and lint commands alongside it only when they apply broadly. Do not automatically include the contents of every linked document.

## 5. Evaluate later tasks

Run two or three representative tasks from the same repository state:

1. Baseline without the knowledge pointer or routes
2. Matched run with the reviewed pilot available

For both conditions, retain the raw conversation and telemetry where policy permits. Compare:

- First-pass correctness and acceptance-test results
- Code searches and files read for orientation
- Model rounds and tool failures before productive work begins
- Incorrect assumptions and human corrections
- Review time and elapsed time
- Input, output, cache and total AI-credit usage

Inspect raw logs rather than relying only on a harness summary. Confirm which documents and messages were actually supplied to each model request.

Include the planning, implementation, review and maintenance cost. A shorter run is not an improvement if quality falls.

## Focused maintenance review

Use this after a material change or for a chosen commit range. It reports impact without rewriting documentation automatically:

```text
Review repository-knowledge impact for <base>...<head>.

Read the root knowledge index, classify the change by activity and application area, then follow only the routes that might be affected. Read the changed implementation and its executable evidence.

Do not edit files. Report:
- affected route or focused document
- evidence that documented behaviour, boundaries, commands or constraints changed
- the smallest update needed
- material uncertainty

Do not propose an update for incidental implementation work that leaves the documented contract unchanged. Do not copy ticket history or duplicate an existing maintained source. If no update is needed, say why.
```

Prefer this manual signal before considering hooks or automatic semantic rewrites.

## Related guidance

- Return to [Repository knowledge for people and agents](../Repository-Knowledge.md)
- Check [Custom instructions](../Copilot-Technologies/Custom-instructions.md) before adding the standing pointer
- Keep the [SDD evaluation](../Spec-Driven-Development.md#evaluate-the-workflow) separate from this pilot
