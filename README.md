# LTX Wiki

A comprehensive knowledge base about [LTX Video](https://github.com/Lightricks/LTX-Video) by Lightricks — covering models, architecture, API, competitors, community projects, and more.

Built using the [Karpathy LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): an LLM maintains a structured, interlinked wiki from raw sources, handling all the summarization, cross-referencing, and bookkeeping that makes a knowledge base actually useful over time.

## What's inside

| | Count | Description |
|---|---|---|
| **Raw sources** | 239 files | Research findings from web, GitHub, HuggingFace, Reddit, arxiv, blogs |
| **Wiki pages** | 213 pages | Structured, cross-linked markdown with `[[wikilinks]]` |
| **Knowledge graph** | 772 nodes, 1038 edges | Interactive HTML visualization via [graphify](https://github.com/safishamsi/graphify) |

### Topics covered

- LTX-Video model versions (0.9.0 through 0.9.8) and LTX-2/2.3
- Architecture deep dives (Video-VAE, DiT transformer, 1:192 compression)
- 6 Lightricks research papers (LTX-Video, LTX-2, AVControl, CAFA, Just-Dub-It, ID-LoRA)
- Official REST API, pricing, and developer docs
- Inference providers (fal.ai, Replicate, Segmind, WaveSpeed, Modal, RunPod)
- Python/Diffusers integration and ComfyUI workflows
- LoRA training, fine-tuning, and IC-LoRA control
- Community projects, adoption metrics, and social media sentiment
- Model competitors (Wan, HunyuanVideo, CogVideo, Mochi, Open-Sora)
- Product competitors (Runway, Pika, Kling, Sora, Luma, Veo)
- Lightricks company, LTX Studio, LTX Desktop, ecosystem

## Quick start

### Browse in Obsidian (recommended)

1. Clone this repo
2. Open the `wiki/` folder as an [Obsidian](https://obsidian.md/) vault
3. Use graph view to explore connections between pages
4. Start from `wiki/index.md` for the full catalog

### Browse the knowledge graph

Open `graphify-out/graph.html` in any browser — no server needed. Click nodes, search concepts, filter by community.

### Use with Claude Code

The wiki comes with 4 slash commands that work automatically when you open this repo in [Claude Code](https://docs.anthropic.com/en/docs/claude-code):

```bash
# Ask questions about LTX — answers cite wiki pages
/wiki-query What are LTX-2.3's advantages over competitors?

# Research new topics and save sources
/wiki-research LTX Video 3.0 announcements

# Process new sources into wiki pages
/wiki-ingest

# Health-check the wiki for consistency
/wiki-lint
```

#### Setup

```bash
git clone https://github.com/Beckdotan/ltx-wiki.git
cd ltx-wiki

# Install graphify for knowledge graph features (optional)
pip install graphifyy
# or
pipx install graphifyy

# Open with Claude Code
claude
```

The wiki skills in `.claude/skills/` are detected automatically — no extra configuration needed.

#### Rebuild the knowledge graph

```bash
# Inside Claude Code:
/graphify ./raw
```

This processes all 239 raw sources into an interactive graph with community detection, god nodes, and surprising cross-document connections.

## How it works

This wiki follows the [Karpathy LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f):

```
Human adds sources → LLM ingests → Wiki pages grow → Human queries → Answers compound
```

Three layers:

1. **`raw/`** — Immutable source documents. The LLM reads but never modifies these.
2. **`wiki/`** — LLM-generated pages with `[[wikilinks]]`, YAML frontmatter, and cross-references. The LLM owns this layer entirely.
3. **`CLAUDE.md`** — Schema defining conventions, workflows, and structure rules.

The wiki is a **persistent, compounding artifact**. Cross-references are already there. Contradictions have been flagged. The synthesis reflects everything that's been read. Every new source and every good question makes it richer.

## Contributing

### Add a new source

1. Save a markdown file to `raw/` (articles, papers, notes — anything text)
2. Run `/wiki-ingest` in Claude Code to process it into wiki pages
3. The LLM will create/update wiki pages, update the index, and log the operation

### Improve existing pages

Edit wiki pages directly, or ask Claude Code to update them. Run `/wiki-lint` periodically to catch issues.

## License

The wiki content is generated from publicly available sources. See individual raw files for source URLs and attribution.

## Credits

- Wiki pattern by [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- Knowledge graph by [graphify](https://github.com/safishamsi/graphify)
- Built with [Claude Code](https://docs.anthropic.com/en/docs/claude-code) by Anthropic
