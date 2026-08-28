# Procedures

The two runnable procedures, as plain Markdown you can follow by hand or hand to
an agent:

| File | When it runs |
|---|---|
| `process-incoming.md` | On demand, whenever intake has accumulated |
| `audit-pass.md` | Manually triggered only — never scheduled |

## Using them

Copy this directory into your tree wherever your tooling looks for runnable
instructions, and keep **exactly one copy of each**. That last part is the only
real requirement: synchronized duplicates drift, always, and the drift is
silent.

These are written as tool-neutral Markdown on purpose. This system assumes no
particular agent runtime and no packaging mechanism — a procedure here may be
kept as a skill, a prompt file, a slash command, a runbook, or a printed
checklist beside your keyboard. If your tool wants a directory with a manifest
per procedure, wrap the file; the content is what matters.

Both files defer to `IMPOSTER-MEMORY.md` as the authority. If a procedure and
the spec ever disagree, the spec wins and the procedure is a bug.

## They work by hand

Nothing here requires automation. The phase separation in `process-incoming.md`
is about *what you may write down and when*, not about how many processes are
running. One person working through it in order gets the same guarantees an
agent does — including the important one, that nothing gets silently dropped.

The one thing to preserve when running it yourself is the discipline behind
phase 1: read all of intake and route it before you write anything into master
notes, so you never have to unpick a conclusion a later source invalidates.
