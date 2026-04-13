---
description: Research a topic online and save findings as raw source files for the LTX wiki. Use when the user says "research", "find sources about", "look up", "search for information on", or wants to expand the wiki's raw source collection on a new topic. Conducts web research via multiple parallel agents and saves specific, granular markdown files to raw/.
---

# Wiki Research

Research a topic online using parallel subagents and save findings as specific, granular markdown source files in `raw/`. This feeds the wiki's ingest pipeline — research first, then ingest separately.

**Working directory:** The current wiki project root (contains `raw/`, `wiki/`, `CLAUDE.md`).

## Arguments

`$ARGUMENTS` — The topic or question to research. Can be broad ("LTX-2.3 community reception") or specific ("fal.ai pricing for LTX Video").

## Workflow

### 1. Scope the Research

Based on the topic, define 3-10 research areas. Present them to the user for approval:

```
| # | Research Area | What to Find |
|---|-------------- |-------------|
| 1 | Area name     | Specific targets |
| ...
```

Ask: "Does this breakdown look right, or would you shift any areas?"

### 2. Launch Research Agents

Launch parallel subagents (one per research area). Each agent:
- Uses **WebSearch** to find relevant pages
- Uses **WebFetch** to extract content from specific URLs
- Uses **Write** to save findings as markdown files in `raw/`

### 3. File Naming Convention

Each raw file should be specific and descriptive:

**Good:** `raw/fal-ai-ltx-video-pricing-april-2026.md`
**Bad:** `raw/fal-ai.md` (too broad — the wiki page will be the synthesis)

Each file should contain:
```markdown
# Title

**Source:** URL
**Date:** YYYY-MM-DD
**Retrieved:** YYYY-MM-DD

## Content

[Actual content, not just a link. Include key facts, quotes, data.]
```

### 4. Report Results

After all agents complete, report:
- Total files saved
- Files per research area
- Key findings or surprises
- Suggested next step: "Run `/ingest` to process these into wiki pages"

## Agent Prompt Template

Each research agent should receive:

```
You are a research agent. Conduct thorough online research about [TOPIC]
and save findings as markdown files in [PATH]/raw/.

Topics to research:
- [Specific sub-topics]

For each distinct finding, create a separate markdown file in raw/ with a
descriptive filename. Each file should have a source URL at the top and
contain actual content, not just links. Be specific.

Use WebSearch to find pages, WebFetch to extract content, Write to save files.
List all files created at the end.
```

## Tips

- **Be specific, not broad**: "LTX-2.3 NVFP4 benchmarks on RTX 5090" not "LTX performance"
- **One topic per file**: Don't create catch-all files. The wiki ingest will synthesize.
- **Check existing sources**: Read `wiki/index.md` first to avoid researching what's already covered
- **Save actual content**: The raw file should contain enough information to be useful without re-fetching the URL
- **Prefix for organization**: Use consistent prefixes (e.g. `reddit-`, `paper-`, `competitor-`, `tutorial-`)

## Scaling

| Sources needed | Agents |
|---------------|--------|
| 1-10 | 1-3 agents |
| 10-50 | 3-5 agents |
| 50-100 | 5-10 agents |
| 100+ | 10-20 agents |

For very large research sweeps, group agents by topic area and run in a single parallel batch.
