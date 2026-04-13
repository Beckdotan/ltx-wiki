---
title: "JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-just-dub-it-arxiv-2601.22143.md
  - raw/paper-just-dub-it.md
tags:
  - paper
  - just-dub-it
  - dubbing
  - lora
  - audio-visual
  - lightricks
  - tel-aviv-university
---

# JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion

**Authors:** Anthony Chen, Naomi Ken Korem, Tavi Halperin, Matan Ben Yosef, Urska Jelercic, Ofir Bibi (Lightricks); Or Patashnik, Daniel Cohen-Or (Tel Aviv University)
**Affiliations:** [[lightricks-research-overview|Lightricks]], Tel Aviv University
**Date:** January 2026
**arXiv:** [2601.22143](https://arxiv.org/abs/2601.22143)
**Project Page:** [justdubit.github.io](https://justdubit.github.io/)
**Code:** [GitHub](https://github.com/justdubit/just-dub-it)

## Summary

JUST-DUB-IT introduces a single-model approach that adapts [[paper-ltx-2|LTX-2]] for video-to-video dubbing via a lightweight LoRA. The approach enables the model to condition on an input audio-video while jointly generating translated audio and synchronized facial motion, replacing complex multi-stage dubbing pipelines with a single adapted model.

## Key Contributions

1. **Single-model dubbing pipeline** -- replaces complex multi-stage dubbing pipelines with a single model adapted via lightweight LoRA fine-tuning
2. **Joint audio-visual generation** -- simultaneously generates translated audio and synchronized facial motion, maintaining consistency between modalities
3. **Self-supervised training data** -- leverages the generative model itself to synthesize paired multilingual videos of the same speaker
4. **Identity and lip sync preservation** -- preserves speaker identity and lip synchronization while being robust to complex motion and real-world dynamics

## Methodology

### Foundation

Built on top of LTX-2's audio-visual diffusion model architecture, using the [[paper-avcontrol|AVControl]] framework's parallel canvas conditioning mechanism.

### LoRA Adaptation

A lightweight LoRA (Low-Rank Adaptation) applied to the foundation model enables video-to-video dubbing without full model fine-tuning. The model conditions on input audio-video for generating translated output.

### Self-Supervised Training Data Generation

1. Use the generative model to create multilingual videos of the same speaker
2. Generate language switches within a single clip
3. Inpaint face and audio in each half of the clip
4. Use resulting pairs for LoRA training

This approach eliminates the need for manually paired multilingual dubbing datasets.

## Results

- Higher visual fidelity than existing dubbing pipelines
- Better lip synchronization
- Greater robustness to complex motion and real-world dynamics

## Relationship to LTX Ecosystem

JUST-DUB-IT builds directly on the [[paper-ltx-2|LTX-2]] foundation model and the [[paper-avcontrol|AVControl]] framework, demonstrating the versatility of the joint audio-visual architecture for downstream tasks beyond generation. The work shows that LTX-2's bidirectional audio-visual cross-attention and synchronized generation capabilities can be effectively adapted for video editing/dubbing tasks via parameter-efficient fine-tuning. It is one of three concurrent works (alongside [[paper-id-lora|ID-LoRA]] and In-Context Sync-LoRA) that adopted the AVControl framework.
