---
description: Query the LTX wiki to answer questions, synthesize knowledge, and optionally file answers as new wiki pages. Use when the user asks a question about wiki content, wants analysis, comparisons, or summaries drawn from the knowledge base. Triggers on "query the wiki", "what does the wiki say about", "search the wiki", "answer from wiki", or any question that should be answered from compiled wiki knowledge.
---

# Wiki Query

Answer questions by reading the compiled wiki — not raw sources. The wiki is the compiled knowledge; raw sources are the ground truth only when deeper detail is needed.

**Working directory:** The current wiki project root (contains `raw/`, `wiki/`, `CLAUDE.md`).

## Arguments

`$ARGUMENTS` — The question or topic to investigate.

## Workflow

### 1. Read the Index

Start by reading `wiki/index.md` to identify relevant pages. This is the primary navigation tool.

### 2. Read Relevant Pages

Read the wiki pages identified from the index. Follow `[[wikilinks]]` to discover related pages. Read as many as needed to build a complete answer.

If wiki pages are insufficient, read the referenced `raw/` sources for deeper detail.

### 3. Synthesize an Answer

Compose an answer that:
- **Cites wiki pages** — reference specific pages by name (e.g. "According to [[video-vae]], the compression ratio is 1:192")
- **Synthesizes across pages** — don't just summarize one page; connect information from multiple pages
- **Notes confidence** — flag where information might be incomplete or contradictory
- **Is concise** — answer the question directly, then provide supporting detail

### 4. Optionally File the Answer

If the answer represents valuable synthesis — a comparison, analysis, connection, or insight that doesn't exist in the wiki yet — ask the user:

> "This answer synthesizes information from multiple pages. Want me to save it as a new wiki page?"

If yes:
- Create a new wiki page (type: `analysis` or `comparison`)
- Add it to `wiki/index.md`
- Append to `wiki/log.md`: `## [YYYY-MM-DD] query | Question summary`

## Output Formats

Adapt the answer format to the question:

- **Factual question** → direct answer with citations
- **Comparison** → markdown table with citations
- **"What do we know about X"** → structured summary with sections
- **"What's missing"** → gap analysis with suggestions for new sources
- **Technical "how to"** → step-by-step with code examples from wiki pages

## Tips

- The wiki is the first source of truth — it contains compiled, cross-referenced knowledge
- If the wiki can't answer the question, say so and suggest what sources to look for
- Good queries compound: file valuable answers back into the wiki
- For broad questions, read more pages; for specific questions, grep for keywords
