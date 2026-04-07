# Log

Chronological record of changes. Append-only.

## Format

Each entry starts with: `## [YYYY-MM-DD] <type> | <title>`

Types: `ingest`, `query`, `lint`, `project`

Entries are parseable: `grep "^## \[" log.md | tail -5`

---

## [YYYY-MM-DD] project | Initialize KG

Created Knowledge Graph with initial structure.

- Added: [[index]]
- Added: [[log]]
- Added: [[Active.md]]
- Added: [[AGENTS.md]]