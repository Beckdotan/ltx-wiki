---
title: "AVControl: Efficient Framework for Training Audio-Visual Controls"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-avcontrol-arxiv-2603.24793.md
tags:
  - paper
  - avcontrol
  - lora
  - control
  - audio-visual
  - lightricks
---

# AVControl: Efficient Framework for Training Audio-Visual Controls

**Authors:** Tavi Halperin*, Naomi Ken Korem*, Mohammad Salama, Harel Cain, Asaf Joseph, Anthony Chen, Urska Jelercic, Ofir Bibi (* equal contribution)
**Affiliation:** [[lightricks-research-overview|Lightricks]]
**Date:** March 27, 2026
**arXiv:** [2603.24793](https://arxiv.org/abs/2603.24793)
**Project Page:** [matanby.github.io/AVControl](https://matanby.github.io/AVControl/)
**Code:** [GitHub (ltx-trainer)](https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer)

## Summary

AVControl is a lightweight, extendable framework built on [[paper-ltx-2|LTX-2]] where each control modality is trained as a separate LoRA on a parallel canvas that provides the reference signal as additional tokens in the attention layers. The framework requires no architectural changes beyond the LoRA adapters themselves. It supports 13 independently trained control modalities -- from depth and pose to camera trajectories and audio-visual controls -- and outperforms all evaluated baselines on the VACE Benchmark for depth-guided, pose-guided generation, inpainting, and outpainting.

## Key Contributions

1. **Parallel canvas conditioning** -- a compute- and data-efficient framework for training per-modality control LoRAs, enabling faithful video structural control and fine-grained inference-time strength modulation
2. **13 diverse control modalities** -- independently trained from spatially-aligned controls and camera trajectory to audio-visual applications
3. **Small-to-large control grid** -- a training strategy that reduces reference canvas resolution for spatially sparse modalities, lowering latency without sacrificing control fidelity

## Methodology

### Parallel Canvas

The core innovation is providing the reference signal as **additional tokens in self-attention layers** (not concatenated spatially like IC-LoRA). LTX-2's per-token timestep mechanism naturally distinguishes reference (clean) tokens from generation (noisy) tokens. No positional encoding changes are required. The only trainable component is a minimal LoRA adapter on the frozen audio-visual backbone. Reference influence can be continuously modulated at inference time (globally or locally).

### Why Not IC-LoRA (Spatial Concatenation)?

IC-LoRA concatenates reference and target side-by-side, creating large spatial distance between semantically corresponding positions that weakens attention interaction. This approach fails for structural controls (depth, pose) in video. The parallel canvas resolves this by placing reference tokens directly in the attention computation.

### Per-Modality LoRA Training

- Each control modality trained as a separate lightweight LoRA
- LoRA rank 128 (default; ranks 32-128 differ by less than 1 VBench point)
- Training converges in ~1,000-3,000 steps for spatial controls
- Total budget across all 13 modalities: ~55K steps (vs VACE's 200K steps for a single training run)

## Supported Control Modalities (13 total)

**Spatial Controls:**
- Depth-guided generation
- Pose-guided generation
- Canny edge control

**Camera Control:**
- Camera trajectory from image
- Camera trajectory from video (re-rendering)

**Motion Control:**
- Sparse point tracking

**Video Editing:**
- Cut-on-action
- Inpainting
- Outpainting

**Audio-Visual Controls:**
- Who-is-talking (speaker-aware generation)
- Audio intensity to waveform
- Audio-visual synchronization controls

## Results

### VACE Benchmark

- **Outperforms all evaluated baselines** on depth-guided, pose-guided generation, inpainting, and outpainting
- **Competitive results** on camera control and audio-visual benchmarks

### Efficiency

- Total training budget: ~55K steps across all modalities (vs VACE's 200K single training run)
- Each modality requires only a small dataset
- Convergence in few hundred to few thousand steps
- Generalization from synthetic data demonstrated for several LoRAs

### Ablations

- Depth VBench scores: 81.1 at 1,000 steps, 81.6 at 3,000 steps
- LoRA ranks 32/64/128 yield VBench averages of 80.9/81.3/81.6

## Downstream Adoption

The AVControl framework has been adopted by concurrent work:
- [[paper-just-dub-it|JUST-DUB-IT]] -- video dubbing via joint audio-visual diffusion
- [[paper-id-lora|ID-LoRA]] -- identity-driven audio-video personalization
- **In-Context Sync-LoRA** -- portrait video editing

## Limitations

- Inherits base model ([[paper-ltx-2|LTX-2]]) capabilities and limitations
- Mask representation: rare failures when video colors match designated inpainting color
- Complex character motion: temporal jitter with rapid intricate movements
- Camera control: stretching/ghosting in scenes with rapid non-rigid motion
- Reference image conditioning (identity preservation) is a capability gap vs VACE

## Relationship to LTX Ecosystem

AVControl is built directly on [[paper-ltx-2|LTX-2]] as the frozen backbone, using LTX-2's per-token timestep mechanism as a key enabler. It is released as part of the LTX-2 model family with available checkpoints including Union Control (Depth+Pose+Canny), Detailer, and Motion Track. The framework descends from the [[paper-ltx-video|LTX-Video]] ecosystem and works with [[ltx-2.3-technical|LTX-2.3]] as well.
