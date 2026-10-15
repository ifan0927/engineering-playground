# Documentation index

**Purpose:** Find the information needed for the current task, not a mandatory reading list for every session.

The root [AGENTS.md](../AGENTS.md) is the shortest entry point. If the area to change is already known, read its relevant sections and code directly. Otherwise, use the table below and search from there. Expand reading when cross-module evidence is needed; it is not limited to a single document.

## Read by task

| Current task | Primary document | Add only when needed |
|---|---|---|
| Product purpose, scope changes, or technology decisions | [product.md](product.md) | [architecture.md](architecture.md) |
| Controller, module boundaries, or data storage | [architecture.md](architecture.md) | [flows.md](flows.md), [runtime-safety.md](runtime-safety.md) |
| Authoring through publication, or exercise versions | [flows.md](flows.md) | [agents.md](agents.md), [runtime-safety.md](runtime-safety.md) |
| Branch/SHA confirmation, deployment, or evaluation | [flows.md](flows.md) | [runtime-safety.md](runtime-safety.md) |
| Author quality, prompts/skills, or Reviewer behavior | [agents.md](agents.md) | [Author background](../refs/author-brief.md), [design notes](../refs/author-design-notes.md) |
| UI, environment operations, or report presentation | [architecture.md](architecture.md) | [flows.md](flows.md) |
| Implementation, bug fixes, tests, or dependency changes | [development.md](development.md) | Affected topics and actual code/tests |
| Overlong guidance, missing information, or conflicting documents | [context.md](context.md) | [Reference index](../refs/README.md) |
| Third-party code execution, Git publication, secrets, or cleanup | [runtime-safety.md](runtime-safety.md) | [flows.md](flows.md) |
| Trace original v0.4 proposals and revisions | [Reference index](../refs/README.md) | Only the relevant historical sections |

## Document responsibilities

`product.md` owns confirmed product boundaries and unconfirmed technology choices. `architecture.md` and `flows.md` describe the current design. `agents.md` describes platform-agent roles and authoring principles; it is not an installed skill.

`development.md` and `context.md` contain the development conventions introduced by this starter package. `runtime-safety.md` consolidates existing version, evaluation, and execution boundaries. Reorganizing material does not turn every old proposal into a user decision.

`refs/` retains sources, background seeds, and exploratory proposals. It is neither a backlog nor a progress tracker, and it is not an alternative entry point that overrides current rules.

## When implementation is missing

This package contains no application, schema, build scripts, or ready-to-use execution environment. Do not assume undocumented paths, commands, dependency versions, or deployment addresses exist. Obtain facts from the actual working repository before adding them to the appropriate document.

Confirm requirements and design from current documentation, implemented behavior from code and tests, and execution state from tools and logs. When they disagree, identify the difference rather than silently using one to validate another.

See [refs/README.md](../refs/README.md) for sources and the authority of historical material.
