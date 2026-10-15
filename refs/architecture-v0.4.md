# Personal AI Engineering Playground — v0.4 System Architecture Draft

> Translation note (2026-10-15): This is a complete English translation of the historical v0.4 document, preserving its structure, proposals, and original date. It is not a byte-identical original. Later decisions in the root guidance and `docs/` take precedence; see [Reference index](README.md). Historical implementation plans and deliverable suggestions below are not current instructions.

Date: 2026-10-14 (Asia/Taipei)  
Audience: Ifan and the Codex agent taking over later  
Document type: Design and implementation boundaries, not a deployed-system description, performance test, or security-verification report.  
Revision: GitHub is confirmed as remote code storage; PR submissions and integrations are removed. The UI confirms repository, branch, and full commit SHA, after which the platform fetches, builds, deploys, and tests that revision. Engineering review produces reports only, without inline comments.

## 0. Purpose and authority of decisions

The goal is to establish a complete, usable engineering-practice flow. Author selection, exploration, and preparation methods can be adjusted manually through use without resolving every Author-design question first.

This version follows the v0.3 architecture draft and earlier v0.2 handoff. The following latest user decisions take precedence over older text:

- The first version serves one selected open-source repository. Exploration, build verification, and operating information are prepared during development; no in-product project onboarding, setup wizard, or multi-project configuration management.
- Each platform agent task uses a single session. Do not build cross-session state machines, automatic recovery, long-conversation facilitation, or agent negotiation.
- Configure the Author primarily through AGENTS.md, prompts, skills, prepared background, and a few scripts. Manually revise its strategy through use.
- Exclude Jev and do not reserve a dedicated integration.
- Retain the user's own IDE/Codex, real Git delivery, a runnable environment, the Verifier, and an independent Reviewer.
- GitHub is remote code storage. Version platform and exercise sources on GitHub while keeping private authoring code and checks separately visible. Do not self-host Git or build another source-storage system.
- Submissions do not use PRs. After pushing, the user confirms the exercise repository, branch, and SHA in the UI; the platform fetches and evaluates only that confirmed revision.
- Code review produces a report. Do not pursue inline comments, GitHub review threads, or individual-comment management.
- Exclude Consult, capability assessment, learning-feedback systems, and automatic curricula. If the tool is unsuitable, change the rules or code rather than building a productized feedback pipeline.

The programming languages, tables, exact GitHub repository split/names, filenames, buttons, and implementation details below are suggested defaults, not individually confirmed user decisions. GitHub, no-PR submissions, branch/SHA confirmation, and report-only review are confirmed. The baseline, model, host capabilities, and authentication still require confirmation in the implementation environment. HertzBeat remains a previous candidate.

## 1. The system in one sentence

**GitHub stores code; a thin controller connects one-off AI authoring, exercise delivery, and deployment, testing, and reporting for user-confirmed revisions.**

```text
Preparation: selected repo + pinned revision + exploration notes + verified recipes
                                      │
                         Manually maintained context/rules
                                      │
User ── Web UI / CLI ── Playground controller
                                      │
                  ┌───────────────────┼───────────────────┐
                  │                   │                   │
           Agent entry point    Git / Publishing    Runtime / Verifier
           Author               Independent         Build, deploy, test
           Reviewer             learner repo        Initialize, clean up
                                Pinned source SHA
                  │                   │                   │
                  └───────────────────┼───────────────────┘
                                      │
                           SQLite + private file storage

User ── clone GitHub learner repo ── own IDE/Codex ── commit/push
 │                                                            │
 └── view test/review reports ◀── fetch/build/deploy/test ◀── confirm SHA in UI

GitHub: platform code / private authoring code and tests / independent learner repos
Local: rebuildable working copies, runtime artifacts, SQLite index, logs, reports
```

Boxes describe software responsibilities, not separate microservices. Author and Reviewer share one existing coding-agent runtime but use fresh sessions and different task contexts. The Verifier is code, not a third AI.

## 2. Suggested physical deployment

