# Visual concepts POC

_Draft visual review page - 27 August 2026_

> **Proof of concept:** these diagrams are here for content and layout review. They are not yet the final illustration set. Once the useful versions are agreed, they can move onto the relevant pages and this POC page can be removed.

[[_TOC_]]

## 1. Repository knowledge as task-driven routing

This now uses the observed React and .NET Field Visits pilot. It compares orientation reads and model requests before the first edit, then shows the shared acceptance outcome and a cache-normalised cost estimate beside the observed billing anomaly.

![An observed Field Visits implementation comparing orientation without repository knowledge against a small pointer, root index and two focused guides](Media/poc-repository-knowledge-routing.svg =900x)

Related page: [Repository knowledge for people and agents](Repository-Knowledge.md)

## 2. Choose a Copilot technology by what is reused

This turns the technology comparison into a question-led choice. It also separates reusable content or process from the Copilot feature used to deliver it.

![Rules route to instructions, procedures to skills, saved requests to prompt files, worker roles to custom agents, isolated work to subagents and guarantees to deterministic enforcement](Media/poc-technology-choice.svg =760x)

Related page: [Choose the right Copilot technology](Copilot-Technologies/Choose-the-right-technology.md)

## 3. Skills use progressive disclosure

This shows why a skill catalogue can remain discoverable without loading every workflow and reference into every request.

![Skill name and description are always discoverable, while the skill body and linked resources load only after the task selects them](Media/poc-skill-progressive-loading.svg =760x)

Related page: [Agent skills](Copilot-Technologies/Agent-skills.md)

## 4. Context boundary and cost consequence

This compares continuity within one agent, a custom-agent handoff that retains the conversation, and a fresh chat that reads a saved plan. It is intended to make the context and cache trade-off visible without claiming that one route is always cheaper.

![Same-agent continuation may retain a warm prefix, an agent handoff changes instructions while retaining accumulated context, and a fresh chat starts cold with selected input](Media/poc-context-cost-options.svg =760x)

Related pages: [Custom agents and subagents](Copilot-Technologies/Custom-agents-and-subagents.md) and [Working efficiently and managing cost](Working-Efficiently-and-Managing-Cost.md)
