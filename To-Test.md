# To Test: Copilot behaviour and usage experiments

| Page status | Audience | Last reviewed |
| --- | --- | --- |
| Initial cross-wiki test backlog | Copilot platform owners and experimenters | 11 August 2026 |

This page turns uncertain or surface-specific behaviour into reproducible tests. Do not publish a numeric claim from one run. Repeat tests, record client and extension versions, and separate model variability from product behaviour.

Terminology in this page:

- A **test run** is one complete execution of an experiment
- A **user turn** runs from one submitted message to its final response
- An **agent round** is one internal model-and-tool iteration inside that turn

[[_TOC_]]

## Test harness requirements

Record for every run:

- Date, Copilot client/surface and version
- IDE and Copilot extension version where applicable
- Account plan and relevant organisation policies
- Selected parent and child models
- Repository commit and configuration files
- Enabled instruction, skill, agent and tool settings
- Prompt, full observable transcript and produced files
- Context/token view, per-model usage, AI credits and latency where exposed
- Whether prompt caching or automatic compaction occurred
- At least three repeats plus a control run

Use a synthetic repository with no secrets. Give each injected artefact a unique canary string so its presence can be tested without relying on stylistic interpretation.

Scope each conclusion to the tested IDE and version. Prioritise VS Code, Visual Studio and JetBrains for workplace adoption tests.

## T01 Always on instruction cost

**Question:** How much context and usage does a repository instruction file add across successive user turns and agent rounds?

**Method:** run the same scripted conversation with no instructions, a 100-token file, a 1,000-token file and a 5,000-token file. Use unique canaries and record context and usage after each user turn; capture individual agent rounds where the surface exposes them.

**Measure:** initial context, incremental tokens, cache hits, credits, latency, compaction point and answer quality.

**Surfaces:** VS Code, Visual Studio and JetBrains where the feature is supported.

## T02 Instruction discovery and precedence

**Question:** Which instruction files are loaded, merged or given precedence?

**Method:** put conflicting, uniquely identifiable rules in personal instructions, `.github/copilot-instructions.md`, path-specific instructions, root `AGENTS.md` and nested `AGENTS.md`. In VS Code, repeat with the experimental nested-`AGENTS.md` setting on and off. Also repeat from different working directories and target paths.

**Measure:** References list, `/instructions` or context display, observable compliance and ordering.

## T03 Uninvoked skill overhead

**Question:** What context cost is caused by installed skills that are not selected?

**Method:** compare 0, 10, 100 and 500 minimal skills with unique names/descriptions while issuing an unrelated fixed prompt.

**Measure:** context size, request tokens, latency, routing accuracy and credits.

## T04 Skill activation and persistence

**Question:** When is `SKILL.md` injected, and does its content remain available in later agent rounds, later user turns or after compaction?

**Method:** invoke a canary skill, refer indirectly to its rule over several user turns, change tasks, manually compact and test again.

**Measure:** context view, correct recall, false persistence and usage per user turn and observable agent round.

## T05 Supporting skill resources

**Question:** Are unreferenced resource files loaded, indexed only, or ignored until read?

**Method:** place separate canaries in `SKILL.md`, a linked reference, an unlinked reference and a script. Invoke the skill without naming the resources.

**Measure:** which canaries appear in context or output and which file/tool reads occur.

## T06 Selected custom agent context

**Question:** What is preserved when a person switches the active custom agent in the same chat?

**Method:** establish facts, attachments and decisions; switch agents; query each category; switch back.

**Measure:** conversation recall, attachments, profile instructions, tool changes, context size and model changes.

## T07 Parent to subagent context transfer

**Question:** What does a new subagent receive from the parent?

**Method:** place separate canaries in early chat history, latest prompt, repository instructions, an active skill, an attached file and an uncommitted file. Delegate without repeating them.

**Measure:** child-visible canaries, file reads, child context size and failures caused by missing constraints.

## T08 Subagent to parent return

**Question:** Does the parent receive a summary, full transcript, artefacts or selected results?

**Method:** ask a child to generate a large structured result with distinct items and a file artefact, then query the parent about details omitted from the visible summary.

**Measure:** detail retained, context added to parent, files available and audit trace.

## T09 Nested subagents

**Question:** How are instructions, models, permissions and context propagated through multiple levels?

**Method:** parent delegates to child A, which delegates to child B. Use separate canaries and tool permissions at each level.

**Measure:** effective model/toolset, context isolation, result flow, depth limits, credits and failure reporting.

## T10 Parent and child model routing

