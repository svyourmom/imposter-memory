# Templates

The canonical skeleton to copy when creating a project. This is the thing the
spec means by "copy the standard skeleton" — real files, not a diagram.

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

`IMPOSTER-MEMORY.md` shows this at `Projects/.cursor/templates/` in its layout
diagram. That path is one tool's convention, and the spec says so. Any location
works, as long as there is exactly one canonical copy and everything that needs
it reads that copy. In this repository it sits at the root, because this
repository is documentation — not a working tree.

## A note on "project"

Throughout this system, a **project** means a `Projects/<Name>/` directory
inside an Imposter Memory tree. It never means a git repository, a GitHub
project, or any other external sense of the word.