Use **one Mac mini as the controller/execution host, with a Go controller, thin React/shadcn UI, SQLite, file storage, Codex CLI, GitHub, and Docker Compose** as the concrete design default.

This does not claim that the Mac mini is connected, has sufficient resources, or has every tool installed. Initial development must still confirm CPU architecture, required images, and the toolchain.

The controller can serve frontend static files without a separate frontend service, Redis, queue server, model service, or container cluster. GitHub is the confirmed remote code source. Do not introduce PR integration, a GitHub App/webhooks, self-hosted Git, or evaluation automatically triggered by push. The user explicitly starts evaluation after confirming a revision; GitHub Actions is not required.

Development may use an existing remote connection to the Mac mini or happen locally on another device followed by a push. The platform does not read or synchronize uncommitted user changes. Provide application access through a reachable address or existing protected tunnel; remote localhost is not the user's device-local localhost.

Expose the control plane only locally or through protected access, not as public host control for a portfolio demo. The frontend shows public exercise material, execution status, and user reports, not a live stream of the Author's private editing process.

### 2.1 GitHub storage scope and minimal separation

Delegate code storage to GitHub. The platform keeps revision references, execution indexes, and necessary artifacts rather than another remote source-storage service. The following is proposed; repository names and public/private visibility still need confirmation during implementation:

| Content | Suggested storage |
|---|---|
| Playground code, architecture documents, general role rules | GitHub platform repository, initially private if appropriate. |
| Private authoring settings, reference patches, fixed checks, and fixtures | Private GitHub scope. A private platform repository may contain a controlled directory for these without requiring a third repository. Before publishing the platform repository, move this material to a separate private repository. |
| Each exercise's initial code, public task, and user changes | An independent GitHub learner repository per exercise, following the snapshot-publication strategy. |
| Working checkouts, build cache, images, volumes | Temporary/runtime storage on the execution host, not committed to Git. |
| Execution relationships, logs, test and review reports | Local platform database/files. Reports need not be committed or pushed to GitHub every time. |

Using GitHub for all code does not mean putting everything into one learner repository. Private answers, authoring history, and fixed checks must not live on other learner branches. Hidden directory names do not prevent a solving agent from finding them. Repository separation does not establish local agent-access isolation; section 11 still applies.

The baseline source is the GitHub repository and base SHA selected during preparation. The platform's local clone is an execution copy, not the only place preserving code. Unpublished drafts may remain local temporarily; published exercise code, reference solutions, and fixed checks require GitHub revision references. Do not upload keys, login data, or secret-bearing logs as code.

## 3. Material prepared once during development

Prepared baseline material may live in private GitHub-versioned directories, with local copies used for execution. Do not create a Project table or onboarding workflow:

```text
prepared/
  repo-guide.md        # Pinned revision, entry points, confirmed flows, limits, source locations
  author-brief.md      # User-supplied background, direction, exercise preferences
  baseline.json        # Upstream location, base SHA, toolchain/recipe revision
  recipe/              # Approved container recipes and build/initialize/test entry points
agent-guidance/
  author/              # AGENTS.md, skill, prompt, a few good/bad examples
  reviewer/            # Exercise/submission review guidance: two one-off tasks for one role
```

This illustrates content organization. Actual skill installation paths depend on the selected runtime; arbitrary directories are not assumed to load automatically.

The repo guide is a map, not a permanently complete repository summary. Distinguish observed execution, code-based inference, and unresolved information. For each exercise, the Author continues reading and experimenting along relevant flows rather than rescanning the whole project or trusting notes blindly.

Start each exercise from a clean copy of the same known base SHA. Do not automatically accumulate a previous faulty revision, reference solution, or user answer into the next baseline. Update preparation material and retest manually when upgrading; do not automatically follow upstream.

The first version reuses exploration through files, not an endlessly extended conversation. Session IDs may help debugging but are neither authoritative project knowledge nor prerequisites for recovery.

## 4. Internal responsibilities

