---
title: "LTX-Video Citations and Impact"
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/papers-citing-ltx-video.md
tags:
  - citations
  - ltx-video
  - impact
  - benchmarks
  - community
---

# LTX-Video Citations and Impact

This page tracks papers citing or building on [[paper-ltx-video|LTX-Video]] and the broader LTX model family, as well as community adoption metrics.

## Internal Papers Building on LTX-Video

These papers by Lightricks directly extend the LTX-Video architecture and ecosystem:

| Paper | arXiv | Date | Relationship |
|-------|-------|------|-------------|
| [[paper-ltx-2|LTX-2]] | 2601.03233 | Jan 2026 | Direct successor; extends to joint audio-visual generation |
| [[paper-avcontrol|AVControl]] | 2603.24793 | Mar 2026 | Uses LTX-2 as frozen backbone for control LoRAs |
| [[paper-cafa|CAFA]] | 2504.06778 | Apr 2025 | V2A research preceding the joint approach in LTX-2 |
| [[paper-just-dub-it|JUST-DUB-IT]] | 2601.22143 | Jan 2026 | Video dubbing via AVControl framework on LTX-2 |
| [[paper-id-lora|ID-LoRA]] | 2603.10256 | Mar 2026 | Identity-driven personalization via AVControl framework |
| In-Context Sync-LoRA | 2512.03013 | Dec 2025 | Portrait video editing using IC-LoRA paradigm |

## External Papers Citing LTX-Video

### Efficiency and Architecture Research

- **Taming Diffusion Transformer for Efficient Mobile Video Generation** (arXiv: 2507.13343, Jul 2025) -- Snap Research notes LTX-Video's 1.9B parameters remain prohibitive for mobile; builds mobile-optimized 0.9B model achieving higher VBench total score

### World Models

- **Vid2World: Crafting Video Diffusion Models to Interactive World Models** (arXiv: 2505.14357, May 2025) -- References LTX-Video as key video diffusion model; builds interactive world models via video diffusion causalization

### Benchmarking and Evaluation

- **T2VPhysBench** (arXiv: 2505.00337, May 2025) -- Evaluates LTX-Video for physical consistency (gravity, collision, fluid dynamics)
- **T2VWorldBench** (arXiv: 2507.18107, Jul 2025) -- Evaluates LTX-Video's world knowledge, reporting average score of ~0.68
- **IP-Bench** (arXiv: 2603.26154) -- Notes nearly all protection methods yield lowest VBench degradation rates on LTX and Skyreel

### Detection and Safety

- **AI-Generated Video Detection (One-Stop Solution)** (arXiv: 2601.11035, Jan 2026) -- Includes LTX-Video among video generation models evaluated for AI-generated content detection

### Surveys

- **A Survey on Long-Video Storytelling Generation** (ICCV 2025 Workshop) -- References LTX-Video as key model in long-video storytelling landscape

### Applications

- **LLMPopcorn: Exploring LLMs as Assistants for Popular Micro-video Generation** (arXiv: 2502.12945, Feb 2025) -- Highlights LTX-Video alongside HunyuanVideo for video generation in micro-video pipeline

### Models Compared Against LTX-2

- **WAN 2.1/2.2** -- Compared in speed benchmarks; LTX-2 is ~18x faster on H100
- **Ovi** -- Concurrent open-source T2AV model using two 5B streams fine-tuned from Wan 2.2
- **BridgeDiT** -- Concurrent T2AV work cited for high computational overhead vs LTX-2
- **HunyuanVideo** (arXiv: 2412.03603) -- Contemporary work using 8x8x4 VAE compression (vs LTX-Video's 32x32x8)

## Citation Themes

LTX-Video citations cluster around several themes:

1. **Benchmarking** -- used as baseline in video generation benchmarks (VBench, T2VPhysBench, T2VWorldBench, IP-Bench)
2. **Efficiency research** -- referenced for high-compression VAE and real-time generation; targeted for further optimization (mobile deployment)
3. **World models** -- cited as foundation for interactive world models from video diffusion priors
4. **Detection** -- included in AI-generated content detection evaluations
5. **Surveys** -- referenced in comprehensive surveys of the video generation landscape
6. **Downstream tasks** -- foundation for dubbing, personalization, portrait editing, foley generation

## Community Adoption Metrics

As of April 2026:

- 567K+ monthly downloads (HuggingFace)
- 24 LoRA adapters published
- 25 community fine-tunes available
- 16 quantization variants created
- 100+ HuggingFace Spaces using the model
