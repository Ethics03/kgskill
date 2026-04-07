# KG Schema

Instructions for the LLM on how to maintain this Knowledge Graph.

## Purpose

This wiki accumulates knowledge from sources over time. The LLM reads sources, extracts knowledge, updates wiki pages, and maintains cross-references.

## Structure

```
MyKG/
├── index.md              ← START HERE. Catalog of all pages.
├── log.md                ← Chronological change history. Append-only.
├── Active.md             ← Current focus items.
├── AGENTS.md             ← This file. Schema and workflows.
├── raw/                  ← Source documents. Immutable.
│   ├── articles/
│   ├── papers/
│   └── transcripts/
├── wiki/                 ← LLM-generated wiki pages.
│   ├── entities/         ← People, orgs, tools, places.
│   ├── concepts/         ← Ideas, topics, themes.
│   ├── synthesis/        ← High-level summaries.
│   └── comparisons/      ← Side-by-side analyses.
└── <Project>/            ← Project-specific workspaces.
    ├── SKILL.md          ← Project instructions.
    ├── AGENTS.md         ← Project schema.
    ├── raw/
    └── wiki/
```

## Page Types

### Entity Pages (`wiki/entities/`)
Pages for specific entities: people, organizations, tools, places.

```markdown
# Entity Name

## Overview
Brief summary of the entity.

## Key Facts
- Fact 1
- Fact 2

## Relationships
- [[Related Entity]]: relationship description

## Sources
- [[raw/source-1]]
- [[raw/source-2]]

## Mentions
- [[wiki/concepts/Topic A]]
- [[wiki/synthesis/Overview]]
```

### Concept Pages (`wiki/concepts/`)
Pages for ideas, topics, themes, frameworks.

```markdown
# Concept Name

## Definition
What is this concept?

## Key Points
- Point 1
- Point 2

## Related Concepts
- [[concepts/Related Concept]]: how it relates

## Applications
- [[entities/Tool X]]: how it uses this concept

## Sources
- [[raw/source-1]]
```

### Synthesis Pages (`wiki/synthesis/`)
High-level summaries integrating multiple sources.

```markdown
# Topic Overview

## Summary
Synthesis of knowledge from multiple sources.

## Key Themes
- Theme 1
- Theme 2

## Open Questions
- Question 1

## Sources
- [[Source Summary 1]]
- [[Source Summary 2]]
```

### Comparison Pages (`wiki/comparisons/`)
Side-by-side analyses.

```markdown
# A vs B

## Overview
What's being compared and why.

## Comparison

| Aspect | A | B |
|--------|---|---|
| X | | |
| Y | | |

## Takeaways
- Key insight 1

## Sources
- [[raw/source-1]]
```

### Source Summaries (`wiki/`)
Summary of a single source.

```markdown
# Source Title

## Source
- Type: Article / Paper / Transcript
- Date: YYYY-MM-DD
- Location: [[raw/filename.md]]

## Summary
Key takeaways from the source.

## Entities Mentioned
- [[entities/Person X]]
- [[entities/Org Y]]

## Concepts
- [[concepts/Topic A]]

## Quotes
> Notable quote from source.

## Relation to Other Sources
- Agrees with [[Source B]] on X
- Contradicts [[Source C]] on Y
```

## Ingest Workflow

When ingesting a source:

1. **Read** the source document
2. **Discuss** key takeaways with user
3. **Create** source summary in `wiki/`
4. **Extract entities** — create/update pages in `wiki/entities/`
5. **Extract concepts** — create/update pages in `wiki/concepts/`
6. **Note contradictions** — if new source contradicts existing knowledge
7. **Update synthesis** — if high-level summary needs updating
8. **Update index** — add new pages to `index.md`
9. **Append to log** — record what was ingested

A single source might touch 10-15 wiki pages.

## Query Workflow

When answering questions:

1. **Read index** to find relevant pages
2. **Drill into** specific pages
3. **Synthesize** answer with citations `[[Page Name]]`
4. **Optionally save** answer as new wiki page

Good answers can be filed back into the wiki. This way queries compound knowledge too.

## Lint Workflow

Periodically check wiki health:

- **Orphan pages** — no inbound links
- **Contradictions** — conflicting claims between pages
- **Missing cross-references** — pages that should link but don't
- **Stale claims** — superseded by newer sources
- **Entity pages without sources**
- **Concepts mentioned but not defined**

## Conventions

### Wikilinks
- `[[Page Name]]` — internal link
- `[[Page Name#Section]]` — deep link
- `[[Page Name|Display Text]]` — alias
- `[[Project/wiki/Page]]` — cross-project link

### Frontmatter
```yaml
---
tags:
  - entity | concept | synthesis | comparison | source-summary
  - category
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - raw/source-name.md
related:
  - [[Related Page]]
---
```

### File Naming
- Lowercase with hyphens: `entity-name.md`
- Place in appropriate subdirectory
- Raw sources: preserve original name or slugify

### Log Format
```markdown
## [YYYY-MM-DD] ingest | Source Title
- Added: [[entities/X]], [[concepts/Y]]
- Updated: [[synthesis/Z]]
- Contradiction: [[concepts/A]] vs [[concepts/B]]
```

## Maintenance

After each session:
1. Update `log.md` with changes
2. Update `index.md` for new pages
3. Update `Active.md` if priorities changed

Weekly:
1. Run lint check for orphans
2. Review synthesis pages for completeness

Monthly:
1. Archive stale content
2. Review open questions