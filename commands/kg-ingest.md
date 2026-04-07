---
description: Ingest a source document into the Knowledge Graph - extract entities, concepts, create wiki pages
---

# KG Ingest Command

Ingest a source document and integrate its knowledge into the wiki.

## Usage

`/kg-ingest <source>` where source is a file path or URL.

## Implementation

### Step 1: Resolve KG Path

Use standard resolution (MYKG_PATH, ~/.config/mykg/config, ~/MyKG, etc.)

```bash
KG_PATH=$(resolve_kg_path)
```

### Step 2: Read Source

1. If file path: read the file
2. If URL: fetch and extract text (use defuddle for web pages)
3. Store original in `raw/` with appropriate subdirectory

```bash
# Determine source type
if [[ "$SOURCE" == http* ]]; then
  # URL - fetch and extract
  # Store in raw/articles/
else
  # Local file - copy to raw/
  # Detect type: .pdf → papers/, .md → articles/, etc.
fi
```

### Step 3: Extract Knowledge

Read the source and discuss with user:

1. **Identify source type** - paper, article, transcript, notes
2. **Summarize key takeaways** - what's the main contribution?
3. **Extract entities** - people, orgs, tools, places mentioned
4. **Extract concepts** - ideas, topics, themes introduced
5. **Note contradictions** - conflicts with existing wiki pages
6. **Identify relationships** - how entities and concepts connect

### Step 4: Create Source Summary

Create `wiki/<source-name>.md`:

```markdown
# <Source Title>

## Source
- Type: Article / Paper / Transcript
- Date: YYYY-MM-DD
- Location: [[raw/<filename>]]

## Summary
<Key takeaways in 2-3 paragraphs>

## Entities Mentioned
- [[entities/Person X]] - brief context
- [[entities/Org Y]] - brief context

## Concepts
- [[concepts/Topic A]] - brief context
- [[concepts/Topic B]] - brief context

## Key Quotes
> Notable quote 1
> Notable quote 2

## Relation to Other Sources
- Agrees with [[Source B]] on X
- Contradicts [[Source C]] on Y
- Extends [[Source D]] with new evidence

## Open Questions
- Question that emerged from reading
```

### Step 5: Create/Update Entity Pages

For each entity, create or update `wiki/entities/<entity>.md`:

```markdown
# Entity Name

## Overview
Brief summary of the entity.

## Key Facts
- Fact 1
- Fact 2

## Relationships
- [[entities/Related Entity]]: relationship description

## Mentions
- [[wiki/Source A]]: what this source said
- [[wiki/Source B]]: what this source said

## Sources
- [[raw/source-1]]
- [[raw/source-2]]
```

### Step 6: Create/Update Concept Pages

For each concept, create or update `wiki/concepts/<concept>.md`:

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
- [[entities/Tool X]]: how it applies

## Sources
- [[raw/source-1]]
- [[raw/source-2]]
```

### Step 7: Note Contradictions

If new source contradicts existing knowledge:

```
⚠️ Contradiction detected:

[[concepts/Topic A]] claims X
New source [[wiki/New Source]] claims Y

Update [[concepts/Topic A]] to note the contradiction.
```

### Step 8: Update Index

Add new pages to `index.md`:

```markdown
## Entities
| [[entities/New Entity]] | Brief description | 1 |

## Concepts
| [[concepts/New Concept]] | Brief description | 1 |

## Source Summaries
| [[wiki/New Source]] | raw/article.md | YYYY-MM-DD |
```

### Step 9: Append to Log

```markdown
## [YYYY-MM-DD] ingest | <Source Title>
- Added: [[wiki/Source Summary]]
- Added: [[entities/X]], [[entities/Y]]
- Added: [[concepts/A]], [[concepts/B]]
- Updated: [[concepts/C]]
- Contradiction: [[wiki/X]] vs [[wiki/Y]]
```

### Step 10: Report Summary

```
✓ Ingested: <Source Title>

Created/Updated:
- 1 source summary
- 3 entity pages
- 2 concept pages
- 1 contradiction noted

Files:
- raw/articles/<filename>
- wiki/<source-name>.md
- wiki/entities/<entity1>.md
- wiki/entities/<entity2>.md
- wiki/concepts/<concept>.md
```