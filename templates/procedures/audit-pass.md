# Procedure — Audit pass

**Manually triggered only. Never scheduled, never automatic.**

The authority for everything below is `IMPOSTER-MEMORY.md` §"The audit pass".
Where this file and the spec disagree, the spec wins and this file is a bug.

Read `AGENTS.md` first. The invariants there are not optional and nothing in
this procedure overrides them.

---

## What this is for

The intake run is concerned with new material. **The audit pass is concerned
with what nothing has touched.**

Its focus:

- **Closing out older issues** — items open a long time that are probably
  resolved, superseded, or no longer relevant.
- **Finding forgotten items** — commitments recorded once and never mentioned
  again.
- **Trimming the master task list** — the counterweight to that file being
  persistent. Without this, it grows indefinitely.
- **Consistency checking** — cited evidence files that no longer resolve,
  dismissed items that have reappeared, project notes contradicting master
  notes.

## The rule that defines this procedure

**Surface items for decision; do not decide.**

Where an intake run may dismiss something *because new evidence closed it*, the
audit pass raises "this has not moved in five weeks" **and asks**. It closes
nothing on its own authority.

This is why the pass is never scheduled. There is **no automatic aging** in this
system: nothing is retired because it went quiet. "Nobody has mentioned this
lately" means completely different things in different contexts, and an aging
rule deletes exactly the items nobody will ever learn existed. A pass that ran
on a timer would drift into being that rule.

---

## Steps

1. Re-examine master and every **Active** project's notes. Take project status
   from the registry in the master `README.md`, not from scanning folders.
2. **Surface long-open items** for a decision. Present them with what is known:
   how long open, what it is blocked on, what last touched it. Do not close
   them.
3. **Raise forgotten commitments** — things recorded once and never followed up.
4. **Trim the master task list**, with the owner's decisions from steps 2 and 3.
   Anything removed leaves a dismissal row naming what closed it; nothing
   disappears silently.
5. **Verify citations resolve.** Every bare filename cited in a living note
   should correspond to a file that exists in an evidence directory. Report the
   ones that do not — do not repair a citation by guessing, and never edit an
   evidence file to make one resolve.
6. Check for contradictions between project notes and master notes, and between
   dismissed items and items that have reappeared. Report them; where two
   sources genuinely disagree, both stay recorded along with the disagreement.
7. **Append** an entry to `status/audit-log.md`. Append-only — never edit a
   prior entry.
8. Refresh the core-memory block.

While re-sorting task lists in step 4, keep position meaningful: the `## Open`
table is ranked by position and row 1 carries the `**(Top priority)**` marker.
An audit is exactly when a list that has drifted out of order gets caught.

---

## Definition of done

- [ ] Master and every Active project's notes re-examined
- [ ] Long-open items surfaced for decision, not silently closed
- [ ] Forgotten commitments raised
- [ ] Master task list trimmed
- [ ] Citations verified to resolve
- [ ] Entry appended to the audit log
- [ ] Core-memory block refreshed
