---
description: Initialize a new Knowledge Graph with raw/ and wiki/ structure
---

Initialize a new Knowledge Graph at the path provided. If no path was given, check `MYKG_PATH` env var, then `~/.config/mykg/config`, then default to `~/MyKG`.

## Steps

**1. Resolve target path**

Use the first available source:
1. Argument passed to this command
2. `$MYKG_PATH` environment variable
3. Contents of `~/.config/mykg/config`
4. Default: `~/MyKG`

**2. Check if already exists**

If the target directory contains an `index.md`, tell the user the KG already exists and suggest `/kg-setup` to reconfigure. Stop.

**3. Create directory structure**

Run:
```bash
mkdir -p <path>/raw/{articles,papers,transcripts,notes}
mkdir -p <path>/wiki/{entities,concepts,synthesis,comparisons}
```

**4. Create core files**

Write these four files at the target path:

`index.md`:
```
# Index

Catalog of all wiki pages. The LLM reads this first to find relevant content.

## Entities

Pages for specific entities: people, organizations, tools, places.

| Page | Summary | Sources |
|------|---------|---------|

## Concepts

Pages for ideas, topics, themes, frameworks.

| Page | Summary | Sources |
|------|---------|---------|

## Synthesis

High-level summaries and overviews.

| Page | Summary | Sources |
|------|---------|---------|

## Comparisons

Side-by-side analyses and trade-offs.

| Page | Summary | Sources |
|------|---------|---------|

## Source Summaries

| Page | Source | Date |
|------|--------|------|

## Projects

| Project | Description |
|---------|-------------|
```

`log.md`:
```
# Log

Chronological record of changes. Append-only.

Each entry: `## [YYYY-MM-DD] <type> | <title>`
Types: `ingest`, `query`, `lint`, `project`

---

## [<today's date>] project | Initialize KG

Created Knowledge Graph with initial structure.

- Added: [[index]]
- Added: [[log]]
- Added: [[Active]]
- Added: [[AGENTS]]
```

`Active.md`:
```
# Active

Current focus items and priorities.

## Focus Areas

| Priority | Area | Status | Next Step |
|----------|------|--------|-----------|

## Questions to Investigate

-

## Sources to Find

-

## Completed

| Date | Item | Notes |
|------|------|-------|
```

`AGENTS.md`: copy the full content from the AGENTS.md template in the kgskills repo at `templates/kg-init/AGENTS.md` if it exists, otherwise write a minimal version with structure, page types, and workflow instructions.

**5. Save config**

```bash
mkdir -p ~/.config/mykg
echo "<path>" > ~/.config/mykg/config
```

**6. Report success**

Tell the user the KG was created, show the path and structure, and suggest next steps: `/kg-ingest <file>`, `/kg-project <name>`.