**Controller:** Accept operations, create run records, prepare inputs, invoke tools in a fixed order, and collect results. It does not determine which module offers exercise value or control every agent search.

**Agent entry point:** Handle one-off Author/Reviewer invocations, workspace and permission configuration, timeouts/cancellation, and result/private-log preservation. Codex non-interactive execution, JSONL, and structured final output can support this entry point. [S1] The first version does not depend on resume, fork, App Server, or multi-model routing.

**Git / Publisher:** Export learner content, create independent history, publish to the configured GitHub remote, and preserve GitHub revision references for published private exercise packages. During submission, provide branch/commit lookup, obtain the user-confirmed full SHA, and verify the checkout. Author/Reviewer cannot directly push remotes or force-push user branches. This module does not read/create PRs, post GitHub comments, create review threads, or manage merges/approvals.

**Runtime / Verifier:** Build, deploy, initialize, run exercise checks, and collect evidence in specified environments. Use approved recipes; do not execute arbitrary model-generated shell/Compose configurations with host privileges. Newly generated tests and fixtures run in a restricted execution area.

These may be Go packages and fixed scripts, not a provider-plugin architecture, event bus, or independent services.

## 5. The main handoff artifact: an exercise package

Author internals may change while the minimal handoff to the platform remains stable:

```text
exercise-package/
  manifest.json
  learner/                         # Content to export as a cloneable repository
    <required upstream source and documentation>
    AGENTS.md                      # Solving guidance, not authoring guidance
    .playground/
      TASK.md                      # Scenario, requirements, scope, completion conditions
      ENVIRONMENT.md               # Startup, testing, observation, debugging
      SUBMISSION.md                # Push → confirm branch/SHA in UI → evaluate/report
      exercise.json                # Exercise identity without spoilers
  private/
    design.md                      # Root cause, design rationale, quality rationale
    reference.patch                # Reference fix relative to the final learner starting content
    checks/                        # Fixed evaluation and reproduction tests
    fixtures/                      # Initial data; public portions may be exported separately
    evidence/                      # Actual results and private authoring records
```

At minimum, the manifest identifies package-format version, task type, base SHA, content locations, approved recipe reference, fixture/check identities, and expected outcomes. The controller verifies and supplies Exercise ID, published version, and learner starting SHA rather than trusting Author-supplied source fields.

Do not embed the starting SHA in the same commit that must produce that SHA. The learner repository contains non-self-referential information such as exercise ID/version; the platform record stores the full SHA.

Public requirements must be clear enough. Private tests may use undisclosed data cases but cannot introduce multi-node behavior, uninterrupted migration, or a specific function implementation absent from the task.

Publication freezes the complete exercise version: task, starting code, recipe, fixture, checks, reference solution, and publication evidence. Prompt changes do not retroactively change exercises. Requirement/grader revisions create a new version, preserving earlier criteria for existing submissions.

One package version can cover all of this; recipes, fixtures, and graders do not each require a version-management service. Assembly may begin as local files. After publication, locate learner code by GitHub repository + starting SHA, and private code/tests by private repository + commit + directory; local files are execution copies. References and checksums prevent mix-ups, not provide tamper-proof attestation. "Code on GitHub" does not require committing large runtime artifacts or raw logs.

## 6. End-to-end flow A: prepare a new exercise

### 6.1 Explicit trigger

The user enters the desired direction in the UI and selects "Prepare exercise." The controller acquires the single execution lock, saves the request and guidance snapshot, and creates a private workspace from the fixed baseline.

One Author session handles local exploration, scenario selection, edits, tests, and corrections within the task. It may use tools repeatedly without persisting every internal step as platform state.

### 6.2 Author delivers a draft

The Author produces exercise content and limitations. The controller checks required fields, file existence, path scope, and revision consistency. A successful agent exit or "done" message means only that the invocation ended, not that publication is allowed.

Requests outside the recipe, such as privileged mounts, external-service credentials, or unsupported deployment shapes, stop with a clear need for manual baseline-configuration changes. AI does not expand execution privileges.

