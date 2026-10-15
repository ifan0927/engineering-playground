# Reference index and source authority

**Read when:** Tracing design decisions, checking background sources, or exploring Author methods further. This is not a preload list for every task.

## Usage rules

The root files and `docs/` are the current entry points. Material here is historical or supplementary. It does not require following old startup instructions, automatically become a backlog, or override the user's later explicit requests.

Statements such as "do this first," "deliver this," or "next phase" in historical documents describe proposals at that time. They are not instructions to the current development agent. Return to the current problem once the relevant passage is found; reading the entire historical document is unnecessary.

## References included

| File | Nature | When useful |
|---|---|---|
| [architecture-v0.4.md](architecture-v0.4.md) | Complete English translation of the user-provided v0.4 historical document, preserving its structure and design content. It is not a byte-identical copy of the original. Original date: 2026-10-14. | Tracing how specific data, flows, or safety decisions were originally described. |
| [author-brief.md](author-brief.md) | Attributed background seeds extracted from the earlier v0.2 handoff. Not a capability diagnosis or the current authoring request. | When the Author's personalization actually needs this background. |
| [author-design-notes.md](author-design-notes.md) | Exploratory methods proposed in the discussion, not adopted mandatory steps. | Improving selection, evidence, and local exploration. |

The full v0.2 document and repeated conversation history are not copied wholesale, avoiding competing instructions about superseded Consult, PR, feedback, and development-phase proposals. Still-relevant background and quality principles identify their sources in the corresponding documents.

## Explicit revisions in this package

This package was compiled on 2026-10-15. Source documents retain their original dates.

The user requested basic AGENTS.md, README.md, and related guidance. AGENTS.md must remain concise and index-like; retrieve data and references on demand, with agents finding information relevant to the current task. Do not prescribe deliverables, development plans, phases, or progress tracking in advance.

Accordingly, `docs/development.md` and `docs/context.md` introduce development conventions, while the other documents organize existing material by topic. No outside knowledge is used to add product features or promote technology candidates to decisions.

The subsequent language revision, dated 2026-10-15, translates the package's documentation and related guidance into English. The root AGENTS.md explicitly requires communication with Ifan in Traditional Chinese (zh-TW). The former documentation-language default in development guidance is replaced accordingly. This changes language policy, not product scope, architecture, or planning requirements.

The historical v0.4 document is also translated in full. Its translation notice identifies the change; descriptions that formerly called it an unmodified original now identify it as a translation. Its historical status and original source dates remain unchanged.

## Mapping v0.4 sections to current documents

| Original section | Current entry and authority |
|---|---|
| §0 Purpose and decisions | [product.md](../docs/product.md): preserves the distinction between confirmed scope and proposals. |
| §1–4 Overall system, deployment, preparation, responsibilities | [architecture.md](../docs/architecture.md), [flows.md](../docs/flows.md). |
| §5 Exercise package | [architecture.md](../docs/architecture.md): preserves public/private/version responsibilities; filenames and schemas do not become fixed package formats. |
| §6–8 Authoring, practice, submission | [flows.md](../docs/flows.md): product execution flows, not development order. |
| §9–11 Environment, records, safety | [flows.md](../docs/flows.md), [runtime-safety.md](../docs/runtime-safety.md): preserve engineering boundaries without creating a general state machine. |
| §12 UI | [architecture.md](../docs/architecture.md): retains operations without fixing page count or APIs. |
| §13 Author adjustments | [agents.md](../docs/agents.md): methods remain manually editable; no automatic optimization or learning feedback. |
| §14 First implementation slice | Historical reference only, not this package's development sequence, milestones, or mandatory checklist. Necessary behavioral boundaries are consolidated in the flow and safety documents. |
| §15 Codex handoff | Retains the principles of respecting environment authorization and not reopening confirmed requirements; does not adopt its default first-task deliverables or startup checklist. |
| §16 Sources | External links are references retained from earlier documents, not newly verified sources. |

## External technical material

OpenAI/Docker URLs from v0.4 remain unchanged. Their inclusion does not establish current CLI options, authentication behavior, Docker behavior, or local versions. When a task actually involves a tool, consult official material and the current execution environment on demand; do not browse every source merely to finish reading refs.

This package adds no external research, performance measurements, effort estimates, or learning-outcome conclusions. Feasibility, safety, and exercise-quality claims still require actual evidence.

## Supplementary source boundaries

`author-brief.md` draws on v0.2 §1.2 (relevant background) and §8.3 (quality criteria) in the user-provided pasted-text attachment. It retains only authoring-relevant information, not income, employer, family, personal accounts, or company code.

`author-design-notes.md` consolidates candidate formation, local exploration, and evidence-design proposals from the Author discussion. In light of the later one-task/one-session decision, it explicitly does not adopt multi-session stages, Jev, or automatic repair control flows.

This package does not include every original tool output, conversation memory, or earlier attachment. State the gap when original evidence beyond this synthesis is needed; do not use a summary as a substitute for an unavailable full source.
