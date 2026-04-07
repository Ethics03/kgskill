# Concepts

The MyKG skill follows the **LLM Wiki pattern**.

## Three Layers

```
┌─────────────────────────────────────────────┐
│              Raw Sources (immutable)         │
│  Papers, articles, transcripts, notes, URLs │
└──────────────────────┬──────────────────────┘
                       │ ingest
                       ▼
┌─────────────────────────────────────────────┐
│              Wiki (LLM-maintained)           │
│  Summaries, entity pages, concepts,         │
│  synthesis, cross-references                │
└──────────────────────┬──────────────────────┘
                       │ query
                       ▼
┌─────────────────────────────────────────────┐
│              Schema (configuration)          │
│  AGENTS.md — tells the LLM how to maintain  │
└─────────────────────────────────────────────┘
```

### Why This Pattern?

Most RAG systems retrieve from raw documents every query. The LLM rediscovers knowledge from scratch each time.

This pattern is different: **the wiki is a persistent, compounding artifact.**

- Cross-references are already there
- Contradictions are already flagged
- Synthesis already reflects everything read
- The wiki keeps getting richer with every source

The LLM does the grunt work: summarizing, cross-referencing, filing, bookkeeping. You curate sources and ask questions.

## Operations

### Ingest

Read source → extract knowledge → update wiki:

1. Read the source document
2. Discuss key takeaways
3. Create source summary in `wiki/`
4. Create/update entity pages in `wiki/entities/`
5. Create/update concept pages in `wiki/concepts/`
6. Note contradictions with existing pages
7. Update `index.md`
8. Append to `log.md`

A single source can touch 10-15 wiki pages. Knowledge compounds.

### Query

Search wiki → synthesize answer with citations:

1. Read `index.md` to find relevant pages
2. Drill into specific pages
3. Synthesize answer with `[[citations]]`
4. **Good answers can be saved as new pages** — queries compound too

### Lint

Health check the wiki:

- Orphan pages (no inbound links)
- Contradictions between pages
- Missing cross-references
- Stale claims
- Concepts mentioned but not defined

## Two-Tier Skills

### Universal Skill

Loaded once. Teaches the agent:
- KG structure and conventions
- Ingest, query, lint operations
- Standard page types
- Wikilink syntax

### Project Skill

Loaded per-project. Customizes for domain:
- What entities and concepts are relevant
- Custom ingestion workflows
- Domain-specific terminology
- Project-specific page types

Example: Reading a book might have custom page types for characters, themes, plot threads.

## Key Insight

The tedious part of maintaining a knowledge base is bookkeeping — updating cross-references, keeping summaries current, noting contradictions. LLMs don't get bored. They can touch 15 files in one pass.

Your job: curate sources, direct analysis, ask good questions.

LLM's job: everything else.