### 6.3 Assemble the actual learner repository

The Publisher exports only learner content and creates independent Git history. Do not copy authoring `.git`, answer refs, private packages, personal agent settings, or credentials. Preserve required upstream source, documentation, and license files.

If the baseline relies on submodules, LFS, Git-derived version generation, or special output files, handle that explicitly during baseline preparation instead of trusting a generic archive. Inspect `.github` automation so practice pushes cannot accidentally trigger upstream product releases or deployments.

A private candidate GitHub learner repository may be created before validation, but remote creation is not completed publication. Candidates that have not passed all checks are absent from the practice list. Version private exercise code/tests in a controlled private repository, not in the learner repository. Immediately save remote locations, source revisions, and candidate IDs; verify before retrying rather than creating duplicate or same-named repositories repeatedly.

### 6.4 Verify the final delivered content

The Verifier clean-clones the final learner repository, pins its SHA, and actually builds, initializes, and starts it. Apply the reference solution in another clean copy. Do not deploy the answer first and then deliver that container as the faulty environment.

For a controlled bug exercise, the known-good baseline behaves correctly under declared conditions, the initial exercise exhibits the intended problem, and the reference solution passes target and necessary regression checks.

For a feature exercise, the initial revision retains necessary existing behavior but does not meet the new requirement; the reference implementation satisfies both the new and required existing behavior.

Evaluate the expected behavioral difference, not a blanket requirement for all initial tests to pass. A nonzero exit code alone does not prove the intended fault exists. Distinguish target-behavior failure, test-system error, and build failure.

### 6.5 One-off exercise review and publication

After verification, invoke the Reviewer in a fresh session to assess the task, tests, actual evidence, and reference solution. Check consistency between requirements and evaluation rather than relying on Author self-assessment.

Mark an exercise ready only when execution evidence is complete and matches expectations, no blocking review issues remain, and published code/checks have saved GitHub revision references. Publication checks bind to the same learner SHA and private package; any content change invalidates earlier checks. Exercise review produces a platform report, not a GitHub PR/review.

If Author, Verifier, or Reviewer fails or lacks evidence, stop and preserve the draft and cause. No automatic Author ↔ Reviewer revision loop or automatic reselection.

Exercise revision requires an explicitly triggered new Author task. Earlier drafts/reports may be input, but this remains a new session, not automatic recovery. A failed tool step may be rerun against the same fixed artifact as a new execution without invoking the Author again.

**One button may invoke Author and Reviewer sequentially, but each agent task is still one session, not a continuing multi-agent conversation.**

## 7. End-to-end flow B: start and continue practice

The user selects a ready exercise. The platform makes it active, deploys its initial learner revision using the fixed recipe, and initializes data from its fixture.

The workspace provides the task, GitHub clone location, starting revision, actual application entry point, currently deployed revision, and environment operations. After cloning, the user investigates, develops, debugs, writes tests, and commits/pushes with their own IDE/Codex. The platform neither controls AI conversations nor observes uncommitted local content. No PR is required; return to the workspace to confirm the branch/full SHA for evaluation.

Development and platform exercise/evaluation environments have separate data scopes. The user manages the former; the latter runs fixed artifacts without mounting actively edited source.

Practice can span several days. A one-off agent session does not mean practice must finish in one sitting or that closing the browser ends work or deletes data.

The first version has no separate PracticeSession entity. Store only the current exercise version, environment identity, and actual deployed revision. Repeating an exercise may use a new branch from its starting commit; do not track blind-attempt counts, seen hints, or AI usage.

## 8. End-to-end flow C: confirm a revision, deploy, test, and report

**A Submission means "evaluate this GitHub revision," not a PR, pending merge, or approval request.**

### 8.1 Select a revision in the UI, then confirm the full SHA

The exercise already identifies its learner repository. Display that GitHub repository directly without asking for a URL each time or supporting arbitrary-repository onboarding. After pushing, the user selects a branch; the platform retrieves a candidate commit and displays its branch, full SHA, and brief commit information for confirmation. An earlier revision can also be specified by a full SHA that is retrievable from that repository.

