# System architecture

**Read when:** Changing module boundaries, data responsibilities, code storage, UI, or platform integrations.

Basis: v0.4 sections 1–5, 10, and 12–13. This is a design, not a deployed system. See [Product and scope](product.md) for the distinction between confirmed decisions and suggested defaults. Data names and diagram components do not prescribe actual code directories or schemas.

## Overall shape

GitHub stores code. A thin controller connects one-off AI authoring, exercise publication, environment operations, and fixed-revision evaluation. The user works with their own IDE/AI, outside the platform's control.

```text
Prepared baseline, operating notes, manually maintained authoring brief
                              │
User ── Thin UI ── Playground controller
                              │
            ┌─────────────────┼──────────────────┐
            ▼                 ▼                  ▼
     Agent entry point   Git / Publisher   Runtime / Verifier
     Author              GitHub sources    Build, deploy, test
     Reviewer            Learner export    Initialize data, clean up
            └─────────────────┼──────────────────┘
                              ▼
                 Revision references, records, reports

User ── clone learner repo ── own IDE/AI ── push to GitHub
  └── confirm branch/full SHA in UI ── fetch/deploy/test ── report
```

These are responsibilities, not microservices or one agent per box. Author and Reviewer may share one existing runtime, but each task has its own fresh session. The Verifier is ordinary code.

The proposed deployment uses one main host and one controller to serve the UI and operations, alongside existing Git and environment tools. Do not add Redis, a scheduling service, an event bus, or a general-purpose agent framework.

## Component responsibilities

| Component | Responsible for | Not responsible for |
|---|---|---|
| Thin UI | Exercise entry points, environment operations, branch/SHA confirmation, and results. | An IDE, AI chat, PR/inline review, or development-progress management. |
| Controller | Explicit operations, input preparation, execution records, and fixed-order tool calls. | Every agent search decision or the agent's reasoning state. |
| Agent entry point | One-off Author/Reviewer calls, controlled context, cancellation/timeouts, and result collection. | Long-conversation recovery, multi-model routing, task negotiation, or automatic repair until success. |
| Git / Publisher | Fixed-source retrieval, learner-content export, GitHub publication, and revision checks. | Self-hosted Git, PRs, merges, approvals, or comment synchronization. |
| Runtime / Verifier | Build, deploy, initialize, run fixed checks, and preserve actual results using established recipes. | Declaring success from AI self-assessment or running arbitrary model-generated host commands. |

Implement these as functions, packages, and a few scripts in one program; they do not each need a service, database, or plugin.

## Code and data locations

| Content | Location and responsibility |
|---|---|
| Platform code and stable design | GitHub platform repository; this package can seed its documentation. |
| Fixed baseline and operating information | Source repository/base SHA plus versioned exploration notes and environment recipes. |
| Initial exercise code, public task, and user changes | GitHub learner repository. One independent repository per exercise is the v0.4 publication proposal. |
| Private authoring material, reference solution, and fixed checks | Controlled private GitHub scope, not other branches in the learner repository. |
| Checkouts, build cache, images, and environment data | Working copies and runtime storage on the execution host; not another source-code storage service. |
| Operation relationships, execution results, logs, and reports | A few local platform records and controlled files, backed up as needed. |

If the platform repository starts private, private authoring code may live there without requiring a third repository. Before making the platform public, inspect and separate private material, including history. Publication is not just a visibility toggle.

Code on GitHub does not mean local records are backed up. Sources needed for reruns must remain retrievable. If a source is lost, retain existing reports and state the limitation; do not build a mirror/bundle service to cover every scenario.

## The exercise package is a semantic handoff, not a frozen format

The Author and platform need identifiable, executable, and inspectable exercise content. Three responsibilities remain:

**Learner-facing material:** Initial code, public scenario and requirements, environment/test information, necessary exploration entry points, and GitHub submission instructions.

**Private authoring material:** Design rationale, root cause, reference solution, fixed checks, initialization data, and authoring/publication execution evidence.

**Identity information:** Exercise version, baseline and starting revision, relevant recipes/tests, and their storage locations, sufficient to identify exactly which material is in use.

Names such as `manifest.json` and `reference.patch` in v0.4 illustrate organization. This package does not create a formal schema, fixed-filename contract, or template. Decide the smallest interface when integrating code; an unsettled format does not remove the need for verifiable content.

Published requirements and checks must not change with code edits during an exercise. Revisions to requirements, tests, or environment conditions create a distinguishable exercise version, preserving the criteria used for earlier submissions.

## Minimal persisted information

v0.4 proposes exercise versions (`ExerciseVersion`), submissions (`Submission`), and executions (`Run`). These names describe meaning, not existing database tables.

An exercise version identifies public/private content and the starting revision. A submission identifies the repository and SHA the user asked to evaluate. A run records one operation, its result, reason, and report location. One submission may have multiple execution records from reruns.

Store the active exercise, actual environment identity, and deployed revision as a small amount of configuration. Do not create additional management systems for baselines, practice sessions, or individual review comments.

Running, finished, failed, cancelled, and interrupted describe execution status, not development progress or agent reasoning. Separate behavioral test results from review opinions; a completed invocation is not a passed evaluation.

## Operation and execution lifecycle

The proposal allows one active exercise environment and only one authoring, evaluation, or environment-changing operation at a time. Report that the system is busy rather than first building a queue.

Leaving the browser does not cancel server work. After the controller stops, do not promise automatic agent continuation. Inspect remaining processes/resources, mark interruptions, and require an explicit rerun. This does not require a cross-session recovery engine.

Maintain enough association between inputs, fetched sources, build artifacts, and results to explain each run. Event sourcing, digital signatures, and an evidence-attestation platform are not required.

## Three kinds of UI content

**Exercise entry:** The current authoring request, available exercises, and active practice. No arbitrary-project onboarding or model marketplace.

**Task and environment:** Task description, clone/application entry points, actual deployed revision, start/stop/reset, and submission by confirmed branch/full SHA.

**Results:** The evaluated revision, build/deployment/test results, a readable engineering review, and existing run records. Reports may be Markdown; no per-comment threads, resolved status, or GitHub synchronization.

These can be sections on one page, using polling. Page count, component structure, and API paths are not fixed in advance. Do not stream raw private agent output directly into the practice screen.

## Change boundaries

Author selection, exploration, and methods may evolve within their existing responsibilities. A new task type requiring different container topology, data migration, or verification capabilities changes the environment/contract; it is not necessarily a prompt-only change.

See [System flows](flows.md) for operations and [Runtime and safety boundaries](runtime-safety.md) for trustworthy execution and permissions. Consult the [reference index](../refs/README.md) only when the original proposals are needed.
