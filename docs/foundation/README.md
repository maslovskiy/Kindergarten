# Foundation: entry point

Discovery and architecture work for a multi-tenant SaaS platform for private kindergartens. This folder is the **source of truth** between sessions and agents. Do not rely on chat history.

## Current state

- **Active stage:** S0 Process and documentation setup (`in_progress`)
- **Roadmap status:** `draft`, awaiting owner review
- **Next action:** see [handoff.md](handoff.md)

## Reading order for a new session

1. This file
2. [handoff.md](handoff.md): exact next step and any half-finished question
3. [roadmap.md](roadmap.md): stages, dependencies, status
4. [`/GLOSSARY.md`](../../GLOSSARY.md): canonical terms (created lazily once the first term is accepted)
5. [decisions.md](decisions.md): decision register (ADRs in [`/docs/adr/`](../adr/))
6. [open-questions.md](open-questions.md)
7. The `stages/` document for the active stage
8. [product-context.md](product-context.md) when you need the original brief

If something you need is missing, say what is missing. Do not invent it.

## Files

| File | Holds |
| --- | --- |
| `README.md` | Entry point, current state |
| `product-context.md` | The owner's brief: stated requirements, constraints, future capabilities, plus agent-observed tensions |
| `roadmap.md` | Discovery stages, research items, spikes, dependencies, statuses |
| `decisions.md` | Decision register (one line per decision, with status). Large decisions link to an ADR |
| `open-questions.md` | Unresolved questions, each tagged with its target stage |
| `stages/SNN-<slug>.md` | Detailed outcome of one stage (created when the stage starts) |
| `research/` | Output of `/research` items (cited, primary sources) |
| `milestones.md` | Implementation milestones (created in S12) |
| `handoff.md` | Session log and next step |
| `/GLOSSARY.md` | Canonical domain language (repo convention, see `docs/agents/domain.md`) |
| `/docs/adr/` | ADRs for decisions that are hard to reverse, surprising, and a real trade-off |

A decision lives in **one** place (its register entry or ADR). Everything else references it by ID (`D-012`, `ADR-0003`).

## Statuses

`not_started`, `in_progress`, `draft`, `accepted`, `blocked`, `needs_review`, `deferred`.

An agent may **draft**. Only the owner's explicit agreement makes something **accepted**. Silence is not approval.

## Levels of confidence

Keep these distinct in every document:

1. **Proposed design**: an agent's or owner's proposal (`draft`).
2. **Accepted decision**: owner explicitly agreed (`accepted`).
3. **Technically validated**: a spike or research item produced evidence (link it).
4. **Implemented and tested**: exists in code with passing tests.

## How a session runs

- **Start:** read the files above in order and summarize: what is decided, what this session decides, which stages depend on it, and which missing decisions could block progress.
- **During:** grill using the `grilling` and `domain-modeling` skills (see D-003 for the question format). Update `GLOSSARY.md` inline as terms resolve. Record decisions in `decisions.md` as they are agreed.
- **Conflicts:** if a new decision conflicts with an accepted one, name it, explain the impact, propose a revision, mark affected stages `needs_review`, and get the owner's agreement before replacing it.
- **End:** update the stage doc, `decisions.md`, `open-questions.md`, `roadmap.md` statuses, and `handoff.md` (including the next unanswered question if the stage is unfinished).
