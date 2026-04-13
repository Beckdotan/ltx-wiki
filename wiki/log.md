---
title: Wiki Log
type: overview
created: 2026-04-13
updated: 2026-04-13
---

# Wiki Log

Chronological record of wiki operations. Append-only — never edit past entries.

Format: `## [YYYY-MM-DD] verb | Subject`

---

## [2026-04-13] create | Wiki initialized

Scaffolding created: directory structure, CLAUDE.md schema, index.md, log.md.

## [2026-04-13] ingest | Bulk research — 236 raw sources across 11 research agents

Research conducted via 11 parallel agents covering: LTX Video models, Lightricks products, GitHub repos, API/developer docs, ComfyUI workflows, Python/Diffusers, research papers, community projects, social media/Reddit, competitors (model + product), LTX-2 deep dive. Total: 236 markdown source files saved to raw/.

## [2026-04-13] ingest | Bulk wiki build — 206 wiki pages across 20 ingest agents

20 parallel ingest agents processed all 236 raw sources into 206 structured wiki pages. Each agent owned a thematic domain:

1. **LTX Video Overview** (8 pages) — overview hub, versions, capabilities, changelog, variants, HuggingFace, evolution
2. **Early Versions 0.9.0–0.9.6** (10 pages) — per-version pages, distillation, CFG, FP8, dev vs distilled
3. **LTX Video 0.9.7–0.9.8** (7 pages) — 13B model, IC-LoRA, spatial upscaler, multiscale rendering, variants
4. **LTX-2 Core** (13 pages) — overview, architecture, versions, 2.3, capabilities, variants, benchmarks, reception, API, NVIDIA
5. **LTX-2 Integration** (9 pages) — ComfyUI, Diffusers, guides (T2V, I2V, IC-LoRA, training), system requirements
6. **Architecture** (10 pages) — VAE, denoising decoder, DiT, RoPE, compression, loss functions, conditioning, rectified flow
7. **Research Papers** (9 pages) — 6 Lightricks papers, citations, research overview
8. **Official API** (10 pages) — REST API, endpoints, models, pricing, prompting, formats, errors, HF model cards
9. **Inference Providers** (9 pages) — fal.ai, Replicate, Segmind, WaveSpeed, HuggingFace, Modal, RunPod, RunComfy
10. **Python Pipelines** (13 pages) — installation, all pipeline classes, scheduler API, VAE API, memory optimization, native API
11. **Python Advanced** (17 pages) — conditioning, schedulers, batch, callbacks, speed/memory optimization, LoRA, integration, SDKs
12. **ComfyUI** (12 pages) — official nodes, node reference, community nodes, workflows, tutorials, performance, LoRA, Manager
13. **Training** (7 pages) — methodology, LoRA training, datasets, hyperparameters, trainers, third-party services, community
14. **Community Projects** (10 pages) — GGUF, Jetson Thor, PromptGen, Turbo Space, LipSync, camera LoRAs, benchmarks
15. **Adoption & Feedback** (7 pages) — metrics, HF Spaces, Civitai, Discord, feedback, feature requests, showcases
16. **Social Media** (14 pages) — Reddit, YouTube, X/Twitter, blogs, HN, sentiment, desktop/studio reception
17. **Model Competitors** (11 pages) — Wan, HunyuanVideo, CogVideo, Mochi, Open-Sora, AnimateDiff, SVD, landscape
18. **Product Competitors** (9 pages) — Runway, Pika, Kling, Sora, Luma, Veo, desktop tools, MCP integrations
19. **Products & Company** (10 pages) — Lightricks, ecosystem, Studio, Desktop, MCP, licensing, NVIDIA, API, Builders
20. **Tutorials & Use Cases** (10 pages) — prompting, hardware, installation, Apple Silicon, upscalers, use cases, audio-video

## [2026-04-13] create | index.md consolidated

Built comprehensive index.md with 206 pages organized into 16 categories: Overviews, Company & Products, LTX-Video Versions, LTX-2, Architecture, Papers, API, Providers, Python, ComfyUI, Training, Community, Adoption, Social Media, Competitors (model + product), Guides.
