---
title: Text Conditioning
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-transformer.md
  - raw/architecture-video-dit-vae.md
  - raw/ltx-video-architecture-paper-details.md
  - raw/ltx-video-architecture-technical.md
  - raw/ltx-video-transformer-dit-architecture.md
tags:
  - text-conditioning
  - cross-attention
  - t5
  - gemma
  - ltx-video
---

# Text Conditioning

Text conditioning controls what the [[video-dit-transformer]] generates based on natural language prompts. LTX-Video uses cross-attention rather than the MM-DiT approach adopted by several competitors.

## Cross-Attention vs. MM-DiT

LTX-Video tested both approaches and found cross-attention empirically superior:

- **Cross-attention** (LTX-Video's choice): Dedicated layers where video tokens attend to text tokens via separate query/key/value projections. Text and video representations remain in separate spaces.
- **MM-DiT** (used by SD3, FLUX.1, CogVideoX): Text tokens are processed in parallel with video tokens through unified attention layers, sharing the same attention computation.

A learnable projection layer is applied to the input text embeddings before cross-attention.

## Text Encoders by Version

### LTX-Video / LTXV-13B: T5-XXL

- Pre-trained, frozen encoder
- English-only language support
- Learnable projection layer on embeddings

### LTX-2: Gemma 3 12B

A significant upgrade introducing several innovations:

- **Multilingual decoder-only LLM** (frozen)
- **Multi-layer feature extractor**: Extracts features from ALL decoder layers (not just the final one), with mean-centered scaling per layer, projected via a learnable matrix W to the target dimension
- **Thinking tokens**: Learnable tokens appended to the text connector that replace padding positions and serve as global information carriers
- **Bidirectional refinement blocks**: Compensate for the causal-only limitation of the decoder-only LLM
- **Separate text connectors** for the video and audio streams

### LTX-2.3: Gemma 3 12B (Enhanced)

- Same base encoder as LTX-2
- 4x larger gated attention text connector for better prompt adherence

## Modality-Aware CFG (LTX-2)

LTX-2 extended Classifier-Free Guidance with separate text and cross-modal guidance terms:

```
M_hat(x,t,m) = M(x,t,m) + s_t * (text guidance) + s_m * (cross-modal guidance)
```

Default guidance scales:
- Video: s_t=3, s_m=3
- Audio: s_t=7, s_m=3

Increasing s_m promotes mutual information refinement between video and audio modalities.

## Inference Guidance

- **Dev models**: Use STG (Spatiotemporal Guidance) or CFG
- **Distilled models** (v0.9.6+): No STG/CFG required
- **Negative prompts**: Supported for quality improvement
