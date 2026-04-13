# LTX-2 Benchmarks, Evaluations, and Model Comparisons

## Artificial Analysis Leaderboard Rankings

### Text-to-Video Arena (Elo Scores -- as of early 2026)
Among open-weights text-to-video models with audio:

| Model | Elo Score |
|-------|-----------|
| LTX-2.3 Fast | 975 |
| LTX-2.3 Pro | 956 |
| LTX-2 Fast | 937 |

LTX-2.3 Fast leads among open-weights text-to-video models with audio.

### Video-Only Rankings (November 2025)
- **3rd in Image-to-Video** generation
- **4th in Text-to-Video** generation
- Surpassed: Sora 2 Pro, Wan 2.2-14B
- Notable: Joint audio-video training does NOT compromise visual quality

## Human Preference Studies

### Audiovisual Quality
- Participants evaluated: visual realism, audio fidelity, temporal synchronization (lip-sync, foley)
- **Significantly outperforms** open-source alternatives (e.g., Ovi)
- **Comparable to** leading proprietary models (Veo 3, Sora 2)

## Inference Speed Benchmarks

### H100 Benchmark (121 frames @ 720p, single Euler step, CFG=1)

| Model | Parameters | Modalities | Relative Speed |
|-------|-----------|------------|----------------|
| LTX-2 | 19B | Audio+Video | 1x (baseline) |
| Wan 2.2 | 14B | Video only | ~18x slower |
| Ovi | 2x5B | Audio+Video | Slower than LTX-2 |

Performance gap widens at higher resolutions and longer durations. Speed advantage comes from the efficient 1:192 latent space compression inherited from LTX Video.

### Consumer GPU Benchmarks

| GPU | Resolution | Duration | Time |
|-----|-----------|----------|------|
| RTX 4090 | 4K, 30-36 steps | 10 seconds | 9-12 minutes |
| RTX 3090 | 4K, 30-36 steps | 10 seconds | 20-25 minutes |
| RTX 3080 (GGUF Q4_K_S) | 960x544 | 5 seconds | 2-3 minutes |
| RTX 3070 Ti laptop (8GB) | Various | Various | ~300-400 seconds |

### Fast vs Pro Mode
- Fast mode: ~2x faster inference than Pro
- On consumer GPU: 30-second render (Fast) vs 60-second render (Pro) for same content

## Competitive Positioning (2026)

### Overall Landscape

| Model | Type | Key Strength |
|-------|------|-------------|
| Runway Gen-4.5 | Proprietary | Overall benchmark leader |
| Sora 2 Pro | Proprietary | Realism king, highest physics/cinematics |
| Veo 3.1 | Proprietary | Cinematic quality leader |
| LTX-2.3 Pro | Open-source | Best open/dev-friendly 4K + high-fps pipeline |
| Wan 2.5 | Open-source | Alternative open-source option |

### LTX-2 Competitive Advantages
- First open-source 4K audio-video model
- 18x faster than Wan 2.2
- Up to 50% lower compute cost than competitors
- Runs on consumer hardware
- Full open weights + training code

### LTX-2 Limitations vs Proprietary Models
- Proprietary models (Sora 2, Veo 3.1, Runway Gen-4.5) maintain advantages in:
  - Overall cinematic quality
  - Physics simulation realism
  - Coherent long-form cinematics

## Temporal Scope Comparison

| Model | Max Duration | Type |
|-------|-------------|------|
| LTX-2 | 20 seconds | Open-source |
| Sora 2 | 16 seconds | Proprietary |
| Veo 3 | 12 seconds | Proprietary |
| Ovi | 10 seconds | Open-source |
| Wan 2.5 | 10 seconds | Open-source |

LTX-2 leads in maximum generation duration among both open and proprietary models.

## VBench Scores

### Direct VBench Scores
The LTX-2 paper focuses on human preference studies rather than reporting extensive VBench subscores. The T2AV (text-to-audio-video) benchmark page is available at:
- https://appvikalabs.github.io/ltx2bench/

### AVControl VBench Results (Related)
From the AVControl paper (built on LTX-2):
- LoRA rank 128: VBench 81.6
- Depth-guided: 81.1 @ 1K steps, 81.6 @ 3K steps

## NVIDIA Optimization Impact

| Quantization | Speed Improvement | VRAM Reduction |
|-------------|-------------------|----------------|
| NVFP8 (RTX 30/40/50) | 2x faster | 40% less |
| NVFP4 (RTX 50 only) | 3x faster | 60% less |

## Community-Reported Performance

### EasyCache Optimization
- Community contribution enabling 2.3x inference speedup
- Works with existing LTX-2 weights

### Multi-GPU Inference
- LTX-2 supports multi-GPU inference for production workloads
- Reduces per-clip generation time proportionally

## Sources

- [Artificial Analysis Text-to-Video Leaderboard](https://artificialanalysis.ai/video/leaderboard/text-to-video)
- [Artificial Analysis Video Model Comparisons](https://artificialanalysis.ai/video/models)
- [LTX-2 ArXiv Paper (2601.03233)](https://arxiv.org/abs/2601.03233)
- [MindStudio -- LTX-2 19b vs Ray Flash 2](https://www.mindstudio.ai/blog/ltx-2-19b-vs-ray-flash-2-fastest-video-models)
- [RizzGen -- AI Video Model Comparison 2026](https://rizzgen.ai/blogs/runway-kling-veo-sora-ltx-wan-seedance-comparison)
- [Pixazo -- Model Comparison](https://www.pixazo.ai/blog/veo-3-1-vs-sora-2-pro-vs-kling-2-6-vs-wan-2-5-vs-hailuo-2-3-vs-ltx-2-pro-vs-seedance-pro)
- [LTX-2.3 vs WAN 2.2 -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-vs-wan-2-2-comparison-2026/)
- [NVIDIA RTX Accelerates LTX-2](https://blogs.nvidia.com/blog/rtx-ai-garage-ces-2026-open-models-video-generation/)
- [LTX-2 T2AV Benchmark](https://appvikalabs.github.io/ltx2bench/)
