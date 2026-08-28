# AGENTS.md — Operating Contract

The rules for how an Imposter Memory tree is maintained. These are invariants —
no folder, project, or note overrides them.

This file is the *contract*. `IMPOSTER-MEMORY.md` is the full specification it
implements. The root `README.md` is the *registry* and current state. If the
contract and the spec ever disagree, the spec wins and the contract is a bug.

## The flow

Every level of the tree repeats the same three-state flow:

```
intake  →  living notes  →  immutable evidence
```

- **Intake** (`incoming/` at master, `Incoming/` inside a project) — transient
  drop-off. Raw material lands here unorganized and is processed out.
- **Living notes** — continuously rewritten; the current understanding.
  `status/` and `memory/` at master; the numbered topic files and
  `action-items.md` inside each project.
- **Evidence** (`artifacts/YYYY/MM/`) — append-only, never edited. The
  permanent record of what a source said when it arrived.

## Invariants

1. **Never edit evidence.** Files under `artifacts/` are immutable forever,
   including their internal citations. A corrected source is re-dropped into
   intake as a new arrival; the old copy stays as it was.
2. **Never invent facts, dates, or owners.** Where information is missing,
   write `TBD` or `[NEEDS INPUT]`. A blank is a fact about the source; a guess
   is a corruption indistinguishable from a real fact one week later.
3. **Cite the source.** Every extracted claim names the evidence file it came
   from, by bare filename — never a path — so reorganizing evidence storage
   never breaks a citation.
4. **One canonical home per fact.** Cross-reference rather than restating —
   except master task-list entries, which must stand alone (see invariant 7).
5. **Daily trackers and the audit log are append-only.** Never edit a prior
   entry.
6. **Intake is transient.** When a run finishes, an intake directory holds
   only material deliberately left unprocessed, and that material is named in
   a task list so the omission is visible rather than silent.
7. **Projects are self-contained.** No project may depend on a sibling or on
   the parent tree. A master record that names a project must carry the
   important fact itself, never merely point into a project file — this is
   what lets any project be archived or deleted without breaking anything
   outside it.
8. **Trust the registry, don't scan.** Project state comes from the
   `README.md` registry table, not from inferring it by looking at folders.
9. **Freshness is recorded.** Every living note carries a
   `> Last updated: YYYY-MM-DD` line beneath its title, updated whenever the
   file changes. Evidence files never carry one, because they never change.
10. **Task lists stay sorted.** Position in a `## Open` table *is* the priority,
    so the table is re-sorted whenever priority changes, and row 1 carries a
    `**(Top priority)**` marker. There is no due-date or deadline field anywhere
    in this system, by design — see `IMPOSTER-MEMORY.md` §"No date-based
    urgency, ever".

## Roles

A processing run uses three distinct, deliberately separated roles — full
definitions in `IMPOSTER-MEMORY.md` §"Three agent roles":

| Role | Sees | Writes |
|---|---|---|
| Router | All of master `incoming/` | Only project `Incoming/` directories |
| Project agent | Only its own project directory | Only inside its own project |
| Collector | Master leftovers, plus every project agent's report | Only master-level files |

The phases are strictly ordered. The router writes nothing at master level.
The collector starts only after every project agent has reported, and re-reads
master leftovers from source rather than inheriting the router's summary.

A project agent's report is **mandatory even when empty** — that is the only
thing that distinguishes a project with nothing to say from an agent that died.

## Procedures

Two runnable procedures, kept as skills in one canonical location:

- **The intake run** — on demand. Router, then every Active project's agent,
  then the collector.
- **The audit pass** — manually triggered only. Never scheduled. It surfaces
  stale and forgotten items for a human decision rather than closing them.

Keep exactly one copy of each procedure and have every consumer read that copy.
Synchronized duplicates drift, always, and the drift is silent.

## Definition of done for an intake run

`incoming/` contains only deliberately-unprocessed material when the run
finishes, and everything left there is named in a task list. An empty
`incoming/` is the glanceable "all caught up" signal.

Full checklist: `IMPOSTER-MEMORY.md` §"Definition of done".