When the user selects "Deploy and evaluate this revision," the request contains the exercise version, repository identity, and **full SHA already confirmed on screen**. The branch is a source label. The controller only needs to verify this tuple, not create a confirmation-workflow entity with its own lifecycle. Submission notes are optional and brief; no PR description or simulated PR form.

If the confirmation screen showed A and the remote branch later moves to B, evaluate A. Do not resolve the branch again during execution and silently test B. Only an explicit revision selection or refresh changes the candidate. Stop clearly if the confirmed commit cannot be found, the source does not match the exercise repository, or read permission is missing. Do not fall back to the latest branch or another revision. Retrieval of arbitrary unavailable SHAs is not guaranteed.

Unpushed local changes are outside platform evaluation. The first version does not require merging a fix into main. The user manages their own branches; the platform evaluates only the confirmed revision.

### 8.2 Fixed execution order

```text
The user commits/pushes to GitHub
  → UI displays the exercise repository; select a branch/commit
  → The user confirms the full SHA and selects "Deploy and evaluate"
  → Record repository + source SHA + exercise version; acquire the single execution lock
  → Fetch that commit from GitHub, clean-checkout, and verify actual HEAD
  → Build using the approved recipe in an area without private credentials
  → Record the actual image ID and build environment
  → Preserve necessary previous logs and recreate evaluation data
  → Deploy the artifact built in this run
  → Execute the platform's frozen checks
  → A fresh Reviewer session reads the fixed task, diff, optional notes, and results
  → Save and display test results and an engineering review report
```

"Fetch and directly deploy/test" means one confirmation initiates the full product flow. Source still requires the recipe's build steps; neither raw source nor a floating official image counts as the deployed submission. Once a new artifact is deployed, immediately record its SHA/image rather than waiting for passing tests before updating the environment display. If startup fails, display the attempted revision and actual observations; do not incorrectly claim the old environment remains healthy.

If the new build fails, do not first remove the previous exercise environment while it remains useful for observation. Deploying the evaluation artifact resets the specified stage's data, so the button and confirmation must explain that effect. A push during evaluation does not change this run; evaluating the new revision requires another explicit confirmation.

Fixed tests come from the platform's frozen exercise package, not an edited learner TASK.md, self-declared grader, or handwritten PASS. New user tests may inform review but do not overwrite official evaluation. The Reviewer compares the same exercise's starting SHA with the submitted SHA, independently of PRs or main's merge base.

### 8.3 Review is a readable report

Suggested report contents:

- Repository, source-branch label, full submitted SHA, exercise version, starting SHA used for comparison, and Run ID.
- Build, deployment, and fixed-test results, plus necessary failure reasons and execution evidence.
- The Reviewer's change summary, confirmed problems, unresolved concerns, and nonblocking improvement suggestions, with file paths/line numbers where useful.

File/line references support reading this report for a fixed revision. Do not implement inline annotations, diff viewers, cross-version location tracking, threads, resolved status, or GitHub comment synchronization. Markdown rendered safely in the platform and made available for download is sufficient for the first version; each opinion need not become a database entity. Reports need not be written back to GitHub automatically.

Present tests and review opinions separately. Tests may pass despite an evidenced requirement violation. If the Reviewer invocation fails, show review as unavailable without erasing existing test evidence. Alternative implementations meeting requirements are acceptable; the reference patch is not the sole criterion.

The Reviewer does not edit user branches, merge, modify the grader, or post GitHub reviews/comments. Unverified concerns and personal style preferences are not confirmed defects. If an exercise is flawed, propose a revision and evaluate its new version; an LLM must not simply rewrite an old report as PASS.

The user chooses whether to modify, rerun, or end practice without waiting for approval or a merge. Reports can be copied to the user's own Codex. The platform does not take over subsequent fixes or turn review into capability scores or next-exercise strategies.

## 9. Environment management and minimal operations

