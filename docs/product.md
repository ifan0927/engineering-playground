# Product and scope

**Read when:** Determining whether a task fits the product, expands scope, or depends on a confirmed technology choice.

This document consolidates user-confirmed direction. Concrete implementation defaults are labeled separately; detail alone does not make a proposal an adopted decision.

## Product core

A personal AI engineering playground. Given the user's stated engineering background and goals, the Author designs deep, runnable, reproducible, and verifiable engineering tasks on a real open-source codebase.

The user clones a learner repository, investigates and edits with their own IDE/coding agent, tests, and pushes. The platform provides the task and environment, fetches the user-confirmed revision, runs verification, and provides an engineering review.

Practice and interactivity are the primary purposes. The project and completed engineering cases may serve as portfolio material; formal upstream contributions, maintainer review, and a multi-user commercial product are not required goals.

## Confirmed design boundaries

| Topic | Current decision |
|---|---|
| Baseline | The first version serves one selected repository. Exploration, build verification, and operating notes are prepared during development; no onboarding feature. |
| Agent execution | Each platform agent task uses one one-off session; no cross-session state management, automatic recovery, or long-running facilitation. |
| Author maintenance | Primarily manual adjustment through AGENTS.md, prompts, skills, prepared context, and a few scripts; methods can improve through use. |
| Personal development | The user retains their own IDE/coding agent; the platform does not control its conversations, uncommitted changes, or personal AI settings. |
| Code storage | GitHub stores platform and exercise code revisions; private authoring content remains separately visible. |
| Submission | After pushing, the user confirms the repository, branch, and SHA in the platform UI; the platform fetches, deploys, and tests that revision. No PR is required. |
| Review | Retain an independent Reviewer. Deliver feedback as a report, without inline comments or comment management. |
| Verification | Retain the Verifier as ordinary code that executes rules and tests, not another AI. |
| Quality iteration | When exercises are unsuitable, the user changes the Author or platform directly; do not turn this into a capability-analysis or feedback product. |

## Explicit exclusions

Do not integrate Jev or reserve a dedicated integration point. Do not build Consult, an AI coding chat UI, user-workspace synchronization, or monitoring of personal AI conversations.

Do not build learning-outcome measurement, capability inference, learner models, self-assessment/satisfaction forms, automatic curricula/difficulty adjustment, or automatic feedback from review results into the next exercise strategy.

Do not implement PR submissions, merges/approvals, inline comments, or GitHub review threads. This is the product scope for exercise submissions, not a rule governing the user's Git policies in every other repository.

Do not import the complete delivery state machines, agent negotiation, or automatic repair lifecycles from ALC/Agent Workbench. Those projects' requirements do not automatically become this project's requirements.

## Generalization outside the first version

Retain the constrained v0.4 design: no arbitrary-repository onboarding, multi-provider plugins, multi-user/multi-host scheduling, self-hosted Git, webhooks, automatic evaluation on push, Kubernetes, knowledge graphs, or large retrieval services.

These are not backlog items and do not require reserved abstractions or empty modules. Do not add them for imagined future scenarios without a new explicit requirement.

The thin UI, AI authoring, and runnable environment remain part of the product. Simplification must not replace the whole product with text-only tickets.

## Suggested defaults, not fully confirmed choices

| Area | Existing proposal and remaining uncertainty |
|---|---|
| Controller and UI | Go and React/shadcn are design defaults, not an established implementation. |
| Data and environment | SQLite, controlled file storage, and Docker Compose are design defaults. |
| Agent runtime/model | Codex CLI is a candidate execution entry point; its version, authentication, model, and available capabilities still need confirmation. |
| Execution host | Mac mini is the main proposal; CPU architecture, resources, access, and isolation have not been verified by this package. |
| Open-source baseline | HertzBeat is only a previously proposed candidate; the repository, base SHA, and operating recipe are not confirmed. |
| Code organization | One controller, a few record types, and one active environment are v0.4 simplification proposals; module/table names are not fixed. |
| Repository separation | A private platform repository plus independent learner repositories is a proposal. GitHub is selected, but namespaces, permissions, and actual repository names are unconfirmed. |

For relevant tasks, a new agent should inspect the current repository's facts. It need not ask about every unresolved choice each time, and must not reopen questions about already excluded features.

## Development conventions for this starter package

Keep only core rules and an index in AGENTS.md. Retrieve detailed context, rules, and references on demand. Navigate documents by task rather than requiring all content to be loaded.

Write project documentation in English; communicate with Ifan in Traditional Chinese (zh-TW) under the language rule in the root AGENTS.md.

This package does not prescribe development plans, schedules, milestones, backlogs, or progress systems, nor fixed sections or documents that a development agent must deliver every time. Address these only when the current user request asks for them.

The product must still deliver usable exercises and trustworthy results. That is a product responsibility, not a claim that this package has frozen the exercise JSON schema, report template, or implementation-document format.

Sources and changes: [Reference index](../refs/README.md).
