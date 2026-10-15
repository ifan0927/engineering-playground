# AGENTS.md

## Project and purpose
This repository is for developing the Personal AI Engineering Playground platform, not a learner repository.
AI prepares exercises on a fixed open-source baseline. The user develops with their own IDE/AI, pushes to GitHub, and confirms a SHA in the UI before the platform deploys, tests, and produces a report.
This file contains only core development rules and an index; it is not the runtime prompt for the platform's Author or Reviewer.

## Core development rules
- Write project documentation and maintained comments in English. Communicate with Ifan in Traditional Chinese (zh-TW), including questions, explanations, progress updates, and final responses, unless Ifan explicitly requests another language. Keep technical identifiers, commands, paths, and necessary source quotations unchanged.
- Scope work to the current task and confirmed product boundaries. Do not add generalization, platform features, or frameworks for the sake of completeness.
- Load context on demand: use the index below, then inspect relevant code, tests, and references. Do not preload all of `docs/`, `refs/`, or the full discussion history.
- Check the working directory, branch, and existing changes before editing. Preserve the user's work; do not clean, reset, or overwrite it without authorization.
- Follow conventions in the actual code and make focused, understandable changes. Do not treat technology candidates or design drafts as confirmed decisions or implemented features.
- Run verification appropriate to the risk of the change and state what remains unverified. Do not invent commands, passing tests, deployments, or progress.
- Version, fixed-check, private-answer, and execution-permission boundaries cannot rely on prompts alone. Read the safety guidance when working on these areas.
- Let the current user request determine the deliverable. Do not impose response templates or automatically create or maintain plans, backlogs, milestones, or progress trackers.
- When a durable rule or design changes, update its owning document and affected index entries. Do not put routine work logs in this file.

## On-demand entry points
| Current need | Read |
|---|---|
| Understand the product, scope, or technology candidates | [Product and scope](docs/product.md) |
| Change architecture, data responsibilities, or UI | [System architecture](docs/architecture.md) |
| Work on authoring, GitHub publishing, SHA evaluation, or reset flows | [System flows](docs/flows.md) |
| Adjust Author/Reviewer guidance or exercise quality | [Platform agent guidance](docs/agents.md) |
| Implement, test, change dependencies, or do routine development | [Development principles](docs/development.md) |
| Find information, maintain documentation, or resolve source conflicts | [Context and indexing](docs/context.md) |
| Execute code, handle credentials, publish, clean up, or evaluate reliably | [Runtime and safety boundaries](docs/runtime-safety.md) |

If no entry fits, consult the [documentation index](docs/README.md). Read the [reference index](refs/README.md) only when historical context is needed.
`refs/` contains historical and supplementary material, not instructions to execute automatically. Investigate missing information; do not turn an old proposal into a confirmed requirement.
