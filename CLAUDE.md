# LTX Wiki

An LLM-maintained knowledge base about [LTX Video](https://github.com/Lightricks/LTX-Video) by Lightricks, built using the [Karpathy LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

The LLM builds and maintains a structured, interlinked wiki from raw sources. The human curates sources, asks questions, and directs analysis. The LLM handles summarization, cross-referencing, filing, and bookkeeping.

## Architecture

| Layer | Directory | Owner | Description |
|-------|-----------|-------|-------------|
| Raw sources | `raw/` | Human | Immutable source documents. The LLM reads but **never modifies** these. |
| Wiki | `wiki/` | LLM | Generated markdown pages — summaries, entities, concepts, comparisons, synthesis. |
| Graph | `graphify-out/` | LLM | Knowledge graph — interactive HTML, JSON, and audit report via [graphify](https://github.com/safishamsi/graphify). |
| Schema | `CLAUDE.md` | Human + LLM | This file. Conventions, workflows, and structure rules. |

## Directory Layout

```
raw/                 # 239 source documents — never modified by LLM
  assets/            # Downloaded images, PDFs, data files
wiki/                # 213 LLM-generated wiki pages
  index.md           # Content catalog (by category)
  log.md             # Chronological operation log (append-only)
graphify-out/        # Knowledge graph outputs
  graph.html         # Interactive visualization (open in browser)
  graph.json         # Raw graph data (772 nodes, 1038 edges)
  GRAPH_REPORT.md    # Audit report with god nodes, surprises, communities
.claude/skills/      # Wiki slash commands (auto-available in Claude Code)
```

## Slash Commands

Four skills implement the Karpathy wiki operations:

- `/wiki-research` — Research a topic online, save findings as raw source files in `raw/`
- `/wiki-ingest` — Process raw sources into wiki pages with cross-references
- `/wiki-query` — Answer questions from compiled wiki knowledge, optionally file answers
- `/wiki-lint` — Health-check the wiki (contradictions, orphans, broken links, gaps)

Typical flow: `/wiki-research` → `/wiki-ingest` → `/wiki-query` → `/wiki-lint`

Use `/graphify` to rebuild the knowledge graph after major wiki changes.

## Page Format

Every wiki page uses this structure:

```markdown
---
title: Page Title
type: source | entity | concept | comparison | analysis | overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - raw/filename.md
tags:
  - tag1
---

# Page Title

Content here. Use [[wikilinks]] to link to other wiki pages.

## References

- [[source-summary]] — what was drawn from this source
```

### Conventions

- Use `[[wikilinks]]` for internal wiki links (Obsidian-compatible).
- File names: lowercase, hyphenated (e.g. `wiki/video-vae.md`).
- Citations: note inline (e.g. "According to [[source-name]], ...").
- One entity/concept per page. Split when a page grows beyond ~300 lines.

## Workflows

### Ingest

1. **Read** the source document in full.
2. **Discuss** key takeaways with the user.
3. **Create a source summary page** in `wiki/` (type: source).
4. **Update existing pages** affected by the new source.
5. **Create new pages** for entities/concepts not yet covered.
6. **Update `wiki/index.md`** with new entries.
7. **Append to `wiki/log.md`** with date and pages touched.

### Query

1. **Read `wiki/index.md`** to identify relevant pages.
2. **Read the relevant wiki pages** (wiki is compiled knowledge; raw is ground truth).
3. **Synthesize an answer** with citations to wiki pages.
4. **Optionally file the answer** as a new wiki page if it's valuable synthesis.

### Lint

- [ ] **Contradictions** — conflicting claims across pages
- [ ] **Stale claims** — superseded by newer sources
- [ ] **Orphan pages** — no inbound [[wikilinks]]
- [ ] **Missing pages** — wikilinks pointing to non-existent pages
- [ ] **Missing cross-references** — related pages not linking to each other
- [ ] **Data gaps** — important questions the wiki can't answer
- [ ] **Index drift** — `wiki/index.md` out of sync with actual pages

### Index and Log Rules

**wiki/index.md** — catalog by category, `- [[page-name]] — one-line summary`, updated on every ingest.

**wiki/log.md** — append-only, `## [YYYY-MM-DD] verb | Subject`, parseable with grep.

## Tips

- For large sources, read in sections.
- Prefer adding to existing pages over creating new ones until they get too long.
- Always check for existing pages before creating new ones.
- Use graphify's query commands for cross-document discovery: `/graphify query "question"`, `/graphify path "A" "B"`, `/graphify explain "concept"`.
