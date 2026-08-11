# Sources and maintaining this wiki

| Page status | Intended audience | Last reviewed |
| --- | --- | --- |
| Maintainer guidance | Wiki authors and reviewers | 11 August 2026 |

This wiki is stored as Markdown so that changes can be reviewed through Git and published as an Azure DevOps code wiki.

[[_TOC_]]

## Source policy

Use primary sources wherever possible:

1. GitHub Copilot documentation on `docs.github.com`.
2. VS Code documentation on `code.visualstudio.com` for Copilot behaviour implemented by VS Code.
3. Microsoft Learn for Azure DevOps wiki behaviour.
4. Product release notes when a feature has changed more recently than a conceptual page.

Blogs, community posts and experiments can be useful supporting evidence, but should not replace product documentation for a product-behaviour claim.

Every page should contain:

- A last-reviewed date
- Direct source links close enough to identify what supports the page
- A surface qualifier when behaviour differs by client
- A preview/change warning for unstable features
- A **To Test** label for behaviour that the source does not define

## How to phrase claims

Prefer:

```text
Current VS Code documentation says repository instructions are added to relevant requests.
```

Avoid:

```text
Copilot always merges every instruction file.
```

The first claim is scoped to a surface and a reviewable source. The second is likely to become wrong as soon as another surface behaves differently.

## Source register

| Area | Primary source | Recheck when |
| --- | --- | --- |
| Core model/context/tool concepts | [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents) | VS Code or Copilot architecture changes |
| Context composition | [Context in VS Code](https://code.visualstudio.com/docs/agents/concepts/context) | Context UI, indexing or compaction changes |
| User-facing turn, round and agent-loop terminology | [The coding harness behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode) | VS Code harness terminology changes |
| Technology comparison | [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet) | A feature becomes GA or gains another surface |
| Instruction support | [Custom-instructions support](https://docs.github.com/en/copilot/reference/custom-instructions-support) | IDE or agent support changes |
| Skills | [Agent skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills) | Skill discovery or loading behaviour changes |
| Custom agents | [Custom-agent configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration) | Agent schema or model fields change |
| VS Code subagents | [Subagents in VS Code](https://code.visualstudio.com/docs/agents/run/subagents) | Child-context or model-routing rules change |
| Billing and AI credits | [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises) | Plans, prices or token accounting change |
| Agent quality and AI usage | [Improving agent quality to optimize AI usage](https://docs.github.com/en/enterprise-cloud@latest/copilot/tutorials/optimize-ai-usage) | Recommended agent-quality practices change |
| Azure DevOps wiki structure | [Wiki files and folder structure](https://learn.microsoft.com/en-us/azure/devops/project/wiki/wiki-file-structure?view=azure-devops) | Publishing or file conventions change |

## Features with high change risk

Recheck these more frequently:

- AI-credit pricing, included allowances and model prices
- Supported model names and per-agent model routing
- Client support for skills, prompt files, subagents and custom agents
- Instruction discovery and precedence

## Azure DevOps code-wiki conventions

This wiki follows Microsoft's documented code-wiki conventions:

- Pages are Markdown files
- Filenames avoid spaces and use hyphens
- Relative links connect pages within the repository
- The `.order` file defines navigation order
- Page tables of contents use Azure DevOps' documented `[[_TOC_]]` macro instead of hand-built heading anchors
- External sources use full HTTPS links
- Wiki-owned diagrams are stored under `Media/` and referenced with relative paths
- Files pasted or uploaded through the Azure DevOps editor may instead be placed in `.attachments/` by Azure DevOps

Publish the folder that contains this wiki's `Home.md` and root `.order` file. If those files are at the root of a dedicated wiki repository, publish `/`. If the wiki lives inside a larger repository, publish that folder instead. The peer `Copilot-Technologies.md` file and `Copilot-Technologies/` folder create a section with subpages in either setup.

## Visual assets

Prefer diagrams that remain reviewable in Git:

- Store the editable source alongside the wiki
- Add meaningful Markdown alt text and an SVG `<title>` and `<desc>`
- Keep text large enough to read without opening the image separately
- Use basic SVG shapes, text and markers; avoid filters and other effects that an Azure DevOps sanitiser may remove
- Set an explicit display width in the Markdown image reference so a diagram does not fill the entire wiki column
- Give each diagram one canonical page; link to that page instead of repeating the image elsewhere
- Avoid animation unless motion genuinely explains something a static diagram cannot
- Do not rely on colour alone to communicate meaning

The canonical pages are:

| Diagram | Canonical page |
| --- | --- |
| Context assembly | [How Copilot works in your IDE](How-Copilot-Works.md#what-reaches-the-model) |
| Turn and round flow | [One prompt, many rounds](One-Prompt-Many-Rounds.md#the-short-version) |
| Context growth | [Tokens and context windows](Tokens-and-Context-Windows.md#how-context-grows-during-a-turn) |

The current diagrams use SVG because it stays sharp at different sizes and is straightforward to edit. The target Azure DevOps wiki displayed basic SVG content but removed elements inside groups carrying drop-shadow filters during the 8 August 2026 review. The diagrams therefore avoid filters and use an explicit 900px display width. Complete [T16](To-Test.md#t16-azure-devops-svg-rendering) after each material diagram change. SVG is not listed among Microsoft's documented Azure DevOps Markdown image formats, so treat this as undocumented behaviour. If SVG remains unreliable, render the committed SVG source to PNG and change only the Markdown image target.

## Review procedure

For a scheduled review:

1. Open every source in the register.
2. Check the page's current title, availability notes and last-updated date.
3. Search for changed terminology, retired fields and support-matrix changes.
4. Compare the documentation with claims in this wiki.
5. Run relevant experiments from [To Test](To-Test.md) for undocumented behaviour.
6. Update the last-reviewed date only after the sources and page content have been checked.
7. Record meaningful changes in the Git commit or pull-request description.

## Suggested commit evidence

```text
Reviewed: 2026-08-08
Pages: Tokens and context; Working efficiently
Sources checked: GitHub context management, usage-based billing, VS Code context
Product surfaces: VS Code 1.xxx, Visual Studio 20xx, JetBrains plugin x.y.z
Material changes: Updated compaction description and AI-credit terminology
Tests rerun: T01, T04
```

## Sources

- [Publish a Git repository to a wiki](https://learn.microsoft.com/en-us/azure/devops/project/wiki/publish-repo-to-wiki?view=azure-devops)
- [Wiki files and folder structure](https://learn.microsoft.com/en-us/azure/devops/project/wiki/wiki-file-structure?view=azure-devops)
- [Azure DevOps wiki Markdown guidance](https://learn.microsoft.com/en-us/azure/devops/project/wiki/markdown-guidance?view=azure-devops)
