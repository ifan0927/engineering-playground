# Personal AI Engineering Playground

AI prepares engineering tasks and runnable environments on real open-source projects. The user solves them with their own IDE, coding agent, and Git workflow; the platform then deploys a specified revision, runs tests, and provides an engineering review report.

**Practice is the focus; the platform is the tool that makes it possible.** The Author can improve incrementally without first building a general-purpose agent platform.

## What this project package contains

This starter package belongs at the root of the platform repository. It contains the current product definition, architecture, development principles, and on-demand documentation index.

This repository currently contains documentation and basic text-formatting/Git ignore settings. It does not yet include an executable application, a connected environment, or validated exercises. Documentation completeness is not implementation progress.

Repository: [ifan0927/engineering-playground](https://github.com/ifan0927/engineering-playground). The default branch is `dev`.

It is based on the existing v0.4 architecture and the user's subsequent requests. Technologies, tables, and filenames that have not been individually confirmed remain proposals. This package does not prescribe development order, dates, milestones, progress tracking, or a fixed deliverable format for development agents.

Project documentation is written in English. Agents must communicate with Ifan in Traditional Chinese (zh-TW), as specified in [AGENTS.md](AGENTS.md).

## User experience

```text
Prepare one fixed baseline and its exploration notes
  → Explicitly trigger one Author task
  → Run verification and independent exercise review
  → Publish a GitHub learner repository and provide its environment
  → The user clones, edits with their own IDE/AI, and pushes
  → Select a branch in the UI and confirm the full commit SHA
  → The platform fetches that revision, builds, deploys, and tests it
  → Display test results and an engineering review report
```

No PR, merge into main, or inline comments are required. The platform does not take over the user's coding agent.

## Core boundaries

The first version targets one prepared baseline and does not include project onboarding. Each platform agent task uses a fresh, one-off session, with no promise of automatic cross-session recovery. GitHub stores code revisions; private authoring material is not published with the learner repository.

Keep the Author, Reviewer, non-AI Verifier, real Git workflow, and runnable environment. Exclude Jev, Consult, learning-outcome assessment, automatic curricula, feedback systems, and PR/comment workflows.

See [Product and scope](docs/product.md) for confirmed boundaries, proposals, and unresolved conditions.

## Working with a development agent

Use this folder as the platform project root, have the agent read [AGENTS.md](AGENTS.md), and give it the current task. Do not add all documentation to the conversation at once; the agent should use the index to inspect relevant topics and actual code.

When adding this package to an existing repository, merge its guidance and settings rather than overwriting the existing `AGENTS.md`, `.gitignore`, or `.editorconfig`.

A sufficient opening instruction is:

> Read the root AGENTS.md first. Use its index to retrieve information needed for this task; do not read every document. The current task is: [task].

This does not authorize full implementation, deployment, service purchases, or remote-resource creation. The agent works within the current request and environment permissions; it does not need to propose an overall development plan each time.

## Documentation entry points

| Entry | Purpose |
|---|---|
| [AGENTS.md](AGENTS.md) | Concise core rules and task routing. |
| [docs/README.md](docs/README.md) | Topic selection by task, including source authority and when to read. |
| [refs/README.md](refs/README.md) | Historical architecture and supplementary Author material, read only when needed. |

Root guidance governs development of this platform. The platform's Author, Reviewer, and future learner-repository guidance serve different purposes and must not be copied wholesale between contexts.

## Technology and execution notes

GitHub is confirmed. Go, React/shadcn, SQLite, Codex CLI, Docker Compose, and Mac mini are existing design defaults, not a claim that all have been selected or are ready to run. HertzBeat remains a previously discussed baseline candidate.

This package does not provide guessed installation or startup commands. Once the stack and baseline are established, document actual commands, prerequisites, and verification entry points in the relevant topic and link to them from the index.

Compiled: 2026-10-15 (Asia/Taipei). English-language revision: 2026-10-15. These dates describe documentation work, not a system release.
