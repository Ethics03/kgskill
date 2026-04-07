---
description: Create a new project in the Knowledge Graph with ADR tracking
---

Create a new project folder in the Knowledge Graph. The argument is the project name; an optional second argument is a description.

## Steps

**1. Resolve KG path**

Check in order: `$MYKG_PATH` env var → `~/.config/mykg/config` → `~/.mykgrc` → common locations (`~/MyKG`, `~/Dev/Obsidian/MyKG`). If not found, tell the user to run `/kg-setup` first.

**2. Validate project name**

If no name was provided, tell the user: `Usage: /kg-project <name> [description]` and stop.

Normalize the name: replace spaces with hyphens, keep only alphanumeric and hyphens.

**3. Check if already exists**

If `<KG_PATH>/<project-name>/` already exists, tell the user and stop.

**4. Create project structure**

Run:
```bash
mkdir -p <KG_PATH>/<name>/{Architecture,Decisions,Plans,Analysis}
```

**5. Write `Decisions/README.md`**

```
# Decisions

Architecture Decision Records (ADRs) for <name>.

## Index

| ADR | Title | Status | Date |
|-----|-------|--------|------|

## Next ADR Number

**Next: 001**

## Creating a New ADR

1. Update the number above
2. Create `ADR-XXX - Title.md`
3. Fill in: Context, Decision, Consequences, Alternatives
4. Update this index
5. Update root `index.md`
6. Add entry to root `log.md`
```

**6. Write `SKILL.md`**

```
---
name: <name>-skill
description: Project-specific instructions for <name>. Use when working on this project.
---

# <name>

## Project Overview

<description>

## Tech Stack

| Component | Technology |
|-----------|------------|

## Key Architecture Decisions

Refer to ADRs in [[Decisions/]].

## Current Priorities

See [[Active]] for current focus items.

## Project Conventions

- [Add conventions]

## Key Files

| File | Purpose |
|------|---------|
| `Architecture/Overview.md` | System design |
| `Decisions/README.md` | ADR index |
```

**7. Update root `index.md`**

Add the project to the Projects table:
```
| [[<name>/]] | <description> |
```

**8. Append to root `log.md`**

```
## [YYYY-MM-DD] project | Create <name>
- Created project [[<name>/]]
```

**9. Report**

Tell the user the project was created, show the path and structure, and suggest next steps: edit `SKILL.md` with project details, create `Architecture/Overview.md`.