The first version has one active exercise stage. Author/publication checks use disposable working environments, while the user's development directory remains separate. These are not three permanent deployments.

Run only one authoring, evaluation, or environment-changing operation at a time. Report busy when another operation is running rather than building a queue first. An idle active stage may keep running, but its artifacts and mutable data are not shared with authoring environments. If resources are insufficient, the user explicitly stops it; do not silently remove it.

Use Compose project names to distinguish platform-owned environments. [S2] Host ports, bind mounts, and volume paths must also be separate. Avoid fixed global volume names and unapproved external resources; name separation is not a security sandbox.

| Operation | Behavior |
|---|---|
| Start | Initially deploy the exercise's starting revision. If stopped, start the recorded current deployment without silently resetting data. |
| Stop | Stop the specified stage while preserving the repository, submissions, reports, and designated data. |
| Reset exercise | Restore initial artifacts and fixtures for that exercise, touching only the platform-owned stage. |
| Deploy and evaluate | Build the user-confirmed GitHub SHA; after a successful build, recreate evaluation data, deploy the artifact, test, and produce a report. |
| End practice | Stop/clean the explicitly identified stage resources and clear the active-exercise pointer without deleting repositories or user work. |
| Cancel operation | Stop its agent/child processes, handle owned temporary resources, and retain the reason and completed records. |

Stopping containers and resetting data are different operations. The default removal scope of `docker compose down` does not include all persistent data. [S3] Before resetting, check the project/volume/path against platform records. Do not use host-wide pruning or `reset --hard` on user directories.

General feature/bug exercises evaluate against clean fixtures. This does not imply first-version support for uninterrupted upgrades, migration, rolling updates, or failures across physical hosts.

## 10. Data and state: necessary execution records only

SQLite is proposed to retain three kinds of records:

| Record | Purpose |
|---|---|
| ExerciseVersion | Exercise identity, published version, type, learner GitHub repository/fixed starting SHA, private package GitHub commit/path, and ready/withdrawn status. Unpublished drafts may remain Run outputs. |
| Submission | Exercise version, GitHub repository, full submitted SHA, branch label at confirmation (optional), and optional notes. Reruns for the same exercise/repository/SHA may reuse the submission while adding run records. No PR field. |
| Run | Operation, target, start/end, execution status, reports, and actual resource references. Author/Reviewer invocation records for one operation may live in its Run directory. |

The active exercise and environment identity can be a small amount of application configuration, without separate practice-session or Project management services.

GitHub stores platform, learner, and private exercise code revisions. SQLite stores references, relationships, and operation outcomes. Controlled local directories contain execution copies, raw logs, tests, and review reports. Do not turn local checkouts into another code source of truth or build a self-hosted mirror/bundle storage service. Sources needed for reruns should remain retrievable from the exercise's GitHub repository. If unavailable, report that honestly; keep existing reports without promising source reconstruction. SQLite and necessary reports still need backups; GitHub code storage does not back up local run records.

Minimum execution statuses: running, finished, error, cancelled, interrupted. These are operation records, not Author reasoning states. Behavioral results are separate: passed, failed, inconclusive, or not-run, with clear causes for invalid exercises, test errors, and environment errors. A completed run is not a passed exercise/submission.

Bind every evaluation result to the exercise version, GitHub repository, source SHA, recipe/fixture/check identities, actual image ID, build platform, and Run ID. The branch is only a historical source label. Do not overwrite failures for the same SHA so that only the final successful run remains.

The controller does not automatically continue work after restart. Mark unconfirmed work interrupted, inspect remaining platform-owned processes/containers, and handle them or request explicit cleanup before accepting conflicting operations. Do not assume the agent finished or repeat remote creation/data deletion. This is basic failure handling, not a durable workflow.

## 11. Minimum safety and data boundaries

The protection goal is to reduce accidental damage, credential exposure, and spoilers, not to build a malicious multi-tenant or anti-cheating platform.

