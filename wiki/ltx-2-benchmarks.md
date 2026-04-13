---
title: LTX-2 Benchmarks and Comparisons
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-benchmarks-and-comparisons.md
tags:
  - ltx-2
  - benchmarks
  - performance
  - comparisons
  - leaderboard
---

# LTX-2 Benchmarks and Comparisons

Performance benchmarks, leaderboard rankings, and competitive comparisons for [[ltx-2-overview|LTX-2]] and [[ltx-2.3-model|LTX-2.3]].

## Artificial Analysis Leaderboard (Early 2026)

### Text-to-Video Arena (Elo Scores)

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
- Joint audio-video training does NOT compromise visual quality

## Human Preference Studies

- Evaluated: visual realism, audio fidelity, temporal synchronization (lip-sync, foley)
- **Significantly outperforms** open-source alternatives (e.g., Ovi)
- **Comparable to** leading proprietary models (Veo 3, Sora 2)

## Inference Speed Benchmarks

### H100 Benchmark (121 frames @ 720p, single Euler step, CFG=1)

| Model | Parameters | Modalities | Relative Speed |
|-------|-----------|------------|----------------|
| LTX-2 | 19B | Audio+Video | 1x (baseline) |
| Wan 2.2 | 14B | Video only | ~18x slower |
| Ovi | 2x5B | Audio+Video | Slower than LTX-2 |

Speed advantage comes from the efficient 1:192 latent space compression inherited from [[ltx-video-to-ltx-2-evolution|LTX Video]]. Performance gap widens at higher resolutions and longer durations.

### Consumer GPU Benchmarks

| GPU | Resolution | Duration | Time |
|-----|-----------|----------|------|
| RTX 4090 | 4K, 30-36 steps | 10 seconds | 9-12 minutes |
| RTX 3090 | 4K, 30-36 steps | 10 seconds | 20-25 minutes |
| RTX 3080 (GGUF Q4_K_S) | 960x544 | 5 seconds | 2-3 minutes |
| RTX 3070 Ti laptop (8GB) | Various | Various | ~300-400 seconds |

### Fast vs Pro Mode

- Fast mode: ~2x faster inference than Pro
- Consumer GPU example: 30-second render (Fast) vs 60-second render (Pro) for same content

## Competitive Landscape (2026)

| Model | Type | Key Strength |
|-------|------|-------------|
| Runway Gen-4.5 | Proprietary | Overall benchmark leader |
| Sora 2 Pro | Proprietary | Realism, highest physics/cinematics |
| Veo 3.1 | Proprietary | Cinematic quality leader |
| LTX-2.3 Pro | Open-source | Best open/dev-friendly 4K + high-fps pipeline |
| Wan 2.5 | Open-source | Alternative open-source option |

### LTX-2 Competitive Advantages

- First open-source 4K audio-video model
- 18x faster than Wan 2.2
- Up to 50% lower compute cost than competitors
- Runs on consumer hardware (see [[ltx-2-model-variants]])
- Full open weights + training code

### LTX-2 Limitations vs Proprietary

Proprietary models (Sora 2, Veo 3.1, Runway Gen-4.5) maintain advantages in overall cinematic quality, physics simulation realism, and coherent long-form cinematics.

## Maximum Duration Comparison

| Model | Max Duration | Type |
|-------|-------------|------|
| LTX-2 | 20 seconds | Open-source |
| Sora 2 | 16 seconds | Proprietary |
| Veo 3 | 12 seconds | Proprietary |
| Ovi | 10 seconds | Open-source |
| Wan 2.5 | 10 seconds | Open-source |

LTX-2 leads in maximum generation duration among both open and proprietary models.

## VBench Scores

The LTX-2 paper focuses on human preference studies rather than extensive VBench subscores. The T2AV benchmark page is at: https://appvikalabs.github.io/ltx2bench/

AVControl VBench results (built on LTX-2): LoRA rank 128 achieves VBench 81.6; depth-guided reaches 81.6 at 3K training steps.

## Optimization Impact

| Optimization | Speed Gain | Source |
|-------------|-----------|--------|
| [[ltx-2-nvidia-optimization|NVFP8]] | 2x faster | NVIDIA RTX |
| [[ltx-2-nvidia-optimization|NVFP4]] | 3x faster | NVIDIA RTX 50 |
| EasyCache (community) | 2.3x faster | Community contribution |
| Multi-GPU inference | Linear scaling | Built-in support |

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2-capabilities]] -- Specifications that drive performance
- [[ltx-2-model-variants]] -- Variant-specific performance
- [[ltx-2-nvidia-optimization]] -- GPU optimization details
- [[ltx-2-community-reception]] -- How benchmarks shaped reception
