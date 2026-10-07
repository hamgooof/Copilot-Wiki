# Spec-Driven Development

> **[DEFERRED, not in 2.0]** This page is not published yet. Do not copy it to Confluence.

_For developers planning and delivering non-trivial changes - Last reviewed 4 October 2026_

**Spec-Driven Development (SDD)** puts durable intent before implementation. A specification defines what should be built, then planning and task stages refine how it will be delivered before code is changed.

**If your team writes tickets with requirements and acceptance criteria, you already have the specification.** What SDD adds is a reviewed plan and small tasks *before* the agent edits code, and a review of the result against those acceptance criteria.

SDD can improve alignment, staged review, resumability and the reviewability of implementation. It also adds work and model calls. Treat cost reduction as something to measure, not as the definition or promised outcome of the process.

[[_TOC_]]

## What SDD means

GitHub Spec Kit describes SDD as a structured, intent-driven process in which the specification defines the **what** before the technical plan defines the **how**. Its recognised core flow is:

```text
Spec -> Plan -> Tasks -> Implement
```

Each stage can produce a Markdown artefact that feeds the next. The artefacts make intent and decisions visible to people as well as agents.

| Artefact or stage | Main question | Useful contents | Review opportunity |
| --- | --- | --- | --- |
| Specification | What problem and behaviour are required? | Outcomes, scenarios, acceptance criteria, constraints and out-of-scope work | Confirm intent without committing to an implementation |
| Plan | How should this repository deliver it? | Affected areas, approach, dependencies, risks, validation and open questions | Challenge architecture and assumptions before edits |
| Tasks | In what bounded order should it be built? | Dependency-aware pieces, state, completion evidence and safe parallel work | Check coverage and sequencing |
| Implementation | Does the code satisfy the agreed intent? | Focused changes, tests and validation evidence | Review the diff against the specification and plan |

Current Spec Kit also provides optional quality stages such as a constitution, clarification, requirements checklists, cross-artefact analysis and post-implementation convergence. The four-stage flow remains the useful foundation; the extra stages should be added when the ambiguity or risk justifies them.

Spec Kit's workflow engine demonstrates explicit human gates after specification and planning. A person running the commands manually still needs to choose and honour their own review points.

> **Human review point:** approve the required behaviour before technical planning, then approve the plan before implementation. For high-risk work, review the task split and final evidence as separate decisions.

## When the overhead is worthwhile

SDD is most useful when:

- Requirements contain ambiguity or competing interpretations
- A change crosses important repository boundaries
- Architecture, security, data or compatibility decisions need review
- Several people or agents will implement different pieces
- Work must pause and resume without losing agreed intent
- The final diff needs a clear basis for acceptance

For an obvious one-file correction with a focused test, a full specification, plan and task set may cost more than it adds. Scale the artefacts to the risk. For most tickets here, start with the lighter variation below; Spec Kit is optional.

## A lighter human-directed variation

The following is one SDD-inspired variation, not the official or only form of SDD:

1. Paste the ticket's requirement and acceptance criteria into a Plan session, or save them as `docs/specs/<TICKET-KEY>.md` so later sessions can read them
2. Let a capable planning model investigate the repository at architectural level
3. Save a concise plan and divide it into bounded, dependency-aware tasks
4. Have a person review the intent and plan
5. Let fresh or isolated workers implement one bounded piece at a time
6. Record task state, evidence and only the discoveries later workers need
7. Run an independent review against the specification and plan

The planner should stop broad reconnaissance once it can identify affected areas, existing patterns, risks and validation without inventing boundaries. It should not turn the plan into a repository findings dump or a copy of the future implementation.

[Repository knowledge](Repository-Knowledge.md) can help the planner remain at this level. It is also useful outside SDD and must be evaluated separately.

## Move from planning to implementation in VS Code

Several VS Code features can support the transition. They are not interchangeable.

| Option | What it provides | Context boundary | Durability |
| --- | --- | --- | --- |
| Built-in **Plan** | Read-only research, clarification and an implementation plan | **Start Implementation** carries the plan and conversation context to the chosen implementation agent | The automatic `/memories/session/plan.md` is cleared when the conversation ends |
| Custom planning agent | Reusable planning role, tool restrictions, optional model and handoff buttons | Carries the whole visible conversation, so it does not shrink context | Chat output is not durable unless it is saved |
| Saved plan in a new chat | Explicit, reviewable input for a new worker | The new chat starts without the planning conversation and reads the plan as ordinary context | Durable when stored in the repository, workspace or tracked issue |
| Subagent | Isolated worker for one delegated question or task | Receives a bounded brief, not the parent's full conversation, and returns a result | Its useful result must be captured by the parent or in a file |

