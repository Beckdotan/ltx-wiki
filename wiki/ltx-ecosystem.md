---
title: LTX Ecosystem
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://ltx.io/
  - https://ltx.studio
  - https://ltx.dev/
  - https://ltx.io/model
  - https://docs.ltx.video/welcome
  - https://ltx.io/ltx-desktop
  - https://ltx.io/model/ltx-developer-program
tags:
  - ltx
  - ecosystem
  - lightricks
  - ai-video
---

# LTX Ecosystem

LTX is [[lightricks-company]]'s professional AI video ecosystem that brings together a commercial platform, open-source models, desktop tools, APIs, and developer programs. The name represents Lightricks' pivot from consumer mobile apps to professional-grade AI video production.

## Ecosystem Map

```
Lightricks
├── Consumer Apps
│   ├── Facetune (photo/video retouching)
│   ├── Videoleap (mobile video editing)
│   ├── Photoleap (image editing + GenAI)
│   └── Popular Pays (creator marketing platform)
│
└── LTX (Professional AI Video)
    ├── LTX Studio (ltx.studio) — Commercial web platform
    ├── LTX Desktop — Open-source desktop app
    ├── LTX Models
    │   ├── LTX-Video / LTXV (original, 2B/13B)
    │   ├── LTX-2 (DiT audio-video model)
    │   └── LTX-2.3 (latest, 22B parameters)
    ├── LTX API (docs.ltx.video) — Developer video generation API
    ├── ComfyUI-LTXVideo — ComfyUI node integration
    ├── LTX-Video-Trainer — Fine-tuning toolkit
    └── LTX Builders (Developer Program)
```

## Key URLs

| Property | URL | Purpose |
|----------|-----|---------|
| ltx.io | https://ltx.io | Central hub: platform, models, API, enterprise |
| ltx.studio | https://ltx.studio | Commercial AI video creation platform |
| ltx.dev | https://ltx.dev | Developer-focused model access |
| docs.ltx.video | https://docs.ltx.video | API documentation |
| GitHub (LTX-2) | https://github.com/Lightricks/LTX-2 | Model code and training |
| GitHub (Desktop) | https://github.com/Lightricks/LTX-Desktop | Desktop app |
| GitHub (ComfyUI) | https://github.com/Lightricks/ComfyUI-LTXVideo | ComfyUI plugin |
| GitHub (Trainer) | https://github.com/Lightricks/LTX-Video-Trainer | Fine-tuning toolkit |
| HuggingFace | https://huggingface.co/Lightricks/LTX-2 | Model weights |

## Product Comparison

| Feature | [[ltx-studio]] | [[ltx-desktop]] | [[ltx-api]] |
|---------|----------------|-----------------|-------------|
| Type | Web platform | Desktop app | HTTP API |
| Cost | $0-$125/mo | Free (Apache 2.0) | Per-second billing |
| Target | Creatives, marketers | Developers, power users | Developers, products |
| Runs on | Cloud (browser) | Local GPU or API | Cloud |
| Model | Multiple (incl. Veo) | LTX-2.3 | LTX-2, LTX-2.3 |
| Open Source | No | Yes | No |
| Max Resolution | Varies by plan | 4K | 4K |
| Audio | Yes | Yes | Yes |
| Collaboration | Yes (Pro+) | No | N/A |

## Open Source Components

All open-source components use the **Apache 2.0** license:

- [[ltx-desktop]] (full application)
- LTX-2 (model code, pipelines, trainer)
- LTX-Video (original model)
- ComfyUI-LTXVideo (plugin nodes)
- LTX-Video-Trainer (fine-tuning toolkit)

Model weights have a separate license -- see [[ltx-video-licensing]] for details.

## Integration Points

See [[ltx-integration-projects]] for the full integration landscape. Key integration points include:

1. **ComfyUI** - Custom nodes for node-based video workflows
2. **fal.ai** - Third-party API hosting of LTX models
3. **Segmind** - Alternative API access
4. **Modal** - Serverless deployment option
5. **AI Coding Agents** - AGENTS.md/CLAUDE.md support in LTX Desktop repo (see [[ltx-mcp-integration]])

See [[github-official-repositories]] for repository details and [[github-community-tools]] for the broader community tool ecosystem.

## Developer Program

See [[ltx-builders-program]] for details on the invite-only LTX Builders developer program.