**Question:** Does an explicit child model or custom-agent model override the parent as documented?

**Method:** run a matrix of fixed parent model, `Auto`, agent-profile child model and explicitly requested child model. Include the “expensive parent, cheaper child” pattern.

**Measure:** actual per-model usage, fallbacks, quality, latency, retries and total credits.

## T11 Model change within a chat

**Question:** Is full useful context maintained when the chat model changes, and what happens when the new context window is smaller?

**Method:** build a long conversation with canaries at different depths, switch models, regenerate and continue until compaction.

**Measure:** recall, context denominator, compaction, cache changes and output consistency.

## T14 Quality impact of bloated customisation

**Question:** Does more instruction content reduce task quality?

**Method:** create a labelled evaluation set and compare concise instructions against an equivalent bloated file containing relevant, irrelevant and conflicting guidance.

**Measure:** task success, instruction compliance, hallucinations, tool errors, latency, credits and evaluator score.

## T15 Skill versus agent experiment

**Question:** For the same workflow, when does a skill outperform a custom agent?

**Method:** implement the same review workflow as always-on instructions, a skill and a custom agent. Keep task inputs and model fixed.

**Measure:** routing reliability, task quality, context size, credits, latency, tool safety and maintainability.

## T16 Azure DevOps SVG rendering

**Question:** Does the target Azure DevOps code-wiki renderer display repository-hosted SVG diagrams correctly?

**Observed 8 August 2026:** Azure DevOps displayed basic SVG paths and markers, but content associated with groups carrying drop-shadow filters was missing. Images also expanded to an unhelpful full-column size when no explicit Markdown width was supplied. The current revision removes filters and requests a 900px display width.

**Method:** after publishing the revised SVGs, open the context, turn/round and context-growth diagrams in both light and dark themes. Check that cards, labels, model panels and stacked context blocks are present; then check browser zoom and mobile-width layout. Repeat on the actual Azure DevOps Server version rather than assuming Azure DevOps Services behaviour.

**Measure:** image displayed, text readable, alt text available when blocked, links resolved and no active-content warning.

**Fallback:** retain SVG as the editable source, render a PNG copy and update the Markdown image targets if the server blocks SVG.

**Support status:** SVG is not listed among the image formats in Microsoft's Azure DevOps Markdown guidance. Treat current rendering as undocumented behaviour and repeat this test after server upgrades. Click every table-of-contents link during the same check.

## T17 Provider transport effective context and cache views

**Question:** For each available model/provider integration, what effective context is processed, what does Cache Explorer display, and what can actually be established about transport or provider-side state?

**Method:** run the same multi-round conversation with available Anthropic, OpenAI and other model families. Record the Cache Explorer prompt signature, prefix diff, cache-hit percentage, total input usage and any approved diagnostic traces. Do not infer a literal wire payload from a UI diff. Record model, client and extension versions and the exact observation date.

**Measure:** effective input size, cached versus uncached input, visible request components, first cache divergence, latency and whether the evidence distinguishes full-message transmission from provider-side state references.

**Classification:** document cache TTLs and transport observations as provider/model/client-specific findings. If available telemetry cannot distinguish two implementations, mark the result **Inconclusive** rather than selecting the more familiar API model.

## Suggested first automated suite

Prioritise T01, T03, T04, T07, T08, T10 and T14. Together they test the claims most likely to change team behaviour: instruction bloat, just-in-time skills, isolated subagents, model routing and real quality/usage impact.

## Evidence standard

Classify a finding as:

- **Confirmed:** explicitly documented and reproduced on the named version/surface
- **Observed:** reproduced consistently but not guaranteed by documentation
- **Inconclusive:** results vary or telemetry is insufficient
- **Changed:** current behaviour contradicts an older observation; retain both dates and versions

Publish findings with scope. Prefer “VS Code version X with model Z did X in N of N runs” over a claim about Copilot as a whole.

## Sources

- [The coding harness behind GitHub Copilot in VS Code: turns and rounds](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)
- [The Copilot SDK agent loop and SDK-specific turn events](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/agent-loop)
- [Diagnose prompt caching with the VS Code Cache Explorer](https://code.visualstudio.com/docs/agents/agent-troubleshooting/cache-explorer)
- [VS Code February 2026 release notes: context compaction](https://code.visualstudio.com/updates/v1_110)
- [Monitoring GitHub AI Credits usage](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage)
- [Markdown and image support in Azure DevOps wikis](https://learn.microsoft.com/en-us/azure/devops/project/wiki/markdown-guidance?view=azure-devops)
