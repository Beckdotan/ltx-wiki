---
description: Ingest new raw sources into the LTX wiki following the Karpathy LLM Wiki pattern. Use when the user adds new files to raw/ and says "ingest", "process this source", "add to wiki", or drops a new article/paper/document. Reads raw sources, creates wiki pages, updates existing pages, maintains index and log.
---

# Wiki Ingest

Process new raw source documents into the wiki. This is the core operation of the Karpathy LLM Wiki pattern — transforming raw sources into structured, interlinked wiki knowledge.

**Working directory:** The current wiki project root (contains `raw/`, `wiki/`, `CLAUDE.md`).

## Arguments

`$ARGUMENTS` — Optional path(s) to specific raw files to ingest. If omitted, detect new/unprocessed files in `raw/`.

## Workflow

### 1. Identify Sources

If specific files are given in `$ARGUMENTS`, use those. Otherwise:
- Read `wiki/log.md` to see what's already been ingested
- Scan `raw/` for files not yet referenced in any wiki page's `sources:` frontmatter
- Present the list to the user and confirm which to ingest

### 2. Read Sources

Read each raw source document in full. For large files, read in sections.

### 3. Discuss Key Takeaways

Before writing wiki pages, briefly summarize to the user:
- What the source covers
- Key claims, data points, or insights
- How it relates to existing wiki content (check `wiki/index.md` first)
- Any contradictions with existing wiki pages

Ask the user what to emphasize or de-emphasize.

### 4. Create/Update Wiki Pages

For each source:

**a. Create a source summary page** (if the source is substantial enough):
```markdown
---
title: Source Title
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - raw/filename.md
tags:
  - relevant-tags
---
# Source Title
Summary content with [[wikilinks]].
## References
- [[related-page]] — connection description
```

**b. Update existing wiki pages** that are affected by the new information:
- Read relevant wiki pages (find them via `wiki/index.md` or grep)
- Add new information, note agreements or contradictions
- Add the new raw source to the page's `sources:` list
- Update the `updated:` date

**c. Create new entity/concept pages** if the source introduces topics without their own page.

### 5. Update Index

Add new pages to the appropriate category in `wiki/index.md`:
```
- [[page-name]] — one-line summary
```

### 6. Update Log

Append to `wiki/log.md`:
```
## [YYYY-MM-DD] ingest | Source Title
Summary of what was done. Pages created: [[page1]], [[page2]]. Pages updated: [[page3]].
```

## Page Format Rules

- Filenames: lowercase, hyphenated (e.g. `wiki/transformer-architecture.md`)
- YAML frontmatter: title, type, created, updated, sources, tags
- Types: `source | entity | concept | comparison | analysis | overview`
- Use `[[wikilinks]]` liberally for cross-references
- One concept per page — split if over ~300 lines
- Always check for existing pages before creating new ones

## Batch Mode

For many sources at once, use subagents:
- Group sources thematically (~10-15 per agent)
- Each agent creates wiki pages but does NOT touch `index.md` or `log.md`
- Consolidate index and log after all agents complete
