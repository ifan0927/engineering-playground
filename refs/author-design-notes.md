# Optional Author design notes

**Status:** Design ideas proposed in the discussion, available when improving the Author. Not a confirmed fixed workflow, skill, schema, or development backlog. No exercise must use every idea.

## Settled premises

Use one prepared baseline and a fresh session for each Author task. Adjust behavior primarily through role guidance, a brief, on-demand references, and a few scripts. Do not add Jev, a general exploration engine, or long-running agent management.

"Exploration/design" and "preparation/verification" may be different thinking activities within the same task; they do not imply two sessions or two agents. The platform needs runnable, inspectable results, not monitoring of internal reasoning stages.

## Local exploration

The operating index supplies the revision, build/test information, module entry points, and major flows. Given the current direction, the Author traces entry points, relevant state, update paths, observable behavior, and verification methods.

The number of files read is not a completion condition. If the core mechanism remains unclear, follow the source or run a controlled experiment rather than filling gaps with a long summary. Distinguish documentation claims, code-based inferences, actual observations, and unresolved questions.

## Candidates and selection

Compare a few candidates to identify one that fits the brief, has codebase evidence, can be reproduced in the current environment, and is worth investigating. Candidate count and recording format are not fixed; there is no need to invent three options or request approval for each one.

Previously discussed sources include controlled defects, adaptations of real issues/historical changes, and reasonable feature extensions. These are options, not a requirement to search online for an issue every time or a claim that all exercise types are supported by the environment.

If a new feature introduces a regression, evaluation should preserve both the feature and the original requirements. Completely reverting the feature must not count as a successful fix merely because the regression disappears.

## Concrete questions about depth

Consider what mechanism the exercise actually requires understanding, what plausible change might merely hide a symptom, and which observations distinguish a genuine correction from a lucky run.

Not every exercise needs several wrong solutions or multiple constraints, and these questions need not become a mandatory form. The purpose is a concrete selection rationale rather than a vague claim of "improving debugging skills."

## Hypothetical example: configuration and background scheduling

This scenario is for discussion. It does not claim that HertzBeat or any selected baseline has been inspected.

The public scenario might be that configuration queries show a new frequency while background collection continues with the old behavior. The Author must first confirm that persistent configuration and active runtime state really have different lifecycles in the code, rather than imposing the story on it.

Leave the root cause for the user to investigate, but explain when an update should take effect, constraints on existing work, and existing behavior that must remain intact. The cause may be unknown; evaluation criteria must not depend on guessing.

Checks can observe whether behavior updates, duplicate tasks remain, or other settings are affected, rather than demanding a particular function call. Where useful, try the most likely false-positive fix; do not require a complete collection of wrong answers for every exercise.

## Useful design notes

When a task needs them, record selection rationale, code locations, public requirements, verification methods, and unknowns. `TASK_DESIGN.md` was an example filename, not a mandatory intermediate document or a request for the model's full internal reasoning.

Execution evidence must still come from the delivered learner revision, with the Reviewer checking requirements against evaluation. Author self-assessment, candidate count, and note length do not replace engineering verification.

## Improve without adding responsibilities

Manually edit exploration methods, the brief, examples, and prompts directly. Unless there is an explicit and necessary capability gap, do not build model routing, a vector database, a scoring platform, session recovery, or an automatic iteration service around them.

These notes are optional approaches, not evidence that Author quality is solved. Let the current task determine the method; do not turn them into a curriculum, development-progress system, or deliverable format.
