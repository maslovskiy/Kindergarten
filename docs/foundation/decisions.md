# Decision register

One line per decision. A decision lives here **or** in an ADR (`/docs/adr/`), which this register links. Other documents reference decisions by ID only.

Statuses: `draft` (proposed), `accepted` (owner explicitly agreed), `needs_review`, `deferred`. Only the owner's explicit agreement makes a decision `accepted`.

| ID | Decision | Status | Stage | Source / rationale |
| --- | --- | --- | --- | --- |
| D-000 | Process rules from the kickoff brief: one stage at a time; agents draft and only the owner accepts; silence is not approval; repo docs are the source of truth; conflicts with accepted decisions need explicit owner agreement; no screens, production code, or migrations during foundation work | accepted | S0 | Stated by owner in kickoff brief, 2026-10-08 |
| D-001 | Doc layout: `docs/foundation/` for process docs (README, product-context, roadmap, decisions, open-questions, stages/, research/, milestones, handoff). **Glossary stays at `/GLOSSARY.md` and ADRs at `/docs/adr/`**, per the repo convention (`CLAUDE.md`, `docs/agents/domain.md`) rather than `docs/foundation/glossary.md` | draft | S0 | Repo convention and installed `domain-modeling` skill already define these paths. Following them lets every skill find them with no extra instructions |
| D-002 | Discovery uses `grilling` + `domain-modeling` per stage (what `/grill-with-docs` invokes), `/research` for AFK research, and `/prototype` for spikes. Implementation later uses `/to-spec` → `/to-tickets` → `/implement` | draft | S0 | Matches the installed skills' main flow |
| D-003 | Question format: one focused question per message, with trade-offs, a recommendation, and the remaining frontier listed underneath so the owner sees what is coming | draft | S0 | The `grilling` skill asks a whole frontier per round, but the owner asked for one question at a time. This keeps the owner's pace and the skill's frontier tracking |
| D-004 | Track discovery in repo docs, not as a `/wayfinder` map on GitHub issues. Reuse wayfinder's ideas: destination, a frontier of decision-shaped stages, fog listed as open questions, out-of-scope list. GitHub issues are used from S12 for implementation tickets | draft | S0 | Owner wants repo docs as the source of truth. One tracker avoids two drifting copies |
| D-005 | Session handoff lives in `docs/foundation/handoff.md` in the repo, not in `/tmp` as the `/handoff` skill does | draft | S0 | Cloud containers are ephemeral; `/tmp` does not survive between sessions |
| D-006 | Decisions go in this register; an ADR is written only when a decision is hard to reverse, surprising without context, **and** the result of a real trade-off | draft | S0 | `domain-modeling` ADR criteria; keeps ADRs meaningful |
