# Procedure — Process incoming

Run this on demand, whenever material has accumulated in intake. Three phases,
**strictly ordered**: phase 3 cannot begin until phase 2 has fully reported.

The authority for everything below is `IMPOSTER-MEMORY.md` §"The intake run".
Where this file and the spec disagree, the spec wins and this file is a bug.

Read `AGENTS.md` first. The invariants there are not optional and nothing in
this procedure overrides them.

---

## Phase 1 — Router

Read **every** file in master intake and decide what each one is.

**Write nothing at master level.** Not notes, not the calendar, not the task
list. A project agent can surface something you never saw — a cancellation
dropped straight into a project's intake — that invalidates a conclusion you
were about to record. Anything written now may have to be rewritten, so nothing
is written now.

For each source, either:

- **Move** it into a single project's intake — only when it is addressed to or
  about exactly one project and carries nothing of team-level or personal
  relevance. A recurring project status email is the clearest case.
- **Excerpt** it into each relevant project's intake, leaving the original in
  master intake.

**When in doubt, excerpt.** The two mistakes are not equally bad: excerpting
something single-project costs mild redundancy, while moving something mixed
means master permanently loses what it needed *and nobody notices*, because the
fact simply never appears anywhere.

Excerpt — do not move — when a source titled for one project also carries a
personal deadline, when a project meeting spends five minutes on team-wide
policy, or when a meeting covers more than one engagement.

**Every excerpt must stand alone.** A project may be archived or deleted, so an
excerpt cannot assume the reader can reach the original. Carry the source
filename and date, the relevant content, the decisions, the actions with owners
and dates, and the open questions.

**Never route into a project that is archived, removed, or absent from the
registry**, and never recreate a project because an old note mentions it. Read
the registry in the master `README.md` to determine status; do not infer it by
looking at folders.

Then start one agent for every project registered **Active** — including the
ones you sent nothing.

---

## Phase 2 — Project agents

**Every Active project is processed on every run**, whether or not phase 1 sent
it anything. This is the only way material dropped straight into a project's
intake is ever found.

**Work blind.** An agent sees only its own project directory. It does not know
the cross-project ranking, that other projects exist, or the owner's personal
commitments. This is deliberate: an agent that knows its project has been
deprioritized softens a real problem into something that reads as tolerable.
All weighing happens later, in one place, where it is visible and correctable.

Within the project directory:

1. Read everything in its intake.
2. Extract facts into the living notes, citing each by bare filename, and
   re-date the `> Last updated:` marker on every file changed.
3. Maintain `action-items.md`, ordered by priority **within this project only**.
   Re-sort the `## Open` table if priority has changed, and move the
   `**(Top priority)**` marker with row 1. A new item is not appended to the
   bottom unless the bottom is where it belongs.
4. Dismiss items the new material clearly closes, each with the evidence that
   closed it. Never delete a row silently.
5. File processed sources into `artifacts/YYYY/MM/`, by the date of the source
   document.
6. **Leave anything you cannot confidently place** in intake, and write a task
   saying so — *"Unresolved: `packaging-notes.md` may belong to a different
   engagement; left in intake."* This is a safe resting state, not a failure:
   the file stays visibly unprocessed and the next run retries it.
7. Return a report.

### The report is mandatory, even when empty

Silence means failure. A report that says "ran, intake was empty, nothing
changed" is the only thing that distinguishes a quiet project from a dead agent.

Every report contains:

- Confirmation that the agent ran
- What it processed and what it changed
- Its current task list
- **Anything it saw that may not belong to it** — a commitment for the owner
  personally, a policy affecting the whole team. Hand these up; do not act on
  them.
- Anything left in intake, and why

---

## Phase 3 — Collector

Start **fresh**, only after every project agent has reported. Do not inherit the
router's reading.

**Re-read the master leftovers from source.** This is a deliberate second read
of material phase 1 already read: going back to the original means you can catch
something the router got wrong, where inheriting its summary would make its
errors permanent and invisible.

Leftovers are everything still in master intake after phase 1 — both material
belonging to no project, and **the original of every mixed source that was
excerpted**, since excerpting copies information outward without removing the
original.

Then:

1. Process the leftovers into the master living notes.
2. Read every project report.
3. Place escalated items — the "may not belong to me" findings — where they
   actually belong.
4. Determine the **cross-project ranking** and **show your reasoning**. This
   cannot be derived from the project lists, each of which was written with no
   knowledge of the others. Present the reasoning so the owner can correct it;
   never apply a hidden rule.
5. Reconcile the master task list **in place**, re-sorted so position reflects
   the ranking you just determined, row 1 marked `**(Top priority)**`. Rank
   standing personal and administrative work in this same list, at its real
   position — nothing routes that work to you, so it is the most reliably
   neglected.
6. Mark the slot of any project that failed to report as **missing**. Do not
   halt the run; that would discard every other agent's completed work.
7. Refresh the core-memory block.
8. Report to the owner.

The master task list is a **persistent file, not a generated one.** Merge into
it; never rewrite it wholesale. Anything the owner typed by hand must survive.
Keeping it from growing without limit is the audit pass's job, not yours.

---

## Definition of done

- [ ] Every intake file read, except operating-system junk
- [ ] Single-project sources moved; mixed sources excerpted with originals kept
- [ ] An agent ran for every Active project, including those with empty intake
- [ ] Every project agent returned a report
- [ ] Living notes updated, cited, and freshness markers re-dated
- [ ] Every processed source filed to evidence in the correct year/month folder
- [ ] Anything left in intake is named in a task list
- [ ] Escalated items placed at the level they belong to
- [ ] Master task list reconciled in place, hand edits preserved
- [ ] Cross-project ranking produced with visible reasoning
- [ ] Any non-reporting project marked as a gap
- [ ] Core-memory block refreshed
- [ ] Structured summary reported

Master `incoming/` now holds only material deliberately left unprocessed, and
everything left there is named in a task list. An empty `incoming/` is the
glanceable "all caught up" signal.
