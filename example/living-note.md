Example living note. In a real tree this file sits at
`Projects/Cheerios/04-technical-context.md`. Everything below is fictional.

Living notes hold the *current* understanding and are rewritten freely. The
freshness marker beneath the title is the only mandatory element. Every claim
names the evidence file it came from, by bare filename — never a path.

---

```markdown
# Cheerios — Technical Context

> Last updated: 2026-03-11

## What we supply

A transform step between the client's ingest and their packaging pipeline. We
own the transform only; they own both sides of it
(`2026-02-14-quarterly-planning-summary.md`).

## Constraints

- **No streaming API.** The client's platform does not expose one, so the
  transform runs batch. This ruled out the original streaming design
  (`2026-02-14-quarterly-planning-summary.md`).
- **Delivery format is unconfirmed.** Two candidates were discussed and
  neither was chosen on the call. Raised twice with no answer recorded
  (`2026-03-09-cheerios-sync.md`). Do not build to either until this closes —
  see `action-items.md` item 1.
- **Batch window: TBD.** Mentioned as "overnight" in passing, never confirmed
  with a time (`2026-03-09-cheerios-sync.md`).

## Environments

Staging works end to end. Production access has not been requested yet; the
owner for that request is `TBD` (`2026-03-10-integration-format-decision.md`).
```

---

Three habits this note demonstrates.

**`TBD` beats a guess.** The batch window is written as unconfirmed rather
than filled in with a plausible time. One week later, a guess is
indistinguishable from a fact.

**Citations are bare filenames.** Evidence storage can be reorganized without
breaking a single reference.

**It contradicts nothing quietly.** Where the source was vague, the note says
the source was vague, and points at the open task that will resolve it.
