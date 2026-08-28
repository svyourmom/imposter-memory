# Templates

The parts of an Imposter Memory tree you copy rather than retype: the project
skeleton, and the two runnable procedures.

| | |
|---|---|
| `project-template/` | The standard project skeleton — what the spec means by "copy the standard skeleton", as real files rather than a diagram |
| `procedures/` | `process-incoming.md` and `audit-pass.md`, the two runnable procedures |

---

## The project skeleton

## Using it

```bash
cp -R templates/project-template /path/to/your-tree/Projects/<Name>
```

Then:

1. Fill in the README front matter — `name`, `owner`, `last_updated`. Unknown
   values stay `TBD`. A new project routinely has `owner: TBD`; that is a fact
   about the project, not an unfinished edit.
2. **Add the project to the registry** in your master `README.md`. It is not a
   project until that row exists.

Copy the whole directory, including the files that are empty and the two
`.gitkeep` markers. An empty `05-timeline.md` states that no timeline is known;
a missing one states nothing at all. That distinction is why the skeleton is
created all at once.

## Where this lives in your own tree

Anywhere. Neither the name nor the position is load-bearing — put both
directories where your tooling looks for them, and keep exactly one copy of
each. In this repository they sit at the root, because this repository is
documentation, not a working tree.

Nothing here assumes a particular agent runtime or packaging format. See
`procedures/README.md`.

## A note on "project"

Throughout this system, a **project** means a `Projects/<Name>/` directory
inside an Imposter Memory tree. It never means a git repository, a GitHub
project, or any other external sense of the word.