Platform-development guidance, private Author guidance, and learner-repository guidance serve three purposes; do not copy them directly between contexts. The Publisher exports explicit content rather than relying only on .gitignore to exclude answers. GitHub storage still requires separation between private authoring scope and learner repositories. GitHub credentials belong only in controlled source operations, not tested code or the Reviewer's execution area.

Platform agents do not inherit global user memories, broad MCP access, or other-project access settings. Start the Reviewer with platform-controlled context and treat reviewed sources as data. Repository instructions cannot elevate tool permissions or change the evaluation contract. Test isolation; changing directories or sessions alone does not establish safety.

Tested build/test/application containers receive no LLM key, Git token, SSH agent, Docker socket, or home directory. Codex documentation warns that repository build/test processes in the same environment may read authentication environment variables. Setting a key for one command does not remove child-process inheritance. [S1]

Agent experiments enter a separate execution area through approved operations. Formal evaluation also runs without private credentials. Newly generated AI test code remains executable code; labeling it a test does not permit direct execution on a privileged host.

Limit cleanup, paths, and command arguments to an explicit run/exercise scope. Use argv, handle traversal and symlinks, and do not concatenate user text into shell commands. Bound timeouts, log size, and resource use.

The control UI requires protected access, Host/Origin checks, and safe text rendering; no unauthenticated public control API. If the user's full-access AI runs under an OS identity that can read private answers, promise only that the learner repository excludes answers, not that the AI can never see them.

## 12. Minimal UI and control interface

The thin UI can be one page with three areas:

**Exercise list/generation:** Enter the current direction, generate an exercise, and view available exercises. No Project onboarding, model marketplace, capability graph, or persistent AI chat.

**Current practice:** Task, GitHub repository/clone/application entry points, current deployment and environment status, start/stop/reset. The submission area displays the exercise repository, allows branch/commit selection, shows the full SHA and brief commit information, and asks for deployment/evaluation confirmation. No PR input, PR list, or merge button.

**Executions/results:** Repository, branch label, actual evaluated SHA, test results, readable/downloadable engineering review reports, past runs, and failure reasons. No inline comments, diff-review UI, or comment-resolution state. Raw private Author output is absent from ordinary practice screens; no satisfaction, capability scores, or post-use questionnaires.

CLI and UI invoke the same internal actions. The API may expose only these operation groups rather than a general workflow API:

| Operation group | Necessary capabilities |
|---|---|
| Exercises | List/read, trigger generation, explicitly retry publication checks for a fixed draft. |
| Environment | Read actual status, start, stop, reset, end practice. |
| Submissions | Read this exercise repository's branches/candidate commits, confirm/submit a full SHA, read/download reports, explicitly rerun the same submission. |
| Executions | Read spoiler-filtered status/results and cancel running work. |

UI polling is sufficient; event streaming is not required. Browser closure does not cancel server work. Server shutdown uses interruption handling, not a promise of transparent recovery. Command and button names may change during implementation; do not preemptively turn them into a dynamic action framework.

## 13. Adjusting the Author without changing the whole system

Primary editable material: the background brief, local-exploration guidance, selection criteria, good/bad examples, task-type methods, reproduction/testing techniques, Reviewer rules, and report templates.

Preserve input snapshots or revisions for each authoring run so the rules used for an exercise can be identified. Do not create a new experimentation platform, metrics dashboard, learning memory, or automatic prompt optimization.

Shallow direction or poor selection can be addressed in skills/prompts/background. New container topologies, data migrations, or verification capabilities require changes to platform recipes or runners. Not every new Author requirement is prompt-only.

The shared interface is only the ability to deliver a draft in the chosen format and produce verifiable results in an approved environment. The platform does not depend on candidate count, first file read, internal tool rounds, or selection method.

## 14. Suggested first implementation slice

Select a baseline and fixed revision, complete a source build, demonstrate an observable small change, recreate fixtures, and write operating notes. This is development preparation, not an onboarding product.

Then interleave two activities using one exercise-package format: let a real Author produce a candidate, and use fixed exercise material to exercise GitHub publication, clean cloning, deployment, UI branch/SHA confirmation, testing, reporting, and reset. Known fixtures can test the platform, but do not indefinitely defer AI authoring as a prompt to add later.

