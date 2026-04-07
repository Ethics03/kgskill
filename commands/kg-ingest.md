---
description: Ingest a source document into the Knowledge Graph - extract entities, concepts, create wiki pages
---

Ingest the provided source into the Knowledge Graph and integrate its knowledge into the wiki.

## Steps

**1. Resolve KG path**

Check in order: `$MYKG_PATH` env var → `~/.config/mykg/config` → `~/.mykgrc` → common locations (`~/MyKG`, `~/Dev/Obsidian/MyKG`). If not found, tell the user to run `/kg-setup` first.

**2. Read the source**

- If a file path: read it directly. Copy it to `raw/` under the appropriate subdirectory (`papers/` for PDFs, `articles/` for markdown/text, `transcripts/` for interviews/talks, `notes/` for everything else). Preserve the original filename.
- If a URL: use the `defuddle` skill if available, otherwise WebFetch. Save the result as markdown to `raw/articles/<slug>.md` where `<slug>` is a kebab-case version of the page title (e.g. `attention-is-all-you-need.md`). Write the source URL as the first line: `> Source: <url>`

**3. Extract knowledge**

Read the source and identify:
- Key takeaways and main contribution
- **Entities** — people, orgs, tools, places mentioned
- **Concepts** — ideas, topics, themes introduced
- **Contradictions** — anything that conflicts with existing wiki pages
- **Relationships** — how entities and concepts connect

**4. Create source summary**

Write `wiki/<source-slug>.md`:

```
# <Source Title>

## Source
- Type: Article / Paper / Transcript / Notes
- Date: YYYY-MM-DD
- Location: [[raw/<filename>]]

## Summary
<Key takeaways in 2-3 paragraphs>

## Entities Mentioned
- [[entities/X]] - brief context

## Concepts
- [[concepts/Y]] - brief context

## Key Quotes
> Notable quote

## Relation to Other Sources
- Agrees with [[Source B]] on X
- Contradicts [[Source C]] on Y

## Open Questions
- Question that emerged from reading
```

**5. Create or update entity pages**

For each entity, create or update `wiki/entities/<entity>.md` with: overview, key facts, relationships, mentions, sources.

**6. Create or update concept pages**

For each concept, create or update `wiki/concepts/<concept>.md` with: definition, key points, related concepts, applications, sources.

**7. Flag contradictions**

If the source conflicts with existing wiki content, note it inline on the relevant page and surface it clearly to the user.

**8. Update `index.md`**

Add any new pages to the appropriate tables (Entities, Concepts, Source Summaries).

**9. Append to `log.md`**

```
## [YYYY-MM-DD] ingest | <Source Title>
- Added: [[wiki/<source-slug>]]
- Added: [[entities/X]], [[concepts/Y]]
- Updated: [[concepts/Z]]
- Contradiction: [[wiki/A]] vs [[wiki/B]]
```

**10. Report**

Summarize what was created/updated: source summary, entity pages, concept pages, contradictions noted.
