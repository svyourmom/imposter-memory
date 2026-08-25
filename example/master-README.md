Example master index. In a real tree this file sits at the root as `README.md`.
Everything below is fictional.

---

```markdown
---
type: master
intake_dir: incoming/
living_dirs: [status/, memory/]
evidence_dir: artifacts/
---

# Vault

<!-- CORE-MEMORY:START (generated — do not hand-edit) -->
## Right now

> Last synced: 2026-03-11 · Last audit: 2026-03-04

- **Top priority:** Renew the Trix access credentials — hard external deadline
  in four days, blocks delivery if missed. Ranked above the Cheerios
  walkthrough because nothing else on any list has a fixed date this close.
- **Urgent:** Cheerios integration walkthrough committed for Thursday.
- **Active projects:** Cheerios, Trix, Fruit Loops
- **Next audit due:** 2026-03-18
<!-- CORE-MEMORY:END -->

This tree runs the system specified in `IMPOSTER-MEMORY.md`. The operating rules
are in `AGENTS.md` — read that first. This file is the *registry* and current
state; it is maintained by the collector role and the audit pass, not
hand-edited outside those runs.

## Registry

| Project | Path | Status | Last processed |
|---------|------|--------|----------------|
| Cheerios | `Projects/Cheerios/` | Active | 2026-03-11 |
| Trix | `Projects/Trix/` | Active | 2026-03-11 |
| Fruit Loops | `Projects/FruitLoops/` | Active | 2026-03-11 (empty intake) |
| Honeycomb | `Projects/Honeycomb/` | Archived | 2026-01-22 |

### Descriptions

- **Cheerios** — Integration delivery for the client's packaging pipeline.
- **Trix** — Platform access and credential management workstream.
- **Fruit Loops** — Scoping for a follow-on engagement; not yet committed.
- **Honeycomb** — Closed out January. Reference only; no agent runs for it.

New projects: copy the standard skeleton into `Projects/<name>/`, then add a
row above. It is not a project until that row exists.

## Master living notes

- `status/owner-action-items.md` — the consolidated task list
- `status/team-status.md` — cross-engagement situational summary
- `status/audit-log.md` — append-only audit history
- `memory/events.md` — dated commitments and deadlines
- `memory/people.md` — roster, roles, reporting lines
- `memory/who-is-doing-what.md` — current work assignments
- `memory/conventions.md` — ticketing and process conventions

## Intake

`incoming/` is the master drop-off. Empty as of the last run — all caught up.
```
