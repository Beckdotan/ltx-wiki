---
description: Run a health check on the LTX wiki following the Karpathy LLM Wiki pattern. Finds contradictions, stale claims, orphan pages, missing pages, broken wikilinks, missing cross-references, data gaps, and index drift. Use when the user says "lint the wiki", "health check", "check wiki consistency", "find wiki problems", or periodically as the wiki grows.
---

# Wiki Lint

Health-check the wiki for consistency, completeness, and quality. This is maintenance that keeps the wiki useful as it grows.

**Working directory:** The current wiki project root (contains `raw/`, `wiki/`, `CLAUDE.md`).

## Arguments

`$ARGUMENTS` — Optional scope limiter (e.g. "architecture pages only", "competitors section", "just check links"). If omitted, lint the entire wiki.

## Checklist

Run through each check systematically. Use subagents for large wikis (200+ pages).

### 1. Broken Wikilinks

Grep all wiki pages for `[[wikilink]]` patterns. For each link, verify the target file exists in `wiki/`.

```
grep -oP '\[\[([^\]|]+)' wiki/*.md | sort -u
```

Report: list of broken links with the page they appear on.

### 2. Orphan Pages

Find wiki pages with zero inbound `[[wikilinks]]` from other pages. These are disconnected from the knowledge graph.

For each orphan: recommend either linking it from relevant pages or merging/deleting it.

### 3. Missing Pages

Find `[[wikilinks]]` that point to pages that don't exist yet. These are concepts/entities mentioned but not yet documented.

For each missing page: assess if it should be created (important concept) or the link should be removed/reworded.

### 4. Contradictions

Read pages that cover overlapping topics and flag conflicting claims. Focus on:
- Version numbers and dates
- Performance numbers (VRAM, speed, resolution)
- Feature availability claims
- Pricing information

Report with specific quotes from each contradicting page.

### 5. Stale Claims

Check if newer sources have superseded older claims. Look for:
- Pages referencing old model versions as "latest"
- Pricing that may have changed
- Feature availability that has evolved
- Community metrics that have grown

### 6. Missing Cross-References

Find pages that discuss related topics but don't link to each other. Look for:
- Pages mentioning the same entity without linking
- Concept pages that should reference each other
- Competitor pages that should link to the features they compare against

### 7. Index Drift

Compare `wiki/index.md` against actual wiki files:
- Pages that exist but aren't in the index
- Index entries pointing to non-existent pages
- Pages in the wrong category
- Missing or outdated one-line summaries

### 8. Format Compliance

Check pages against CLAUDE.md conventions:
- Missing YAML frontmatter fields (title, type, created, updated, sources, tags)
- Non-standard `type` values (should be: source, entity, concept, comparison, analysis, overview, reference, guide, competitor, paper, community-project, technical, product)
- Missing `## References` section
- Filenames not lowercase-hyphenated
- Pages over 300 lines that should be split

### 9. Data Gaps

Based on the wiki's coverage, identify:
- Important questions the wiki can't answer
- Topics mentioned frequently but lacking depth
- Competitor information that seems incomplete
- Suggest specific sources to look for

## Output

Present findings as a structured report:

```markdown
# Wiki Lint Report — YYYY-MM-DD

## Summary
- X broken links
- X orphan pages
- X missing pages
- X contradictions
- X format issues

## Critical (fix now)
[List items that cause confusion or broken navigation]

## Warning (fix soon)
[List items that reduce wiki quality]

## Info (nice to fix)
[List minor issues and suggestions]

## Suggested New Sources
[Topics where research would fill gaps]
```

After linting, append to `wiki/log.md`:
```
## [YYYY-MM-DD] lint | Wiki health check
Summary: X issues found. Critical: X, Warning: X, Info: X. [Brief description of top issues.]
```

## Batch Mode

For large wikis (200+ pages), use 3-5 subagents:
- Agent 1: Broken links + orphans + missing pages (structural)
- Agent 2: Contradictions + stale claims (content)
- Agent 3: Cross-references + index drift (navigation)
- Agent 4: Format compliance (conventions)
- Agent 5: Data gaps (completeness)

Consolidate results into a single report.
