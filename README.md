# Imposter Memory

**A memory system that teaches someone how to do your job — including you.**

Plain Markdown in a directory tree. No database, no index, no application, no
lock-in. An automated agent reads and writes the same files a human does, and
any human can read the whole thing with a text editor.

---

## Why this exists

Two situations look completely different and turn out to be the same problem.

**The first is imposter syndrome.** You sit down to work on something you have
owned for months and cannot reconstruct why it is the way it is. The decision
was made in the third hour of a meeting six weeks ago. The constraint that
rules out the obvious approach was mentioned once, in an email, by someone who
has since left. You know you knew this. You cannot find where you knew it. The
feeling that you are not qualified to do your own job is, more often than not,
a retrieval failure wearing a costume.

**The second is covering for somebody.** A colleague goes on leave and hands
you three engagements. You get a folder of documents and a fifteen-minute
handover call. Everything that matters — who actually decides things, which
deadline is real, what was already tried and abandoned — lives in their head,
and their head is on a beach.

The fix for both is the same: a written record that stays current on purpose,
separates what is true now from what was said when, and can be read cold by
someone with no context.



---

## The core idea

Information moves in one direction only, at every level of the tree:

```
intake  →  living notes  →  immutable evidence
```

| State | What it is | Mutability |
|---|---|---|
| **Intake** | Raw material that has not been read and extracted yet | Transient — should empty out |
| **Living notes** | The current understanding, extracted and organized | Continuously rewritten |
| **Evidence** | The original sources, after extraction | **Immutable forever** |

Living notes answer *what is true now*. Evidence answers *what was said, when,
by whom*. Keeping those apart is what stops the record from decaying into a
pile of stale notes nobody rereads.

Two more structural choices carry most of the weight:

- **Two levels.** A master level holds the cross-cutting view. Projects are
  bounded, self-contained workspaces underneath it. Any project directory can
  be moved or deleted without breaking anything outside it.
- **Three separated agent roles.** A router decides where incoming material
  goes. A project agent processes one project, blind to every other. A
  collector runs last, re-reads the leftovers from source, and produces the
  cross-project ranking. They are separated because mixing them introduces
  errors that are impossible to detect afterwards — the spec explains why each
  boundary sits where it does.

And one rule that does more work than it looks like it should:

> **Unprocessed material must be visibly unprocessed.**

Nothing is ever silently dropped. An agent that cannot confidently place a
file leaves it in intake and writes a task saying so. A project that fails to
report gets its slot marked as missing rather than mistaken for having nothing
to say. The failure mode is a visible gap, not an invisible one.

---

## Getting started

### The easy way: hand it to your assistant

The system is a written specification, so the simplest way to stand one up is
to point an AI assistant at this repository and let it build the tree for you:

> Read the `IMPOSTER-MEMORY.md` and `AGENTS.md` files in this repo, then build
> me an Imposter Memory tree in this directory. Create the master skeleton and
> one project called `<name>`, using the full standard project layout.

Then drop material into `incoming/` and ask it to do an intake run.

Three things worth knowing before you do that:

- **Check what it builds.** Compare against `example/`. Getting the skeleton
  right matters more than it looks — an empty `05-timeline.md` says no
  timeline is known, and a missing one says nothing at all.
- **Hold it to invariant 2.** Never invent facts, dates, or owners. Assistants
  are cheerfully willing to fill a gap with something plausible, and a guess is
  indistinguishable from a fact one week later. If it writes a date you did not
  give it, that is a bug, not a nicety.
- **You own the ranking.** The collector shows its reasoning every run
  precisely so you can overrule it. Read it; do not rubber-stamp it.

### The manual way

1. Copy `IMPOSTER-MEMORY.md` and `AGENTS.md` into the root of a new directory.
2. Build the master skeleton from `example/`: `incoming/`, `status/`,
   `memory/`, `artifacts/`, `Projects/`, and a root `README.md` carrying the
   front matter and the registry table.
3. Create your first project by copying `templates/project-template/` to
   `Projects/<Name>/` — the whole directory, including the files that are empty.
   Then fill in its README front matter.
4. Add the project to the registry in the master `README.md`. It is not a
   project until that row exists.
5. Copy `templates/procedures/` into your tree, keeping one canonical copy of
   each.
6. Drop material into `incoming/` and process it, following
   `process-incoming.md`.

You do not need an agent to use this, and nothing here assumes one exists. The
layout and the rules work by hand, the two procedures are written to be followed
by a person, and no packaging format — skills, prompt files, commands — is
required or assumed. The agent roles just make the processing repeatable.