![The ticket's requirement and acceptance criteria, plan, tasks and build separated by human review gates, with the result checked against the acceptance criteria](Media/sdd-reviewed-flow.svg =840x)

Use **Open in Editor** from the built-in Plan agent when the plan needs to survive the session. In a new Agent chat, reference the saved plan and ask it to implement only the next approved task.

> **Fresh-context alternative:** use a saved plan and a new chat when the planning discussion would otherwise dominate implementation context. This removes conversation history, but the new worker still pays the ordinary input cost of reading the plan and any relevant repository knowledge.

## Select models by phase

Model routing by workflow phase is an optional agent technique, not a new development methodology.

| Phase | Sensible starting point | Why |
| --- | --- | --- |
| Ambiguous specification or planning | A reasoning-capable model selected deliberately | Architectural choices and unresolved intent need judgement |
| Bounded implementation from an approved plan | A balanced or efficient model that passes the task's quality bar | The problem and evidence should already be narrower |
| Independent review | A model capable of challenging assumptions, in a fresh context where practical | Review should not simply repeat the implementer's rationale |

For the built-in Plan flow, VS Code currently provides `chat.planAgent.defaultModel` and `github.copilot.chat.implementAgent.model`. A team can leave the planning default unset and choose it from the model picker, while setting a reusable implementation default.

For custom agents, leave a planning agent's `model` unset when the person should choose it. Configure an implementation agent with one model or a prioritised fallback list. The detailed, current YAML and precedence rules are in [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md#select-models-for-each-phase).

A stronger planner and narrower workers may improve quality because the planner resolves ambiguity once and each worker has a smaller decision surface. They can also cost more because planning, handoffs, saved-plan reads, subagents, review and correction all add model work. Compare total results rather than the advertised tier of one phase.

## Understand what the stages cost

[One turn, many rounds](One-Turn-Many-Rounds.md) explains the model-and-tool loop. The usual rules apply: see [Count model work around tools and files](Working-Efficiently-and-Managing-Cost.md#count-model-work-around-tools-and-files). A saved plan is ordinary input when read; it is not cheaper because an agent wrote it.

Prompt caching is separate and depends on a sufficiently matching model-call prefix, model and current Copilot route. Switching models creates a new model-specific cache boundary. Cache writes, cached input and fresh input have different current prices for some models.

See [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md) and [Tokens and context windows](Tokens-and-Context-Windows.md) for the billing and context foundations.

## Use reviewable handoffs

A VS Code custom-agent handoff can provide a visible transition button. Keep `send: false` in a human-directed flow so the next prompt is pre-filled but not submitted. See [Handoffs](Copilot-Technologies/Custom-agents-and-subagents.md#handoffs) for the current YAML and field meanings.

The developer can inspect the plan, change the next prompt or choose the saved-plan alternative before work starts. Setting `send: true` removes that manual pause.

> **Human review point:** before planning becomes implementation, confirm scope, open questions, rollback or compatibility risks and the evidence required for completion.

After implementation, use a fresh Reviewer chat or an isolated review subagent. Give it the specification, plan, changed files and validation evidence. Ask for supported findings rather than edits. Let a person decide which findings become corrective tasks.

An AI reviewer is useful for spotting gaps, but it is not a correctness check. In internal trials, blind AI review scores did not track whether the code actually worked. Keep running the acceptance criteria as the gate.

## Decompose large features cautiously

Start with the least elaborate split that makes the work bounded:

1. Implement selected task identifiers or one phase
2. Delegate genuinely independent tasks where subagents are supported
3. Combine phase scoping with shallow delegation
4. Only when one phase is still too large, create a roadmap of independent sub-specifications

Spec Kit's large-feature guidance treats each sub-specification as its own complete feature with a specification, plan and tasks. The roadmap records dependencies and state. This is safer than one enormous specification or a deep agent tree, but it adds coordination and should be reserved for work that needs it.

## Cross-IDE limits

The handoff schema, session memory path, model settings and subagent precedence on this page are VS Code features. Do not copy them into Visual Studio or JetBrains configuration.

Current Visual Studio versions have their own Plan experience and save plans differently. Custom-agent fields and supported transitions also differ. GitHub currently marks custom agents and subagents in JetBrains as Preview, while hooks and subagents are not currently documented as supported in Visual Studio. Check the [current support table](Copilot-Technologies.md#current-ide-support) and client documentation before standardising a workflow.

Without a supported Plan or handoff control, use a read-only planning request, save the reviewed plan explicitly, then begin a separate implementation chat.

## What to read next

- Build [repository knowledge for people and agents](Repository-Knowledge.md)
- Configure [custom-agent handoffs, model selection and subagents](Copilot-Technologies/Custom-agents-and-subagents.md)
- Review [working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)

## Sources

- [What is Spec-Driven Development?](https://github.github.com/spec-kit/concepts/sdd.html)
- [GitHub Spec Kit](https://github.github.com/spec-kit/)
- [Spec Kit agentic SDD reference](https://github.com/github/spec-kit/blob/main/docs/reference/agentic-sdd.md)
- [Spec Kit workflows and review gates](https://github.github.com/spec-kit/reference/workflows.html)
- [Spec Kit guidance for complex features](https://github.github.com/spec-kit/concepts/complex-features.html)
- [Spec Kit spec-of-specs guidance](https://github.github.com/spec-kit/concepts/spec-of-specs.html)
- [Planning with agents in VS Code](https://code.visualstudio.com/docs/agents/run/planning)
- [Set up a context engineering flow in VS Code](https://code.visualstudio.com/docs/agents/guides/context-engineering-guide)
- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents)
- [Improving agent quality to optimize AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)
- [Models and pricing for GitHub Copilot](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- [Usage-based billing for organisations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Custom agents in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-specialized-agents?view=visualstudio)
