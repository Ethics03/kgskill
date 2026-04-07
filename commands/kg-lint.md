---
description: Health check the Knowledge Graph - find orphans, contradictions, gaps, stale claims
---

Health check the Knowledge Graph. Find orphans, contradictions, missing cross-references, stale claims, and data gaps. If `fix` was passed as an argument, auto-fix simple issues.

## Steps

**1. Resolve KG path**

Check in order: `$MYKG_PATH` env var → `~/.config/mykg/config` → `~/.mykgrc` → common locations (`~/MyKG`, `~/Dev/Obsidian/MyKG`). If not found, tell the user to run `/kg-setup` first.

**2. Orphan check**

Read all `.md` files under `wiki/`. For each page, check whether any other wiki page links to it via `[[PageName]]`. Report pages with no inbound links.

**3. Contradiction check**

Scan wiki pages for explicit contradiction markers (`contradicts`, `conflicts`, `vs`, `but`). Also read related entity/concept pages and flag where the same claim appears with different values.

**4. Missing cross-references**

Find entity or concept names that appear as plain text in wiki pages but are never linked as `[[wikilinks]]`. These are candidates for adding links.

**5. Broken wikilinks**

Find all `[[links]]` in wiki pages and check if the target file exists. Report any that point to missing pages.

**6. Stale claims**

Check source dates in frontmatter. Flag claims sourced only from old entries where newer sources on the same topic exist.

**7. Data gaps**

Read `Active.md` for open questions. Read synthesis pages for unanswered questions. Flag sparse entity/concept pages (fewer than 2 sources).

**8. Report**

Output a structured lint report:

```
# KG Lint Report

## Summary
- Orphan pages: X
- Contradictions: X
- Missing cross-refs: X
- Broken wikilinks: X
- Stale claims: X
- Data gaps: X

## Orphan Pages
- [[wiki/X]] — no inbound links

## Contradictions
- [[wiki/A]] and [[wiki/B]] conflict on <claim>

## Missing Cross-References
- "Term X" appears in [[wiki/Y]] but has no link

## Broken Wikilinks
- [[wiki/Missing]] referenced in [[wiki/Z]]

## Stale Claims
- [[concepts/X]] last sourced YYYY-MM-DD

## Data Gaps
- [[concepts/Y]] has only 1 source

## Recommendations
1. ...
```

**9. Fix mode**

If `fix` argument was given, auto-fix only:
- Add missing wikilinks for clearly matched pages
- Update `updated` frontmatter dates on modified pages

Do NOT auto-fix contradictions (require judgment) or data gaps (need new sources).
