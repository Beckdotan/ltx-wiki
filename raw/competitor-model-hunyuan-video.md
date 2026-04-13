# HunyuanVideo (Tencent) -- Model Competitor to LTX Video

## Overview

HunyuanVideo is Tencent's open-source video foundation model that achieves performance comparable to or exceeding leading closed-source models. The family includes the original 13B parameter model and the more efficient HunyuanVideo-1.5 with 8.3B parameters, specifically designed to run on consumer-grade GPUs.

## Versions and Evolution

### HunyuanVideo (Original, 2024)
- 13 billion parameters
- Outperformed Runway Gen-3, Luma 1.6, and top Chinese video models in professional human evaluation
- Excels particularly in motion quality
- Full open source on GitHub and Hugging Face

### HunyuanVideo-1.5 (November 2025)
- 8.3 billion parameters (lightweight variant)
- State-of-the-art visual quality and motion coherence
- Designed for consumer-grade GPU inference
- Two-stage pipeline: DiT for initial generation + Super-Resolution Network for upscaling

### HunyuanVideo-Avatar (May 2025)
- Audio-driven human animation model
- Built on HunyuanVideo foundation

### HunyuanCustom (May 2025)
- Multimodal-driven architecture for customized video generation

### Training Code Release (December 2025)
- Full training pipeline with FSDP, context parallel, gradient checkpointing
- LoRA tuning scripts released

## Technical Specifications

| Spec | HunyuanVideo | HunyuanVideo-1.5 |
|------|-------------|-------------------|
| **Parameters** | 13B | 8.3B |
| **Architecture** | DiT | DiT with SSTA attention |
| **Resolution** | Up to 720p | 480p-720p (stage 1) + 1080p (stage 2 upscale) |
| **FPS** | 24 fps | 16-24 fps |
| **Duration** | 5-10 seconds | 2-10 seconds |
| **VRAM** | ~40 GB+ | ~14 GB (with offloading) |
| **Min VRAM (1.5)** | N/A | 13.6 GB (720p, 121 frames, with offloading) |
| **VAE** | 3D Causal VAE | 3D Causal VAE (16x spatial, 4x temporal compression) |
| **Text Encoder** | Bilingual (CN/EN) | Glyph-aware bilingual encoding |
| **License** | Tencent Hunyuan Community License | Open source |

## Architecture Details

### HunyuanVideo-1.5 Key Innovations
- **SSTA (Selective and Sliding Tile Attention):** Prunes redundant spatiotemporal KV blocks, achieving 1.87x end-to-end speedup over FlashAttention-3 for 10-second 720p synthesis
- **Unified DiT:** Single transformer supports both text-to-video and image-to-video generation
- **Progressive Pre-training:** Starts at 256p/16fps, advances through 480p and 720p at 24fps
- **Step Distillation:** Maintains quality while significantly reducing inference steps
- **Video Super-Resolution Network:** Upscales stage-1 output to 1080p

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Audio-driven avatar animation (Avatar variant)
- Custom video generation with multimodal inputs (HunyuanCustom)
- LoRA fine-tuning
- Full training pipeline (FSDP, distributed training)
- Multi-person scene handling (standout capability)

## Strengths

1. **Exceptional multi-person scene quality** -- cinematic clarity in complex compositions
2. **Consumer GPU accessible** -- HunyuanVideo-1.5 runs on a single RTX 4090 with 14 GB VRAM
3. **75% faster generation** on RTX 4090 with SSTA attention (videos within 75 seconds)
4. **Professional-grade motion coherence** rivaling closed-source models
5. **Complete training pipeline** released for community fine-tuning
6. **Bilingual text understanding** (Chinese and English) with glyph-aware encoding
7. **Two-stage pipeline** allows 1080p output from consumer hardware
8. **Strong benchmark performance** outperforming multiple commercial models

## Weaknesses

1. **No native audio generation** -- video only
2. **Complex setup** -- two-stage pipeline adds workflow complexity
3. **Lower resolution at stage 1** -- native generation is 480p-720p; 1080p requires upscaling stage
4. **Larger model still needs professional GPUs** -- original 13B model requires 40 GB+ VRAM
5. **License ambiguity** -- Tencent Hunyuan Community License for original model is more restrictive than Apache 2.0
6. **Limited video duration** -- 2-10 seconds per generation
7. **Slower than LTX** for equivalent quality output

## Comparison to LTX Video

| Dimension | HunyuanVideo-1.5 | LTX-2.3 |
|-----------|-------------------|---------|
| **Parameters** | 8.3B | 2B / 22B |
| **Max Resolution** | 1080p (upscaled) | Native 4K |
| **Native Audio** | No | Yes |
| **Speed (RTX 4090)** | ~75 seconds for a clip | Faster than real-time for 720p |
| **VRAM (min)** | 13.6 GB | ~12 GB (8 GB quantized) |
| **Multi-person scenes** | Best-in-class | Good |
| **Training code** | Yes | Yes |
| **License** | Tencent Community License | Apache 2.0 (revenue cap) |
| **Ecosystem** | ComfyUI, standalone | ComfyUI, Desktop, Studio, MCP |

**Key Takeaway:** HunyuanVideo excels at multi-person scenes and cinematic quality within consumer GPU constraints. LTX-2.3 surpasses it with native 4K, synchronized audio, faster generation, and a broader product ecosystem. HunyuanVideo is a strong choice for teams prioritizing multi-character narrative quality on limited hardware.

## Sources

- [HunyuanVideo GitHub](https://github.com/Tencent-Hunyuan/HunyuanVideo)
- [HunyuanVideo-1.5 GitHub](https://github.com/Tencent-Hunyuan/HunyuanVideo-1.5)
- [HunyuanVideo-1.5 on Hugging Face](https://huggingface.co/tencent/HunyuanVideo-1.5)
- [HunyuanVideo 1.5 Technical Report (arXiv)](https://arxiv.org/html/2511.18870v2)
- [Tencent Open Sources HunyuanVideo (ComfyUI Wiki)](https://comfyui-wiki.com/en/news/2024-12-01-tencent-hunyuan-video)
- [HunyuanVideo-I2V on Hugging Face](https://huggingface.co/tencent/HunyuanVideo-I2V)
- [HunyuanVideo 1.5 Low VRAM Guide](https://apatero.com/blog/hunyuanvideo-15-low-vram-gguf-5g-complete-guide-2025)
- [HunyuanVideo 1.5 Complete Guide](https://apatero.com/blog/hunyuanvideo-15-complete-guide-consumer-gpu-2025)
- [Open-Source I2V Showdown: Hunyuan vs Wan vs LTXV](https://medium.com/@lada.huang2017/open-source-image-to-video-model-showdown-2025-hunyuan-vs-wan-2-1-vs-ltxv-3d14dd3565a5)
