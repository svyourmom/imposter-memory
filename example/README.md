# Example layout

A complete Imposter Memory tree, with three fictional projects — **Cheerios**,
**Trix**, and **Fruit Loops** — plus one archived one, **Honeycomb**. Nothing
here refers to anything real.

Copy this shape. The consistency is the point: an agent or a human opening any
project finds the same files in the same places.

## The tree

```text
/
├── README.md                  # Master index: core-memory block + registry
├── AGENTS.md                  # The operating contract
├── IMPOSTER-MEMORY.md         # The full specification
│
├── incoming/                  # Master intake — transient, should empty out
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
├── artifacts/                 # Master evidence — immutable
│   └── 2026/
│       ├── 02/
│       │   ├── 2026-02-14-quarterly-planning-summary.md
│       │   └── Vendor renewal notice.eml
│       └── 03/
│           ├── 2026-03-10-weekly-leadership-sync.md
│           ├── Weekly leadership sync-transcript.txt
│           └── Industry newsletter March.eml
│
├── Projects/
│   ├── README.md              # Portfolio index (registry lives at root)
│   ├── Cheerios/
│   ├── Trix/
│   ├── FruitLoops/
│   └── Honeycomb/             # Archived — no agent runs, no routing
│
└── procedures/                # One canonical copy of each procedure
    ├── process-incoming.md
    └── audit-pass.md
```

Each project is identical in shape:

```text
Projects/Cheerios/
├── README.md                        # Front matter declares this project's structure
├── Incoming/                        # Project intake
│
├── 01-program-overview.md           # What this is and where it stands
├── 02-stakeholders.md               # Who is involved and who decides
├── 03-workstreams-and-status.md     # Current phase, what is moving
├── 04-technical-context.md          # Systems, constraints, reference detail
├── 05-timeline.md                   # Dated work log and milestones
├── 06-risks-and-open-questions.md   # What is unresolved
├── action-items.md                  # This project's task list
│
└── artifacts/                       # Project evidence — immutable
    └── 2026/
        └── 03/
            ├── Cheerios daily status - Mar 10.eml
            └── 2026-03-10-integration-format-decision.md
```

Create the **full skeleton at once**, including files that will be empty. An
empty `05-timeline.md` says no timeline is known. A missing one says nothing at
all.

## The files in this folder

| File | What it demonstrates |
|---|---|
| `master-README.md` | Master front matter, the pinned core-memory block, the registry table |
| `project-README.md` | Project front matter, and how a project introduces itself to a cold reader |
| `project-action-items.md` | Open/Dismissed task list shape, with cited dismissals |
| `living-note.md` | A living note with its freshness marker and source citations |

They are shown here as flat files for readability. In a real tree they sit at
the paths named above.

## Where to start reading

Start at the master `README.md` — the core-memory block at the top is written
to be the first thing anyone reads, including someone picking the work up cold.
Then the registry, then the project you need.
