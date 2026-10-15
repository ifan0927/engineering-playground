# Development principles

**Read when:** Implementing features, fixing bugs, testing, changing dependencies, or modifying cross-module behavior.

These are basic development conventions introduced by this package, not a schedule, fixed-step engine, or deliverable template. Rules unrelated to the current task do not create additional work.

## Focus on the current problem

Understand the request, existing implementation, and affected behavior. Investigate questions that documents, code, or tools can answer. Ask the user only when missing information would materially change the direction and cannot be established from available information.

Keep changes scoped to the current problem. Avoid unrelated refactoring or extension points with no present need. Replaceable boundaries do not require a general plugin system; do not build an abstraction layer for an unconfirmed second provider.

Do not make "write a plan," "maintain progress," or "update every document" mandatory prerequisites for each task. Provide planning or specific deliverables when the user asks for them.

## Understand and preserve the working environment

Check the actual repository, branch, working directory, and existing modifications. Do not confuse a tool sandbox with the user's Mac mini, or assume a local directory belongs to a connected remote host.

Distinguish existing user changes from this task's changes. Do not reset, delete untracked files, or overwrite content merely to obtain a clean workspace. Use an identifiable, bounded copy when source needs to be executed.

GitHub is the project's code-storage direction. Creating repositories, pushing, changing visibility, installing software, and operating environments still depend on current authorization and tool permissions; design documents do not grant that authority.

## Code and dependencies

Follow the actual codebase's language, naming, error handling, directory structure, and testing conventions. If an empty project has none, make simple, consistent choices based on confirmed technology. Do not assume this package has selected every framework.

Prefer clear responsibilities, direct control flow, and small testable boundaries. Do not mandate DDD, Clean Architecture, microservices, or a specific number of layers. Names and abstractions should reflect real needs.

Extract common logic when there is concrete duplication or a testing need, rather than creating a service for every concept. Before adding dependencies, check whether existing tools suffice and consider installation, execution, and maintenance costs.

Use the language's established formatter/linter. Document actual commands and settings after choosing the tools. Do not invent `make test`, npm scripts, or nonexistent directories.

Comments should explain non-obvious reasons, constraints, and tradeoffs rather than restating code. Write project documentation and maintained comments in English. Follow the root AGENTS.md language rule when communicating with Ifan: use Traditional Chinese (zh-TW). Preserve technical identifiers, commands, paths, and necessary source quotations; do not forcibly translate external names.

## Verification and errors

Choose verification based on the change's impact rather than always running everything or claiming completion from a successful exit alone. When fixing a defect, add or run a regression check for that behavior when the environment permits.

For external integrations, distinguish mocks, local tests, and actual integration results. If permissions, dependencies, or cost prevent testing, state the unverified scope. Do not replace missing evidence with an assumed pass.

Errors should connect to a source revision, operation, and readable cause. Cancellation and timeouts must actually stop managed work or disclose remaining problems, not merely change the UI to "cancelled."

Preserve enough results to investigate failures. Do not hide failures or rerun until green and then delete earlier failures. See [Runtime and safety boundaries](runtime-safety.md) for engineering evaluation and safety; these must not become a mandatory response table for development agents.

## Documentation and truthfulness

Update the relevant topic when stable information changes, such as operating commands, interface semantics, version requirements, or newly confirmed product decisions. Keep temporary logs, to-dos, and daily progress out of AGENTS.md.

Keep the root entry concise. Put detail in a topic with a clear purpose rather than copying it across documents. Fix links and outdated statements while preserving the distinction among confirmed, proposed, and unverified information.

Document length, artifact count, and agent self-assessment do not establish completion. Let the current user request and actual work determine the deliverable. Do not require plans, progress summaries, handoff forms, PR templates, or fixed sections.

## Publication and permissions

Do not put keys, personal settings, answers, authoring logs, or an entire home directory into a build context or learner repository. Preserve necessary attribution and license material for third-party sources; do not present imported open-source code as original work.

Before publication, data reset, container operations, or third-party code execution, read the relevant sections of [Runtime and safety boundaries](runtime-safety.md). "Personal use" and a reminder in a prompt do not replace basic safety.

See [Context and indexing](context.md) for on-demand retrieval and index maintenance.