The smallest complete path is:

```text
Generate an exercise → Review and verify final GitHub publication → Select/start → Clone → Edit/push
→ Confirm repository/branch/full SHA in UI → Fetch/build/deploy/test that revision
→ One-off engineering review report → Inspect/retry/reset
```

Connect the same operations to the thin UI. The Author can improve alongside this; all task types need not be designed first. Prioritize one runnable bug type before small feature exercises. Performance, incident, migration, and refactoring tasks are not implicitly supported.

Minimum completion checks: incorrect submissions fail for the intended reason; reasonable fixes pass; valid alternatives are not forced to match the reference answer; pushes after confirmation or during evaluation do not change the run's SHA; stop rather than substitute when the confirmed SHA is unavailable; zero/all-skipped tests do not pass; reset leaves user files untouched; interruption does not automatically repeat destructive operations; publication/build contexts contain no answers or credentials; no PR is required anywhere, and review reports do not depend on inline comments or GitHub review APIs.

## 15. Boundaries for the Codex handoff

This document does not mean the user's Mac mini has been operated, a GitHub repository created, or privileged resource changes authorized. Implementation follows the user's authorization and environment permissions at that time.

First inspect the working directory, existing changes, and tools, confirm the baseline can run, and then implement the vertical slice above. Do not reintroduce onboarding, Jev, Consult, PR integration, inline comments, long-session recovery, or learning-feedback systems. Do not build multi-provider abstractions or self-hosted Git merely for completeness.

Confirm during implementation: baseline and SHA; host architecture/resources; Codex version, authentication, and isolation; platform/learner GitHub namespaces, private-data locations, and permissions. GitHub is selected, so do not ask which Git platform to use again. Do not reopen PRs, inline comments, built-in AI chat, or user assessment.

## 16. Sources and evidence boundaries

**User sources:** The v0.3 draft, its earlier v0.2 handoff in the user-provided pasted-text attachment, and the confirmed decisions to exclude Jev, prepare fixed-repository exploration in advance, use one task/session at a time, and settle the architecture while adjusting the Author gradually.

**Basis for v0.4 revisions:** The user explicitly requested no PRs, GitHub code storage, UI confirmation of SHA/branch, platform retrieval and deployment/testing of that code, and report-only code review without inline comments. These supersede v0.3 PR entry points, data fields, and related sources.

**Proposals and boundary clarifications in this version:** Evaluate only the confirmed SHA; bind exercises to repositories; optionally store private exercise material with a private platform repository; separate source copies from run reports; do not replace GitHub with another source-storage service; allow file/line references without comment management. These implement the requested direction as design proposals, not execution results. Other v0.3 proposals retain their proposed/unverified status.

**Technical sources retained from v0.3** (original verification date recorded as 2026-10-14; v0.4 is an architecture revision without renewed web verification or local testing):

- [S1] OpenAI, Non-interactive mode: one-off `codex exec`, JSONL/schema output, authentication, and the credential boundary around repository code. `https://learn.chatgpt.com/docs/non-interactive-mode`
- [S2] Docker, Specify a project name: distinguishing multiple Compose environments by name. `https://docs.docker.com/compose/how-tos/project-name/`
- [S3] Docker, docker compose down: removal scope for containers, networks, and volumes. `https://docs.docker.com/reference/cli/docker/compose/down/`

The v0.3 `gh pr view` source was removed because PRs are no longer a system entry point or integration requirement.

This revision only updates architecture documentation. It has not created/uploaded/modified GitHub repositories, operated the user's host, run builds, validated exercises, measured Author success rates, or confirmed host isolation. "GitHub storage" is an architecture decision, not a claim that code has already been uploaded.

**Final principle: GitHub stores code revisions; the user confirms the SHA to run; the platform deploys, tests, and delivers a report. The Author remains a manually adjustable one-off task, without another PR workflow or agent-management platform.**
