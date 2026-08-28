Example task list. In a real tree this file sits at
`Projects/Cheerios/action-items.md`. Everything below is fictional.

Open items first, ordered by priority **within this project only**. Position is
the ranking, so the table is re-sorted whenever priority changes and row 1
carries the `(Top priority)` marker. Dismissed items sit beneath, and every
dismissal names the evidence that closed it — nothing is ever deleted silently.

---

```markdown
# Cheerios — Action Items

> Last updated: 2026-03-11

## Open

| # | Item | Notes |
|---|------|-------|
| 1 | **(Top priority)** Confirm the delivery format with the client | Raised twice, no answer recorded (`2026-03-09-cheerios-sync.md`) |
| 2 | Rebuild the packaging step | Blocked until item 1 resolves |
| 3 | Name an owner for this project | Front matter reads `owner: TBD`; not yet assigned (`2026-03-10-integration-format-decision.md`) |
| 4 | Unresolved: `vendor-comparison-draft.md` left in intake | Appeared without routing. Content does not clearly relate to this engagement. Needs a human decision. |

## Dismissed

| Item | Dismissed | Evidence |
|------|-----------|----------|
| Chase the missing access request | 2026-03-11 | Access was granted 2026-03-10; the request is moot (`2026-03-10-access-granted.md`) |
| Rewrite the transform to use the streaming API | 2026-02-14 | Ruled out — client's platform does not expose it (`2026-02-14-quarterly-planning-summary.md`) |
```

---

Two things worth noticing.

**Item 4 is not a bug.** An agent that cannot confidently place a file leaves
it in intake and writes a task saying so. That is a safe resting state: the
file stays visibly unprocessed and the next run retries it. The failure mode
is a visible gap, not a silent one.

**There is no due date on item 1**, even though it is the most time-pressured
thing here. Urgency shows up as rank, never as a date field — a memory records
that a commitment exists; a reminder system is what interrupts you on the day.

**Nothing ages out.** Items 1 and 3 could sit here for weeks without moving.
They leave only when something actively closes them. Surfacing long-quiet
items for a decision is the audit pass's job, not the intake run's.
