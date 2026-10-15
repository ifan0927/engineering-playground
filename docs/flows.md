# System flows

**Read when:** Implementing authoring, publication, practice, revision confirmation, deployment/evaluation, or environment operations.

These are product execution semantics, not development order, milestones, or a progress plan. Consolidated from v0.4 sections 3, 5–10, and 12. Exact buttons, data fields, and filenames may change during implementation.

## Baseline preparation: a prerequisite, not onboarding

After selecting the first repository, pin its revision during development preparation and verify source builds, required dependencies, initialization data, and an observable small code change. Organize exploration findings into an operating index with code locations, confirmed flows, and limitations.

This preparation does not require in-product scanning states, a Project configuration wizard, or arbitrary-repository onboarding. Start each exercise from the same known base. Manually recheck baseline upgrades; do not automatically follow upstream or carry an earlier exercise's solution or fault into the next one.

Operating notes help the Author navigate; they do not replace checking the code relevant to the exercise. Do not invent available commands, paths, resources, or versions.

## Prepare an exercise

```text
The user explicitly requests an exercise
  → Prepare the fixed baseline, private workspace, background, and request
  → A fresh Author session produces candidate exercise content
  → Check the content and permitted execution scope
  → Assemble the learner repository that will actually be delivered
  → Rebuild and reproduce from that revision; verify the reference solution
  → A fresh Reviewer session independently reviews the exercise
  → Save GitHub revision references and make the exercise available
```

Within this session, the Author may repeatedly search, experiment, edit, and test. Do not model those internal steps as platform states or prescribe candidate counts, tool-call counts, or separate design and implementation sessions.

The Author saying "done" is only an invocation result, not permission to publish. If required material, actual execution evidence, or a basis for review is missing, preserve the candidate and reason, then stop. Do not automatically negotiate between Author and Reviewer until they agree.

Start a new task to revise an exercise only when the user explicitly requests it, using the previous draft and report as input. A tool-only recheck can create another run against the same fixed content without invoking the Author again.

## Publication checks

The Publisher exports only learner content and creates a learner repository without authoring or answer history. Switching branches or using a hidden directory does not establish this publication boundary. Keep upstream attribution, necessary documentation, and license material; do not present upstream code as original work.

If upstream uses submodules, LFS, Git-derived version generation, or special build resources, handle them explicitly in this baseline's recipe. Do not assume an export is complete. Check existing automation so a practice push does not accidentally trigger a product release.

**Verify the exact repository the user will clone.** Do not test only the Author's private workspace. Apply the reference solution to another clean copy with separate data; do not deploy the answer and then deliver the same environment as the initial exercise.

| Exercise type | Behavioral comparison to establish |
|---|---|
| Controlled bug exercise | The known-good baseline behaves correctly under declared conditions; the initial exercise exhibits the specified problem; the reference solution restores the target behavior and passes necessary regression checks. |
| Feature exercise | The initial revision preserves required existing behavior but does not yet meet the new requirements; the reference implementation satisfies both. |

A target-behavior failure, build failure, broken test program, and unavailable dependency are different outcomes. A nonzero exit code alone does not prove that the intended fault was created.

Learner code and published private exercise code/tests need GitHub revision references. Creating a candidate remote is not successful publication. Preserve the location already created and check it before retrying rather than creating duplicate repositories.

## Start and continue practice

The user selects an available exercise version. The platform provides an environment using that version's source, recipe, and initial data, and displays the task, repository, starting revision, application entry point, and current deployment information.

The user investigates, edits, debugs, and tests with their own IDE/coding agent, then commits and pushes to GitHub. The platform does not monitor uncommitted local content, manage personal AI conversations, require a PR, or require merging into main first.

Personal development directories and platform environments use separate mutable data. The platform does not mount source that the user is actively editing, avoiding confusion between a submitted revision and a live working directory. Practice may span multiple days and is independent of one-off platform-agent sessions.

## Confirm the revision

The exercise already identifies a learner repository. The UI displays it, lets the user choose a branch or specify a retrievable commit, and presents the full SHA and brief commit information for confirmation.

The deploy-and-evaluate request contains the exercise version, repository, and SHA confirmed on screen. The branch is only the source label at that moment; there is no need for a separate confirmation-workflow entity.

If the user confirms commit A and the branch later moves to commit B, evaluate A. Stop if A cannot be retrieved, the source does not match, or permission is missing. Do not silently evaluate the latest revision instead. Unpushed changes are outside evaluation scope.

## Deploy, test, and report

```text
Confirm exercise version + GitHub repository + full SHA
  → Acquire the execution slot and record the source
  → Fetch into a clean workspace and verify the actual checkout HEAD
  → Build in a restricted environment using the established recipe
  → Record the actual artifact and build conditions
  → Preserve necessary observations and reset the declared evaluation data
  → Deploy this artifact and inspect environment status
  → Run the platform's fixed checks
  → A fresh Reviewer session reads requirements, diff, and results
  → Display test results and an engineering review report
```

For the user, this is one confirmation, not multiple approval layers. The source must still actually be built; a floating official image is not the artifact for the specified submission.

If the new build fails, do not first dismantle the previous environment while it remains useful for observation. Explain which data deployment/evaluation will recreate before the operation. Once deployment changes, immediately record the attempted/actual revision and observations rather than waiting for every test to pass before updating the UI.

Fixed checks come from the exercise version. An edited task description, deleted tests, or a handwritten PASS in the learner repository does not change the criteria. User-added tests may inform review but do not replace the fixed checks.

The Reviewer compares the same exercise's starting revision with the submitted SHA, without relying on a PR or main's merge base. Accept alternative solutions that meet public requirements; the reference patch is not the only answer.

A report must identify the evaluated revision, actual execution results, and evidence supporting its opinions. This package does not freeze exact sections or file formats. File paths/line numbers can appear directly in the report without inline comments or individual-comment management.

Present fixed-test results separately from model review. A failed review invocation does not erase test evidence, and an unverified concern is not a confirmed defect. The platform does not automatically edit user code, merge, change the grader, or write capability assessments.

## Environment operation semantics

| Operation | Effect |
|---|---|
| Start | Initially deploy the exercise's starting revision. If stopped, restart the recorded current revision without silently resetting data. |
| Stop | Stop the specified environment while preserving code, reports, and the agreed data. |
| Reset exercise | Restore only the platform-owned initial exercise source/artifact and data, without touching user repositories or uncommitted changes. |
| Deploy and evaluate | Build the confirmed SHA, then recreate the agreed evaluation data and test after a successful build. |
| End practice | Clean up the explicitly identified exercise environment, retain code/reports, and clear the active-exercise reference. |
| Cancel | Handle this task's agent, child processes, and resources; preserve obtained results and the reason for incomplete work. |

Stopping a container and recreating initial data are different operations. See [Runtime and safety boundaries](runtime-safety.md) for ownership, credentials, and cleanup limits.

## Failure and interruption

Rerunning the same SHA is a new execution, not an overwrite of the previous failure. Distinguish missing sources, build/deployment errors, broken tests, and invalid exercises where possible. If the cause cannot be determined, preserve the reason rather than automatically marking the user's solution wrong.

After controller restart, do not resume the agent midway. Inspect remaining processes and environments, mark unconfirmed work as interrupted, and require an explicit rerun or cleanup. Do not blindly recreate remotes, reset data, or publish twice.

These operation/result records keep the product from misleading the user. They are not a development-progress system or a general-purpose durable workflow.
