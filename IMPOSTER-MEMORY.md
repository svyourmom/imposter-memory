# Imposter Memory — Specification

> The overview and quick start are in `README.md`. This is the reference.

A file-based memory system for a single knowledge worker who attends many
meetings, receives a high volume of correspondence, and works across several
concurrent engagements.

Everything is plain Markdown in a directory tree. There is no database, no
index, and no application. An automated agent reads and writes the same files a
human does, and any human can read the whole system with a text editor.

This document is the complete specification. It describes the structures, the
file formats, the rules that must not be broken, and the processing flow.

> All names, projects, and dates in the examples below are fictional. They exist
> to make the worked example readable and refer to nothing real.

---

## Contents

1. [What problem this solves](#what-problem-this-solves)
2. [Core concepts](#core-concepts)
3. [Invariants](#invariants)
4. [Directory layout](#directory-layout)
5. [File formats](#file-formats)
6. [The intake run](#the-intake-run)
7. [Routing: move or excerpt](#routing-move-or-excerpt)
8. [The priority model](#the-priority-model)
9. [Dismissal](#dismissal)
10. [Failure handling](#failure-handling)
11. [The audit pass](#the-audit-pass)
12. [Project lifecycle](#project-lifecycle)
13. [Worked example](#worked-example)
14. [Definition of done](#definition-of-done)
15. [Anti-patterns](#anti-patterns)
16. [Deliberate non-goals](#deliberate-non-goals)

---

## What problem this solves

Source material arrives continuously and in mixed forms: meeting transcripts,
email, chat logs, documents. Each item may be relevant to one engagement, to
several, to none, or to the owner personally. Without a system, three things go
wrong.

**Facts get lost.** A commitment made in the third hour of a week's meetings is
never written down anywhere durable.

**Facts get duplicated and drift.** The same decision is recorded in three notes
with three slightly different wordings, and later nobody knows which is current.

**Work becomes invisible.** Material accumulates in an inbox nobody processes,
and the absence is undetectable because nothing reports on it.

The system addresses these by enforcing a single flow — **intake → living notes
→ immutable evidence** — at every level of the tree, and by making unprocessed
material visibly unprocessed rather than silently absent.

The practical payoff is that the tree can be read cold. Someone covering the
work — a stand-in during leave, or the owner returning to a project after six
weeks away — can reconstruct what is true, what was decided, and what is
outstanding without asking anyone.

---

## Core concepts

### The three states of information

| State | Meaning | Mutability |
|-------|---------|------------|
| **Intake** | Raw source material that has not yet been read and extracted | Transient — should empty out |
| **Living notes** | Extracted, organized, current understanding | Continuously edited |
| **Evidence** | The original sources, after extraction | **Immutable** |

Information moves in one direction only. Intake becomes living notes plus
evidence. Evidence never becomes intake again; if a source changes, the new
version is dropped into intake as a fresh arrival.

### Two levels

**The master level** is the root of the tree. It holds the owner's cross-cutting
view: the consolidated task list, the calendar of dated commitments, the team
roster, and anything that belongs to no single engagement.

**A project** is a bounded, self-contained workspace for one engagement. Projects
live under `Projects/`. A project is never a second master — it is subordinate,
inherits every rule, and can override nothing.

The critical property of a project is that **the entire directory can be moved
or deleted without breaking anything outside it.** Nothing outside a project may
depend on a file inside it.

### Three agent roles

A processing run uses three distinct roles. They are separated because each one
needs a different scope of knowledge, and because mixing them introduces errors
that are impossible to detect afterwards.

| Role | Sees | Writes |
|------|------|--------|
| **Router** | All of master intake | Only into project intake directories |
| **Project agent** | Only its own project directory | Only inside its own project |
| **Collector** | Master leftovers, plus every project agent's report | Only master-level files |

---

## Invariants

These hold everywhere and cannot be overridden by any project.

1. **Never edit evidence.** Files in an evidence directory are read-only forever,
   including their internal citations. A corrected source is re-dropped into
   intake as a new arrival; the old copy stays as it was.

2. **Never invent facts, dates, or owners.** Where information is missing, write
   `TBD`, `???`, or `[NEEDS INPUT]`. A blank is a fact about the source. A guess
   is a corruption that cannot later be distinguished from a real fact.

3. **Cite the source.** Every extracted fact names the evidence file it came
   from. Citations use the bare filename, never a path, so that reorganizing
   evidence storage does not break them.

4. **One canonical home per fact.** Cross-reference rather than restate — with
   one deliberate exception, described under [the priority model](#the-priority-model).

5. **Daily trackers and audit logs are append-only.** Never edit a prior day's
   entry.

6. **Intake is transient.** When a run finishes, an intake directory contains
   only material that was deliberately left unprocessed, and anything left there
   is named in a task list so the omission is visible.

7. **Projects stay self-contained.** No links or dependencies from a project to
   the parent tree or to a sibling project. Master-level records may name a
   project, but must carry the important fact themselves rather than pointing
   into project files.

8. **Trust the registry, don't scan.** Project state comes from the registry
   table in the master README, not from inferring it by looking at folders. A
   directory that exists but is not registered is not a project.

9. **Freshness is recorded.** Every living note carries a `> Last updated:`
   marker directly beneath its title, updated whenever the file changes.
   Evidence files never carry one, because they never change.

---

## Directory layout

### Master

```text
/
├── README.md                  # Master index; holds the project registry
│                              # and the pinned core-memory block
├── AGENTS.md                  # The operating contract for automated agents
├── IMPOSTER-MEMORY.md         # This document
│
├── incoming/                  # Master intake
│
├── status/                    # Living notes: current state
│   ├── owner-action-items.md  # The consolidated task list
│   ├── team-status.md         # Cross-engagement situational summary
│   └── audit-log.md           # Append-only record of audit passes
│
├── memory/                    # Living notes: durable reference
│   ├── events.md              # Dated commitments and deadlines
│   ├── people.md              # Roster, roles, reporting lines
│   ├── who-is-doing-what.md   # Current work assignments
│   └── conventions.md         # Ticketing and process conventions
│
├── artifacts/                 # Evidence, immutable
│   └── YYYY/
│       └── MM/
│
├── Projects/
│   ├── README.md              # Portfolio-level index
│   ├── .cursor/templates/     # Canonical project templates
│   ├── Cheerios/
│   ├── Trix/
│   └── FruitLoops/
│
└── .cursor/skills/            # The only copies of the processing skills
    ├── process-incoming/
    └── audit-pass/
```

The `.cursor/` paths are one tool's convention. Any location works, as long as
there is exactly one copy of each skill and every consumer reads that copy.

### A project

Every project has the identical shape. Consistency is deliberate: an agent or a
human opening any project finds the same files in the same places.

```text
Projects/Cheerios/
├── README.md              # Declares this project's structure in front matter
├── Incoming/              # Project intake
│
├── 01-program-overview.md
├── 02-stakeholders.md
├── 03-workstreams-and-status.md
├── 04-technical-context.md
├── 05-timeline.md
├── 06-risks-and-open-questions.md
├── action-items.md        # This project's task list
│
└── artifacts/             # Project evidence, immutable
    └── YYYY/
        └── MM/
```

A newly created project receives this full skeleton immediately, including files
that are empty. An empty `05-timeline.md` is a visible statement that no timeline
is known. A missing one is ambiguous.

**Intake directory casing differs by level** — `incoming/` at master and
`Incoming/` in projects. Resolve case-insensitively and never create a second
one.

---

## File formats

### README front matter

Every README declares its own structure in YAML. Agents read these values rather
than assuming directory names, which is what allows one skill to operate at any
level of the tree.

Master:

```yaml
---
type: master
intake_dir: incoming/
living_dirs: [status/, memory/]
evidence_dir: artifacts/
---
```

Project:

```yaml
---
type: project
name: Cheerios
status: active
owner: TBD
intake_dir: Incoming/
living_dirs: [.]
evidence_dir: artifacts/
todo_file: action-items.md
last_updated: YYYY-MM-DD
---
```

### The project registry

The master README carries the authoritative list of projects.

```markdown
| Project | Path | Status | Last processed |
|---------|------|--------|----------------|
| Cheerios | `Projects/Cheerios/` | Active | YYYY-MM-DD |
| Trix | `Projects/Trix/` | Active | YYYY-MM-DD |
| Fruit Loops | `Projects/FruitLoops/` | Active | YYYY-MM-DD |
| Honeycomb | `Projects/Honeycomb/` | Archived | YYYY-MM-DD |
```

### The core-memory block

The master README carries a pinned block immediately after the title, between
explicit markers. It exists so that an agent starting with no history reads the
current state before anything else. It is also the first thing a stand-in
should read.

```markdown
<!-- CORE-MEMORY:START (generated — do not hand-edit) -->
## Right now

> Last synced: YYYY-MM-DD · Last audit: YYYY-MM-DD

- **Top priority:** Deliver the Cheerios integration walkthrough Thursday
- **Urgent:** Trix access credentials expire in four days with no renewal recorded
- **Active projects:** Cheerios, Trix, Fruit Loops
- **Next audit due:** YYYY-MM-DD
<!-- CORE-MEMORY:END -->
```

It holds exactly four fields and is written only by the collector at the end of a
run, or by the audit pass. It is never hand-edited; if it looks wrong, the source
note is wrong and should be fixed instead.

### Living notes

Plain Markdown. The only mandatory element is the freshness marker:

```markdown
# Team Status

> Last updated: YYYY-MM-DD

## Summary
...
```

### Task lists

Both the master list and each project list use the same shape: open items, then a
dismissed section beneath them.

```markdown
# Cheerios — Action Items

> Last updated: YYYY-MM-DD

## Open

| # | Item | Notes |
|---|------|-------|
| 1 | Confirm the delivery format with the client | Raised twice, no answer recorded (`2026-03-09-cheerios-sync.md`) |
| 2 | Rebuild the packaging step | Blocked until item 1 resolves |

## Dismissed

| Item | Dismissed | Evidence |
|------|-----------|----------|
| Chase the missing access request | 2026-03-11 | Access was granted 2026-03-10; the request is moot (`2026-03-10-access-granted.md`) |
```

Dismissal is never silent. Every removed item leaves a row naming what closed it.

### Evidence naming and storage

Evidence is stored in year/month folders **by the date of the source document** —
the date the meeting happened or the email was sent — not the date it was
processed. This keeps the folder consistent with the filename, and it is the only
scheme that can be applied retroactively, since processing dates are typically
not recorded anywhere.

```text
artifacts/
├── 2026/
│   ├── 02/
│   │   ├── 2026-02-14-quarterly-planning-summary.md
│   │   └── Vendor renewal notice.eml
│   └── 03/
│       ├── 2026-03-09-cheerios-sync.md
│       └── Fwd- packaging question.eml
```

Two kinds of file live here, and they follow different naming rules.

**Original sources keep their original filenames, unchanged.** This is part of
immutability — the name is evidence too. Most originals carry no date in the
name, which is precisely why the folder structure matters.

**Derived summaries written during processing are named `YYYY-MM-DD-topic.md`**,
using the source document's date.

### Source reliability marking

Some sources are unreliable in ways that change meaning — automated meeting
transcription is the common case, where a single dropped negation can invert a
decision.

When a source is known to be unreliable, the derived summary carries a warning
**above any content**, so a reader cannot reach a fact without passing it:

```markdown
# Team Sync — 2026-03-09

Source: automated transcription, 48 minutes.

> ## ⚠ RELIABILITY WARNING — known errors
>
> Do not quote this source verbatim and do not extract tasks from it
> mechanically.
>
> **Corrected by the owner:** at 21:44 the transcript reads
> "this is a priority." The accurate line is "this is **not** a priority."
>
> Full error catalogue at the end of this file.
```

Corrections are recorded **inside the derived summary**, never by editing the
original. An error catalogue at the foot of the file lists mangled proper nouns,
audio gaps, and misattributed speakers, so a later reader knows how much weight
the source can carry.

---

## The intake run

One command triggers the whole sequence. The three phases are strictly ordered;
phase three cannot begin until phase two has fully reported.

### Phase 1 — Router

The router reads **every** file in master intake and decides what each one is. It
writes nothing at master level. Not notes, not the calendar, not the task list.

This restriction is the reason the phases exist. A project agent can surface
something the router never saw — a meeting cancellation dropped straight into a
project's intake, for instance — which invalidates a conclusion the router was
about to record. Anything written before the project agents report may have to be
rewritten, so nothing is written.

For each source the router either:

- **Moves** it into a single project's intake, if it belongs to exactly one
  project and contains nothing of master-level relevance; or
- **Writes a focused excerpt** into each relevant project's intake and leaves the
  original where it is.

The router then starts one agent for every project registered as Active.

### Phase 2 — Project agents

**Every Active project gets an agent on every run**, whether or not the router
sent it anything. This is the only way material placed directly into a project's
intake — bypassing master entirely — is ever discovered. An agent that opens an
empty intake directory reports that it ran and found nothing, and stops.

**Each agent is fully blind to everything outside its own directory.** It does not
know the cross-project ranking, does not know the other projects exist, and does
not see the owner's personal commitments.

This is deliberate. An agent that knows its project has been deprioritized will
tend to phrase its findings to match — softening a real problem into something
that reads as tolerable. Blind agents produce unhedged input, and all weighing
happens later, in one place, where it is visible and correctable.

Within its own directory an agent will:

1. Read everything in its intake.
2. Extract facts into its living notes, updating freshness markers.
3. Maintain `action-items.md`, ordered by **priority within this project only**.
4. Dismiss items that its new material clearly closes, recording each with the
   evidence.
5. Move processed sources into its evidence directory, in the right year/month
   folder.
6. **Leave anything it cannot confidently place in intake**, and add a task
   saying so — for example, *"Unresolved: `packaging-notes.md` may belong to a
   different engagement; left in intake."* This is a safe resting state. The file
   stays visibly unprocessed and the next run retries it.
7. Return a report.

The report is mandatory even when empty, and contains:

- Confirmation that the agent ran
- What it processed and what it changed
- Its current task list
- **Anything it saw that may not belong to it** — a dated commitment for the
  owner personally, or a policy affecting the whole team. The agent does not act
  on these; it hands them up.
- Anything it left in intake, and why

Because a report is always produced, **silence means failure**. This is the only
way an empty contribution can be distinguished from a dead agent.

### Phase 3 — Collector

The collector is a **fresh** role that begins only after every project agent has
reported. It does not inherit the router's reading.

It re-reads the master leftovers **from source**. This is a genuine second read of
material the router already read, and it is intentional: reading the original
means the collector can catch something the router got wrong, whereas inheriting
a summary would make the router's errors permanent and invisible.

**Leftovers** are everything still in master intake after phase 1. This includes
both material that belonged to no project, and **the originals of every mixed
source that was excerpted** — because excerpting copies information outward
without removing the original.

The collector then:

1. Processes the leftovers into master living notes.
2. Reads every project report.
3. Places escalated items — the "may not belong to me" findings — where they
   actually belong.
4. Determines the **cross-project ranking**, and **shows its reasoning**. This
   judgment cannot be derived from the project lists, since each was written
   without knowledge of the others, and it changes for reasons entirely outside
   any project.
5. Reconciles the master task list **in place**.
6. Marks the slot of any project that failed to report.
7. Refreshes the core-memory block.
8. Reports to the owner.

The master task list is a **persistent file**, not a generated one. The collector
merges into it and never rewrites it wholesale, so anything the owner types in by
hand survives. Preventing that file from growing without limit is the job of the
[audit pass](#the-audit-pass).

---

## Routing: move or excerpt

The router's decision is a judgment call on each source, with a **standing bias
toward excerpting** whenever there is doubt.

The bias exists because the two errors are not equally bad.

| Wrong choice | Consequence |
|--------------|-------------|
| Excerpted something that was purely single-project | Mild redundancy. Nothing is lost. |
| Moved something that was actually mixed | Master permanently loses whatever it needed — **and nobody notices**, because the fact simply never appears anywhere |

**Move** when a source is addressed to or about exactly one project and contains
nothing of team-level or personal relevance. Recurring project status reports are
the clearest case: a daily status email for Cheerios is Cheerios material and
nothing else.

**Excerpt** in every other case, including these traps:

- A source titled for one project that also carries a deadline for the owner
  personally.
- A project meeting that spends five minutes on a policy affecting everyone.
- Any meeting covering more than one engagement.

An excerpt must **stand alone**. Because a project may be archived or deleted, an
excerpt cannot rely on the reader having access to the original. It carries the
source filename and date, the relevant content, decisions, actions with owners
and dates, and the open questions — enough to be processed independently by
someone who has never seen the source.

**Never route into a project that is archived, removed, or absent from the
registry.** Never recreate a project because an old note mentions it.

---

## The priority model

There are two independent orderings, and they are allowed to disagree.

**Project priority is internal.** A project's list is ordered by what matters
most *within that engagement*, judged with no knowledge of anything outside it.

**Master priority is cross-cutting.** It weighs projects against each other and
against work belonging to no project at all, and it responds to forces no project
can see.

The same item therefore legitimately appears in both lists with different ranks.
An item that is first in the Cheerios list may sit third on the master list,
behind an unrelated Trix deadline and a personal administrative task. This is not
duplication of a fact; it is two different judgments, both correct.

### Master entries are self-contained

Because a project may be archived or deleted at any time, a master list entry
**never points into a project file** for its content. It carries enough
description to stand on its own. The text is duplicated between the two lists,
and that redundancy is accepted so that projects remain removable.

### The ranking is inferred and shown

The collector works out the cross-project ranking each run and **presents its
reasoning** rather than applying a hidden rule. The owner corrects it where it is
wrong. Over time, patterns that prove reliable can be written down; until then,
the reasoning stays visible on every run.

---

## Dismissal

**There is no automatic aging.** Nothing is retired because it has gone quiet. An
item leaves a list only when something actively closes it.

Automatic aging was removed because "nobody has mentioned this lately" means
completely different things in different contexts. A slow-moving pre-sales
engagement may go a fortnight without discussion while every item on it remains
valid. Under an aging rule, those items disappear and nobody ever learns they
existed.

An agent **may dismiss an item on its own** when the evidence clearly closes it —
the deadline passed, the decision was made, the blocker was removed. Every
dismissal is recorded in the dismissed section beneath the open items, naming the
evidence that closed it. Nothing is deleted.

Items that have gone quiet but are not closed are the concern of the
[audit pass](#the-audit-pass), which surfaces them for a human decision rather
than acting on them automatically.

---

## Failure handling

Two distinct situations, handled differently.

**An agent that cannot place a file** is not a failure. It leaves the file in its
intake and records a task explaining why. The material stays visibly unprocessed
and the next run retries it, since every project is processed every time.

**An agent that fails to report** is a failure, and the run continues anyway. The
collector writes the master list with that project's contribution **explicitly
marked as missing**, so the gap is visible in the output rather than being
mistaken for that project having nothing to contribute. The alternative — halting
the whole run — would discard the work every other agent completed successfully.

Because reports are mandatory even when empty, the collector can always tell the
difference between a project with nothing to say and a project that did not
answer.

---

## The audit pass

A separate, **manually triggered** operation. It never runs automatically or on a
schedule.

The intake run is concerned with new material. The audit pass is concerned with
what nothing has touched. Its focus is:

- **Closing out older issues** — items that have been open a long time and are
  probably resolved, superseded, or no longer relevant.
- **Finding forgotten items** — commitments that were recorded once and never
  mentioned again.
- **Trimming the master task list** — the counterweight to that file being
  persistent. Without this, it grows indefinitely.
- **Consistency checking** — cited evidence files that no longer resolve,
  dismissed items that reappeared, project notes that contradict master notes.

The audit pass **surfaces items for decision rather than deciding**. Where the
intake run may dismiss something because new evidence closed it, the audit pass
raises "this has not moved in five weeks" and asks.

It appends an entry to the audit log — never editing prior entries — and
refreshes the core-memory block.

---

## Project lifecycle

### Creating a project

A project is created when a new engagement first appears in source material,
even if almost nothing is known about it. Recording a name with three known facts
is better than losing it in meeting noise.

1. Create the directory with the **full standard skeleton**, including empty
   files.
2. Write the README with complete front matter. Unknown values are `TBD`, never
   guessed — a new project routinely has `owner: TBD`.
3. Write the first intake note capturing what is known and, explicitly, what is
   not.
4. **Add the project to the registry** in the master README. It is not a project
   until this is done.

Where a name is uncertain — heard in a meeting and never seen written — the
README states so plainly and instructs a future reader to correct it.

### Archiving and removal

Change `status` in the README front matter and in the registry. Once archived, no
new material is routed there and no agent runs for it. Archived projects are
reference-only until explicitly reactivated.

Removal is deletion of the directory. This must be safe at any time, which is why
master records carry their own copies of important facts and why excerpts must
stand alone.

---

## Worked example

A run on a Wednesday morning. Three Active projects: **Cheerios**, **Trix**, and
**Fruit Loops**. Master intake contains four items. All names and dates here are
fictional.

### Before

```text
incoming/
├── Weekly leadership sync-transcript.txt   # covers all three projects + policy
├── Cheerios daily status - Mar 10.eml      # routine, single project
├── Benefits enrollment deadline.eml        # personal, no project
└── Industry newsletter March.eml           # no relevance to anything
```

### Phase 1 — Router

The router reads all four.

**`Cheerios daily status - Mar 10.eml`** is a recurring single-project report
containing nothing of wider relevance. **Moved** to
`Projects/Cheerios/Incoming/`.

**`Weekly leadership sync-transcript.txt`** covers all three engagements *and*
contains a team-wide policy change and a commitment by the owner. **Excerpted**
three times; the original stays put.

**`Benefits enrollment deadline.eml`** belongs to no project. **Left** for the
collector.

**`Industry newsletter March.eml`** has no bearing on anything. **Left** for the
collector, which will file it as evidence without extracting anything.

```text
incoming/
├── Weekly leadership sync-transcript.txt   # left — original of a mixed source
├── Benefits enrollment deadline.eml        # left — master-level
└── Industry newsletter March.eml           # left — no relevance

Projects/Cheerios/Incoming/
├── Cheerios daily status - Mar 10.eml              # moved
└── 2026-03-10-integration-format-decision.md       # excerpt

Projects/Trix/Incoming/
└── 2026-03-10-access-renewal-raised.md             # excerpt

Projects/FruitLoops/Incoming/
└── 2026-03-10-scope-question-unresolved.md         # excerpt
```

Note the router wrote nothing at master level, despite having read a transcript
containing a policy change and a personal commitment. Both wait for the
collector.

### Phase 2 — Project agents

Three agents start, one per project, each blind to the others.

**Cheerios** processes both files, updates `03-workstreams-and-status.md` and
`05-timeline.md`, adds a task, and files both sources into
`artifacts/2026/03/`. Its report notes that the transcript excerpt mentioned a
deadline for the owner personally — **flagged as possibly not belonging here**.

**Trix** processes its excerpt, updates `06-risks-and-open-questions.md`, and
**dismisses** an existing task because the new material closes it:

```markdown
## Dismissed

| Item | Dismissed | Evidence |
|------|-----------|----------|
| Chase the renewal owner | 2026-03-11 | Owner named and renewal scheduled (`2026-03-10-access-renewal-raised.md`) |
```

**Fruit Loops** finds a second file in its intake that the router never placed
there — someone dropped it in directly. It processes the excerpt, but cannot
determine whether the stray file belongs to this engagement. It **leaves the file
in intake** and records:

```markdown
| 3 | Unresolved: `vendor-comparison-draft.md` left in intake | Appeared without routing. Content does not clearly relate to this engagement. Needs a human decision. |
```

Had no agent run for Fruit Loops, that stray file would have remained invisible
indefinitely. This is why every project is processed on every run.

### Phase 3 — Collector

A fresh collector starts. It re-reads the three remaining files from source, not
from the router's notes.

From the transcript it extracts the **team-wide policy change** into
`status/team-status.md` and the **owner's personal commitment** into
`memory/events.md` and the master task list. Neither belonged to any project, and
neither would exist anywhere had the router been permitted to route the
transcript wholesale into one project.

From the benefits email it adds a dated item. The newsletter is filed to
`artifacts/2026/03/` with no extraction.

It reads all three project reports, notices Cheerios flagged a personal deadline,
and places it at master level. It then produces the cross-project ranking with
reasoning shown:

```markdown
## Cross-project ranking — reasoning

1. **Trix credential renewal** — hard external deadline in four days; blocks
   delivery if missed. Trix ranks this second internally, but nothing else on
   any list has a fixed date this close.
2. **Cheerios walkthrough** — Thursday commitment made by the owner in the
   leadership sync. Cheerios ranks this first internally; agreed.
3. **Benefits enrollment** — belongs to no project, but the window closes and
   cannot be reopened.
4. **Fruit Loops scope question** — Fruit Loops ranks this first internally, but
   it is blocked on someone else and has no date.
```

Finally it reconciles the master list in place, leaving hand-written entries
untouched, and refreshes the core-memory block.

### After

```text
incoming/                       # empty — everything processed

artifacts/2026/03/
├── Weekly leadership sync-transcript.txt
├── 2026-03-10-weekly-leadership-sync.md    # derived summary
├── Benefits enrollment deadline.eml
└── Industry newsletter March.eml

Projects/FruitLoops/Incoming/
└── vendor-comparison-draft.md   # deliberately left; named in the task list
```

Master intake is empty. One file remains in a project intake, and it is not
forgotten — it is recorded as an open question, and the next run will look at it
again.

---

## Definition of done

### An intake run

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

### An audit pass

- [ ] Master and every Active project's notes re-examined
- [ ] Long-open items surfaced for decision, not silently closed
- [ ] Forgotten commitments raised
- [ ] Master task list trimmed
- [ ] Citations verified to resolve
- [ ] Entry appended to the audit log
- [ ] Core-memory block refreshed

---

## Anti-patterns

**Leaving processed material in intake without recording it.** Intake is a
worklist. An unrecorded file there is indistinguishable from one that arrived
seconds ago.

**Editing evidence.** Including fixing a typo, including correcting a citation.
Corrections go in the derived summary.

**Writing at master level during the router phase.** The reason is ordering, not
tidiness: a project agent may invalidate the conclusion.

**Giving project agents outside context.** It seems helpful and it biases their
output.

**Pointing a master entry into a project file.** It breaks the moment the project
is archived.

**Guessing at a value to avoid writing `TBD`.** A guess is indistinguishable from
a fact one week later.

**Inferring project state from directory listings.** The registry is
authoritative. A directory may exist for reasons that do not make it a live
project.

**Maintaining duplicate copies of the processing logic.** One definition, read by
every consumer. Synchronized copies drift, always, and the drift is silent.

**Restating the same fact in several notes** rather than cross-referencing —
except for master task entries, where self-containment is required and the
duplication is deliberate.

---

## Deliberate non-goals

**This is not a ticketing system.** It does not replace whatever formal tracker
the organization uses. It is the owner's own working memory, and items in it
frequently have no counterpart anywhere else.

**It does not attempt to be complete.** Sources are read as they arrive. Nothing
reconstructs history that was never captured.

**It does not resolve contradictions automatically.** Where two sources disagree,
both are recorded along with the disagreement.

**It does not act on the owner's behalf.** It reads, extracts, organizes, and
reports. It does not send, reply, commit to version control, or close anything
in an external system.
