# Platform agent guidance

**Read when:** Adjusting Author/Reviewer context, prompts, skills, exercise quality, or review behavior.

This document consolidates confirmed role boundaries and methods that remain adjustable. It is not an installed skill, fixed system prompt, formal output schema, or deliverable format for development agents. Roles follow v0.4; quality criteria follow the earlier handoff and the Author discussion. See [refs](../refs/README.md) for provenance.

## Three different uses

| Use | Context and task |
|---|---|
| Platform development agent | Reads the root AGENTS.md and task-relevant documents to help develop Playground. |
| Platform Author/Reviewer | Started by the controller for each task with a fresh session, specified workspace, and data to author or review. |
| User's exercise-solving agent | Chosen by the user; reads learner code, public requirements, and operating guidance without being managed by the platform. |

Do not copy these contexts directly between roles. In particular, platform design, root causes, private tests, and authoring traces do not belong in the learner repository. An exercise-review session must not continue as a submission-review session.

## Author responsibilities

The Author is an engineering task designer, not a random code-breaking tool. Given manually supplied background/direction, a fixed baseline, an available environment, and current quality criteria, it prepares exercises that can actually be operated and verified.

Its main inputs are the prepared project operating index, user brief, current request, and environment/resource limits. The index need not cover the entire repository. The Author follows the exercise-relevant flow into code, tests, and observed behavior.

Use only user-stated or explicitly revised background. Do not diagnose capability, infer mastery from one submission, or depend on a long-term-memory product to update a profile automatically. See [author-brief.md](../refs/author-brief.md) for background seeds; it is not required reading for every task.

One session can handle exploration, candidate selection, preparation, testing, and corrections within the task. These are working methods, not multiple agents, platform states, or mandatory intermediate documents.

## Current criteria for good exercises

| Criterion | Practical focus |
|---|---|
| Fits the request | Matches the user's stated direction, rather than breaking code first and attaching a rationale afterward. |
| Grounded in a real mechanism | Supported by actual codebase flows and state, not an unrelated puzzle added for the exercise. |
| Engineering depth | Requires understanding causality, boundaries, or tradeoffs and proposing a verifiable correction; not measured by file counts, diff size, or how slowly an AI solves it. |
| Focused and investigable | The user can obtain reasonable code, data, or execution evidence without guessing the author's intended answer. |
| Reproducible and fairly evaluated | The exercise is demonstrably valid, requirements are clear enough, and different reasonable solutions are accepted. |

These are manually adjustable design criteria, not research findings about learning outcomes. They do not require a quantitative quality score or automatic assessment of the user.

## Author versus ordinary code

The Author produces the scenario, exercise content, reference solution, and verification method. The Verifier executes checks; the controller verifies sources and publication content; the Reviewer challenges consistency between requirements and evidence.

A model claiming completion or writing `passed=true` is not a pass. If a requirement exceeds established recipes or permissions, stop and identify the gap instead of escalating host permissions or creating external services.

After publication, do not automatically add problems, raise difficulty, rewrite the task, or change checks. A user-requested revision becomes new work and a distinguishable version.

## Reviewer: two one-off tasks

**Exercise review:** Check consistency among public requirements, observability, the initial fault/feature gap, fixed tests, and reference solution. Support blocking findings with evidence; do not accept an exercise based on the Author's self-assessment.

**Submission review:** Start from fixed public requirements, the submitted SHA, changes relative to the starting revision, and actual evaluation results. Judge independently first and consult reference material when needed. The Author's patch is not the only valid solution.

Separate confirmed requirement violations, unverified concerns, and nonblocking design/style suggestions. The Reviewer may identify a faulty task description or grader rather than attributing every failure to the user.

The Reviewer does not edit the user's branch, merge, change fixed tests, overwrite original reports, or post GitHub comments. Reports may include file paths/line numbers without a per-comment lifecycle.

When supplementary reproduction is needed, test the same source only within the approved temporary execution scope. New findings must not silently introduce criteria absent from the original exercise.

## Distributing guidance

A role's entry point contains only its essential mission, boundaries, and relevant index. Reusable methods and a few good/bad examples may live in a skill or reference document. The invocation prompt describes only the current task and special constraints.

A skill is an option for organizing reusable methods, not a reason to split the work into many role skills. This package does not prescribe runtime-specific installation paths, automatically create `.agents/`, or claim arbitrary documents are loaded automatically.

Author methods can improve incrementally. There is no requirement for two sessions, exactly three candidates, mandatory TASK_DESIGN files, numerical scores, or an external reranker. Earlier ideas remain [optional design notes](../refs/author-design-notes.md).

If exercises are shallow, adjust methods and examples. If the Author finds the wrong entry point, fix the operating index. If initialization repeatedly fails, fix environment scripts. Not every problem is solved by lengthening the prompt.

## No new feedback system

Keep necessary input revisions and execution evidence for manual investigation. If the tool is not useful, the user directly requests changes to the brief, rules, or code. Do not add curricula, learning analytics, self-assessment, questionnaires, or automatic review feedback loops.

The Author framework, exploration strategy, and exercise effectiveness have not been tested by this package. Changes must preserve source, evaluation, and permission boundaries, without prescribing a plan or fixed report before every development task.
