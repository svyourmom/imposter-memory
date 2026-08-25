Example project index. In a real tree this file sits at
`Projects/Cheerios/README.md`. Everything below is fictional.

A project README has one job: get a reader who has never seen this engagement
to the point where they can work on it. Write it for the stand-in.

---

```markdown
---
type: project
name: Cheerios
status: active
owner: TBD
intake_dir: Incoming/
living_dirs: [.]
evidence_dir: artifacts/
todo_file: action-items.md
last_updated: 2026-03-11
---

# Cheerios

Integration delivery for the client's packaging pipeline. We supply the
transform step; they own everything either side of it.

## Where it stands

In delivery. The build works end to end in staging. One open question — the
final delivery format — has been raised twice with no answer recorded, and it
blocks the packaging rebuild behind it. See `action-items.md`.

## What a stand-in needs to know first

- The client's decision-maker is not the person who runs the standups. Check
  `02-stakeholders.md` before promising anything in a call.
- The transform step was rewritten once already; the original approach is
  ruled out by a constraint recorded in `04-technical-context.md`. Read that
  before proposing a redesign.
- Owner is `TBD` in the front matter because it genuinely has not been
  assigned — not because nobody has filled it in.

## Finding your way around

`01-program-overview.md` for what this is and why,
`02-stakeholders.md` for who is involved and who decides,
`03-workstreams-and-status.md` for the current phase,
`04-technical-context.md` for systems and constraints,
`05-timeline.md` for the dated work log,
`06-risks-and-open-questions.md` for what is unresolved,
`action-items.md` for open tasks.

Sources are in `artifacts/YYYY/MM/`, filed by the date of the source document.
Nothing in there is ever edited.
```
