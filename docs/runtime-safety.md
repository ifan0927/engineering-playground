# Runtime and safety boundaries

**Read when:** Working on source retrieval, publication, builds/tests, deployment, data resets, cancellation, credentials, private answers, or result interpretation.

Basis: v0.4 sections 5–11 and their established risk boundaries. This document consolidates design constraints; it is not a completed security audit or a new general-purpose security platform. The goal is to prevent accidental damage, credential exposure, spoilers, and incorrect evaluation, not to support malicious multi-tenancy or anti-cheating.

## Confirmed source and actual artifact must match

Bind evaluation to the user-confirmed repository, full commit SHA, and exercise version. The branch describes the source; do not resolve it again during execution and replace the confirmed SHA. Stop if the source is unavailable.

Verify the actual source after checkout. Build, deployment, checks, and reports must refer to the same artifact from that run. Record an identifier for the actual artifact, such as an image ID on a single host, not just a floating tag. Relevant recipes, initialization data, and fixed checks must be traceable to the exercise version.

These records prevent version mix-ups; they are not tamper-proof attestation or a bit-for-bit rebuild guarantee. Changed exercise content creates a distinguishable version. Old verification does not apply to new content.

## Candidate code cannot rewrite fixed evaluation

Retrieve fixed requirements and checks from the platform's saved exercise version. An edited task description, removed tests, or a custom PASS file in the learner repository does not automatically change official criteria.

Private test cases may differ, but requirements cannot be hidden. Do not introduce multi-node behavior, migration, or a particular function implementation without a declared requirement. The reference solution is a valid approach, not the only implementation.

Confirm that tests actually ran and produced results. Zero tests, all-skipped tests, printing PASS, or a successful exit alone do not establish that the specified behavior passed.

Preserve original test evidence and failures. A Reviewer convincing itself is not a reason to turn a result green. Distinguish environment, test, exercise, and submission problems where possible. State uncertainty when the cause is unknown rather than inventing a classification.

## Separate authoring, solving, and evaluation material

Export only allowed learner content. Do not copy authoring `.git`, answer refs, reference solutions, private graders, agent homes, private logs, or credentials into learner repositories.

The Publisher uses an explicit allowlist of content and a final inspection. `.gitignore` and hidden directories are not answer-protection mechanisms. Builds receive only necessary source; do not use an ancestor directory containing answers or secrets as the build context.

This package itself is platform-development material, not the material delivered to a learner for each exercise. Separating private GitHub scope from learner repositories does not remove the need to control local file visibility.

## Platform-agent and tested-code permissions

Platform agents use explicit role workspaces, contexts, and enabled tools. They do not inherit all personal memories, broad MCP access, or other-project access. This constrains agents inside the product; it does not require changes to the user's own development-agent settings.

Tested builds, tests, and applications receive no LLM keys, Git tokens, SSH agents, Docker sockets, or entire home directories. Git credentials belong only in controlled source operations. Authentication needed by the agent itself must not leak into tested child processes.

Author experiments use approved recipes and restricted execution areas. Generated tests are executable code; calling them "tests" does not permit arbitrary execution on a privileged host.

Do not pass user text or model-generated shell/Compose configuration directly to the host. Fixed operations use explicit parameters, path boundaries, and argv. Handle path traversal, symlinks, and input pointing outside the allowed directories.

If privileged execution, host mounts/namespaces, external networks/volumes, or new service credentials exceed the recipe, stop and seek explicit handling. The model must not expand its own permissions.

## Cancellation, reset, and resource ownership

Each task has identifiable sources, temporary directories, and environments. Before cleanup, check project identifiers, labels, volumes, and real paths. Act only on platform-owned resources for that operation.

Do not substitute host-wide pruning, broad directory deletion, or resetting the user's workspace for an exercise reset. Stop, deploy, and data initialization have different meanings; see [System flows](flows.md) for their effects.

Timeouts, cancellation, logs, and resource use need enforceable limits. Do not promise hard token caps unsupported by the runtime. If processes or environments remain after cancellation, report that fact instead of claiming cleanup is complete.

After interruption, do not automatically repeat publication, remote creation, or data deletion. Inspect what remains before accepting another conflicting operation. This does not require a transparently recovering workflow system.

## UI, reports, and untrusted content

Expose control only locally or through protected access, not through an unauthenticated public host-control API. Handle state-changing operations and source/Origin checks correctly for the actual web stack.

Render logs and model-generated Markdown/text safely, not as trusted HTML or commands. Remove secrets and unintended spoilers from user-facing reports; do not expose the Author's raw editing process.

A reviewed repository, README, AGENTS file, comments, or execution output may contain instructions. The Reviewer treats them as data and must not allow them to change official requirements, tool permissions, or results.

## Do not overpromise

A fresh session, different working directory, or container name does not prove file-read isolation. Test the chosen runtime, OS identity, mounts, and tool scope to establish actual protection.

If the user's full-access agent can read local answers, promise only that the learner repository does not contain answers, not that the agent cannot obtain them. This is an honest statement of boundaries, not a reason to build an anti-cheating platform.

Multiple containers on one host and evaluation with clean data do not establish multi-host availability, uninterrupted migration, or production-scale behavior. Describe demos and outcomes using actual evidence; do not infer safety, performance, or availability from the design.