---

## What's in this repo

| File | What it is |
|---|---|
| `README.md` | This file — the overview |
| `IMPOSTER-MEMORY.md` | The complete specification. Structures, formats, rules, processing flow, worked example |
| `AGENTS.md` | The operating contract. The short version an agent reads first; invariants that nothing overrides |
| `example/` | A worked example tree — master index, project README, task list, living note, and the full directory layout |
| `templates/` | What you copy into your own tree — the project skeleton, and the two procedures as tool-neutral Markdown |

`IMPOSTER-MEMORY.md` is the authority. `AGENTS.md` is the contract that
implements it. If they ever disagree, the spec wins and the contract is a bug.

---

## Design principles

The full list of invariants is in `AGENTS.md`. The ones worth knowing before
you decide whether this suits you:

- **Never edit evidence.** Not to fix a typo, not to correct a citation. A
  corrected source is re-dropped into intake as a new arrival. Corrections
  live in the derived summary, never in the original.
- **Never invent facts, dates, or owners.** Write `TBD` or `[NEEDS INPUT]`. A
  blank is a fact about the source. A guess is indistinguishable from a real
  fact one week later.
- **No automatic aging.** Nothing is retired because it went quiet. "Nobody
  has mentioned this lately" means completely different things in different
  contexts, and an aging rule deletes exactly the items nobody will ever learn
  existed. Items leave a list when something actively closes them, and the
  dismissal is recorded with the evidence that closed it.
- **Project agents are kept blind on purpose.** An agent that knows its
  project has been deprioritized will phrase its findings to match, softening
  a real problem into something that reads as tolerable. All weighing happens
  later, in one place, where it is visible and correctable.
- **Position is the priority.** Task lists are sorted, re-sorted whenever
  priority changes, and row 1 is marked `(Top priority)` so a cold reader never
  has to infer rank from row order.
- **The cross-project ranking is shown, not hidden.** The collector presents
  its reasoning every run so the owner can correct it, rather than applying a
  rule nobody can see.

---

## Deliberate non-goals

- **Not a ticketing system.** It does not replace a formal tracker. It is
  working memory, and items in it frequently have no counterpart anywhere.
- **Not a reminder system.** There is no due-date field, no deadline column, and
  nothing that surfaces an item because a date arrived. A dated commitment is
  recorded as a fact and may well be *why* something ranks first, but priority is
  expressed as position in a sorted list — row 1, marked. Interrupting you on the
  day is a calendar's job.
- **Not complete.** Sources are read as they arrive. Nothing reconstructs
  history that was never captured.
- **Does not resolve contradictions.** Where two sources disagree, both are
  recorded along with the disagreement.
- **Does not act on your behalf.** It reads, extracts, organizes, and reports.
  It does not send, reply, commit, or close anything in an external system.

---

## A note on the examples

Every name, project, and date in this repository is fictional and exists only
to make the worked example readable. The projects throughout — Cheerios, Trix,
Fruit Loops, Honeycomb — are placeholders, not real engagements.

---

## Known gaps

Found by deploying this system by hand and following the instructions above
literally. Recorded here because a gap you can see is worth more than one you
discover the hard way.

- **No init sequence.** Standing up a tree is entirely manual — five prose steps,
  no script. An init step should also greet the reader and, importantly, prompt
  any assistant involved to re-sync whatever memory *it* keeps about the tree,
  which can drift from the tree's own contents without either side noticing.
- **The core-memory block can go stale between runs.** The spec says the collector
  and the audit pass refresh it. It says nothing about the far more common case —
  a human adding one row to a task list outside any formal run — after which the
  block quietly no longer matches. Refresh it when you edit a list by hand.
- **Don't clone this repository as your working tree.** Copy the two documents
  out of it instead, as the manual way says. Cloning gives the root `README.md`
  two incompatible jobs — public documentation and your private master index and
  registry — and puts your working notes in a public repo's git history.
- **"Project" here always means `Projects/<Name>/`** — a directory inside an
  Imposter Memory tree. Never a git repository, a GitHub project, or any other
  sense of the word. The spec never says this outright, and conflating the two is
  how the cloning mistake above happens in the first place.
- **Importing existing content is not specified, and it loses evidence.** The
  manual way covers building an empty tree, not bringing an existing body of work
  into one. Structure carries across easily; the cited *sources* do not, and you
  end up with living notes citing evidence files that were never brought over.
  Gather and file the sources as a deliberate, separate step.

## Status

Early public release. The specification is stable and in daily use; but don't trust it yet. 
