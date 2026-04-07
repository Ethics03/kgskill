---
description: Health check the Knowledge Graph - find orphans, contradictions, gaps, stale claims
---

# KG Lint Command

Check wiki health: find orphans, contradictions, missing cross-references, and gaps.

## Usage

`/kg-lint [fix]`

Add `fix` to automatically fix simple issues.

## Implementation

### Step 1: Resolve KG Path

```bash
KG_PATH=$(resolve_kg_path)
```

### Step 2: Orphan Check

Find pages with no inbound links:

```bash
# Find all wiki pages
find "$KG_PATH/wiki" -name "*.md" -type f

# For each page, check if any other page links to it
# Orphan if no inbound links found
```

Report:
```
## Orphan Pages

These pages have no inbound links:

- [[wiki/entities/X]] - consider adding links from related pages
- [[wiki/concepts/Y]] - consider adding links from related pages
```

### Step 3: Contradiction Check

Find pages with potential contradictions:

Look for:
- Same entity/concept described differently
- Conflicting claims between sources
- Outdated information

```bash
# Search for contradiction markers
grep -r "contradicts\|conflicts\|vs\|but" "$KG_PATH/wiki"
```

Report:
```
## Contradictions

These pages have conflicting information:

- [[wiki/entities/X]]: Claim A vs [[wiki/entities/Y]]: Claim B
- [[wiki/concepts/Z]]: Sources disagree on key point
```

### Step 4: Missing Cross-References

Find pages that should link but don't:

```bash
# For each entity, check if concept pages mention it
# For each concept, check if entity pages use it
```

### Step 5: Stale Claims

Find claims that may be outdated:

```bash
# Check source dates
# Flag claims from old sources
# Note if newer sources exist on same topic
```

### Step 6: Missing Pages

Find concepts/entities mentioned but not defined:

```bash
# Find all wikilinks
grep -roh '\[\[[^]]+\]\]' "$KG_PATH/wiki"

# Check if target exists
# Report missing pages
```

### Step 7: Data Gaps

Find areas that need more sources:

```bash
# Check Active.md for questions to investigate
# Check synthesis pages for open questions
# Note sparse entity/concept pages
```

### Step 8: Generate Report

```
# KG Lint Report

## Summary
- Orphan pages: X
- Contradictions: X
- Missing cross-refs: X
- Stale claims: X
- Missing pages: X
- Data gaps: X

## Details

### Orphan Pages
[List]

### Contradictions
[List]

### Missing Cross-References
[List]

### Stale Claims
[List]

### Missing Pages
[List]

### Data Gaps
[List]

## Recommendations

1. [Action item 1]
2. [Action item 2]
```

### Step 9: Fix Mode (Optional)

If `fix` argument provided:

```bash
# Auto-fix simple issues:
# - Add missing cross-references
# - Update last modified dates
# - Merge duplicate mentions
```

Do NOT auto-fix:
- Contradictions (require human judgment)
- Data gaps (need new sources)
- Complex cross-references