# LTX Ecosystem - Complete Overview

## What is LTX?

LTX is Lightricks' professional AI video ecosystem that brings together a commercial platform, open-source models, desktop tools, APIs, and developer programs. The name represents Lightricks' pivot from consumer mobile apps to professional-grade AI video production.

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
| GitHub (LTX-2) | https://github.com/Lightricks/LTX-2 | Model code + training |
| GitHub (Desktop) | https://github.com/Lightricks/LTX-Desktop | Desktop app |
| GitHub (ComfyUI) | https://github.com/Lightricks/ComfyUI-LTXVideo | ComfyUI plugin |
| GitHub (Trainer) | https://github.com/Lightricks/LTX-Video-Trainer | Fine-tuning toolkit |
| HuggingFace | https://huggingface.co/Lightricks/LTX-2 | Model weights |

## Product Comparison

| Feature | LTX Studio | LTX Desktop | LTX API |
|---------|-----------|-------------|---------|
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

All open-source components are under **Apache 2.0** license:
- LTX Desktop (full application)
- LTX-2 (model code, pipelines, trainer)
- LTX-Video (original model)
- ComfyUI-LTXVideo (plugin nodes)
- LTX-Video-Trainer (fine-tuning toolkit)

Model weights have a separate license: free for companies under $10M annual revenue.

## Integration Points

1. **ComfyUI** - Custom nodes for node-based video workflows
2. **fal.ai** - Third-party API hosting of LTX models
3. **Segmind** - Alternative API access
4. **Modal** - Serverless deployment option
5. **AI Coding Agents** - AGENTS.md/CLAUDE.md support in LTX Desktop repo

## Developer Program: LTX Builders

An invite-only program for developers actively using LTX:
- Closer relationship with the model team
- Early access to features
- Direct communication channel
- Website: https://ltx.io/model/ltx-developer-program

## Sources

- [LTX Platform (ltx.io)](https://ltx.io/)
- [LTX Studio](https://ltx.studio)
- [LTX Dev](https://ltx.dev/)
- [LTX Model Page](https://ltx.io/model)
- [LTX API Documentation](https://docs.ltx.video/welcome)
- [LTX Desktop on ltx.io](https://ltx.io/ltx-desktop)
- [LTX Developer Program](https://ltx.io/model/ltx-developer-program)
- [Awesome LTX-2 (community)](https://github.com/wildminder/awesome-ltx2)
