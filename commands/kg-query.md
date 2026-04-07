---
description: Query KG (supports: active, recent, search <term>, decisions <project>, plan <project>)
---

Answer the user's question by searching and synthesizing knowledge from the wiki. This is not a grep — read pages, follow links, and produce a synthesized answer with wikilink citations.

## Steps

**1. Resolve KG path**

Check in order: `$MYKG_PATH` env var → `~/.config/mykg/config` → `~/.mykgrc` → common locations (`~/MyKG`, `~/Dev/Obsidian/MyKG`). If not found, tell the user to run `/kg-setup` first.

**2. Handle utility subcommands**

If the query is one of these, handle it directly and stop:

- `active` — read and display `Active.md`
- `recent` — read `log.md` and show the last 10 entries
- `decisions <project>` — read `<project>/Decisions/README.md` and list ADRs; offer to read any in full
- `plan <project>` — list files in `<project>/Plans/` and offer to read any

**3. For all other queries — synthesize from the wiki**

This is the main path.

**3a. Find relevant pages**

Read `index.md`. Scan the catalog for pages relevant to the query — entities, concepts, synthesis, comparisons, source summaries. Pick the most relevant ones.

**3b. Read and follow**

Read the selected pages. Follow `[[wikilinks]]` to related pages where they add depth. Keep reading until you have enough to answer well — typically 3–8 pages.

**3c. Synthesize an answer**

Write a real answer, not a list of file contents. Integrate what you found across pages. Use `[[Page Name]]` citations inline so the user knows where claims come from.

**3d. Note conflicts**

If pages contradict each other on a relevant point, surface the contradiction explicitly rather than picking one silently.

**4. Offer to file the answer back**

After answering, ask: "Should I save this as a wiki page?" Good candidates:
- A comparison or analysis that took multiple pages to synthesize
- A connection between concepts that isn't captured anywhere yet
- An answer that would save time on a future query

If yes, write it to the appropriate location (`wiki/synthesis/`, `wiki/comparisons/`, etc.), update `index.md`, and append to `log.md`:
```
## [YYYY-MM-DD] query | <Question>
- Synthesized: [[wiki/synthesis/<slug>]]
- Sources: [[wiki/X]], [[concepts/Y]]
```
