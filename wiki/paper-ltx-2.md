---
title: "LTX-2: Efficient Joint Audio-Visual Foundation Model"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-ltx-2-arxiv-2601.03233.md
  - raw/paper-ltx2-arxiv.md
  - raw/research-papers-ltx-video.md
tags:
  - paper
  - ltx-2
  - audio-visual
  - diffusion-model
  - dual-stream
  - lightricks
---

# LTX-2: Efficient Joint Audio-Visual Foundation Model

**Authors:** Yoav HaCohen, Benny Brazowski, Nisan Chiprut, Yaki Bitterman, Andrew Kvochko, Avishai Berkowitz, Daniel Shalem, Daphna Lifschitz, and 21 additional authors (29 total)
**Affiliation:** [[lightricks-research-overview|Lightricks]]
**Date:** January 6, 2026
**arXiv:** [2601.03233](https://arxiv.org/abs/2601.03233)
**License:** CC BY 4.0
**Code:** [GitHub](https://github.com/Lightricks/LTX-2) | [HuggingFace](https://huggingface.co/Lightricks/LTX-2)

## Summary

LTX-2 is the first open-source foundational model capable of generating high-quality, temporally synchronized audiovisual content in a unified manner. It addresses a significant gap in text-to-video models by incorporating quality audio generation alongside video. The model employs an asymmetric dual-stream diffusion transformer with a 14B-parameter video stream and a 5B-parameter audio stream, linked through bidirectional audio-video cross-attention. It achieves state-of-the-art audiovisual quality among open-source systems and results comparable to proprietary models at a fraction of their computational cost.

## Key Contributions

1. **Asymmetric dual-stream architecture** -- 19B total parameters with purposeful asymmetric allocation (14B video, 5B audio) reflecting different information densities between modalities
2. **Text processing blocks with thinking tokens** -- multi-token prediction using learnable tokens that replace padded positions and serve as global information carriers
3. **Compact neural audio representation** -- a causal audio VAE producing 128-dimensional tokens at ~1/25 second temporal resolution
4. **Modality-aware classifier-free guidance (modality-CFG)** -- enables independent control over text guidance and cross-modal guidance strengths
5. **Multi-scale, multi-tile inference** -- three-stage pipeline achieving native 4K output
6. **18x speedup** over comparable models while handling both modalities

## Architecture

### Dual-Stream Transformer

| Component | Specification |
|-----------|--------------|
| Total parameters | 19B |
| Video stream | 14B parameters |
| Audio stream | 5B parameters |
| Number of layers | 48 |
| Video positional encoding | 3D RoPE (x, y, temporal) |
| Audio positional encoding | 1D temporal RoPE |
| Text encoder | Gemma 3 (12B parameters, multilingual) |

Each dual-stream block performs four sequential operations:
1. **Self-attention** within the same modality
2. **Text cross-attention** for prompt conditioning
3. **Audio-visual cross-attention** for bidirectional inter-modal exchange
4. **Feed-forward network** for refinement

### Cross-Modality Conditioning

- **Bidirectional cross-attention** layers throughout model depth
- **Cross-modality AdaLN gates** -- scaling and shift parameters for one modality conditioned on hidden states of the other
- During audio-visual cross-attention, only the temporal component of RoPE is used, enforcing synchronization in time rather than spatial alignment
- Enables lip-synchronization, environmental acoustics, and foley sound alignment with sub-frame precision

### Text Conditioning (Deep Text Conditioning)

Uses Gemma 3 (12B) as the text encoder with multi-layer feature extraction across ALL decoder layers (not just the final layer):
1. Mean-centered scaling applied across sequence and embedding dimensions per layer
2. Flattened to [B, T, D x L], then projected via learnable dense projection matrix W
3. W jointly optimized with LTX-2 during initial training, then frozen
4. LLM weights kept entirely frozen throughout

**Thinking tokens:** Learnable tokens appended to the text connector input, replacing padded positions. They function as global information carriers with aggregated contextual information and compensate for the causal-only attention of the decoder-only LLM by enabling bidirectional refinement in the text connector.

### Video VAE

Built on the same spatiotemporal causal VAE from [[paper-ltx-video|LTX-Video]], maintaining the high 1:192 compression ratio.

### Audio VAE

| Component | Specification |
|-----------|--------------|
| Input | Stereo audio at 16 kHz |
| Process | Mel-spectrogram per channel, concatenated |
| Latent dimensions | 128-dimensional feature vectors |
| Temporal resolution | ~1/25 seconds per token |
| Architecture | Causal (enables streaming) |

### Vocoder

Modified HiFi-GAN V1 with doubled channel count for joint stereo synthesis and upsampling (16 kHz input to 24 kHz output).

## Inference

### Modality-Aware CFG Formula

```
M_hat(x,t,m) = M(x,t,m) + s_t * (M(x,t,m) - M(x,empty,m)) + s_m * (M(x,t,m) - M(x,t,empty))
```

- s_t controls textual guidance strength
- s_m controls cross-modal guidance strength
- Video stream defaults: s_t=3, s_m=3
- Audio stream defaults: s_t=7, s_m=3

### Multi-Scale, Multi-Tile Inference Pipeline

1. **Base generation** at ~0.5 MP establishing global composition and motion dynamics
2. **Latent upscaling** to increase spatial resolution while maintaining temporal and auditory alignment
3. **Tiled refinement** partitioning upscaled latents into overlapping tiles for 1080p detail synthesis

## Training

- **Data:** Subset of the LTX-Video dataset focusing on clips with significant audio
- **Captioning:** Custom system describing both visual and auditory tracks -- music, ambient sounds, dialogue transcriptions with speaker/language/accent identification, camera motion, lighting, subject behavior
- **Loss:** Flow-matching for velocity field prediction
- **Text connector:** Jointly optimized projection matrix W during initial stage, then frozen; text connector blocks trained simultaneously with DiT blocks

## Evaluation

### Inference Speed

| Model | Modality | Parameters | Seconds/Step |
|-------|----------|-----------|--------------|
| Wan 2.2-14B | Video only | 14B | 22.30s |
| **LTX-2** | **Audio + Video** | **19B** | **1.22s** |

### Visual Performance (Artificial Analysis, November 2025)

- **3rd** in Image-to-Video generation
- **4th** in Text-to-Video generation
- Surpasses Sora 2 Pro and Wan 2.2-14B

### Audiovisual Quality

- Significantly outperforms open-source alternatives (Ovi)
- Comparable to proprietary models (Veo 3, Sora 2) in human preference studies

### Temporal Capacity

| Model | Max Duration |
|-------|-------------|
| **LTX-2** | **20 seconds** |
| Sora 2 | 16 seconds |
| Veo 3 | 12 seconds |
| Ovi | 10 seconds |
| Wan 2.5 | 10 seconds |

## Capabilities

- Text-to-Video
- Image-to-Video
- Text-to-Audio+Video (joint generation)
- Video-to-Audio
- Audio-to-Video
- Synchronized speech, foley, music, and ambient sound generation

## Model Checkpoints

| Checkpoint | Parameters | Notes |
|-----------|-----------|-------|
| ltx-2-19b-dev | 19B | Full model, trainable in bf16 |
| ltx-2-19b-dev-fp8 | 19B | Quantized to fp8 |
| ltx-2-19b-dev-fp4 | 19B | Quantized to nvfp4 |
| ltx-2-19b-distilled | 19B | Distilled (8 steps, CFG=1) |
| ltx-2-spatial-upscaler-x2 | - | 2x spatial upscaler |
| ltx-2-temporal-upscaler-x2 | - | 2x temporal upscaler |

## Limitations

- Language performance varies; underrepresented dialects show weaker speech synthesis
- Multi-speaker scenarios may inconsistently assign dialogue to characters
- Sequences exceeding ~20 seconds risk temporal drift
- No explicit reasoning or world-modeling capabilities

## Relationship to Other Work

LTX-2 is the direct successor to [[paper-ltx-video|LTX-Video]], extending its spatiotemporal latent space and architecture principles to joint audio-visual generation. It was further refined in [[ltx-2.3-technical|LTX-2.3]]. The [[paper-avcontrol|AVControl]] framework uses LTX-2 as its frozen backbone for training modular control LoRAs. [[paper-just-dub-it|JUST-DUB-IT]] and [[paper-id-lora|ID-LoRA]] are downstream applications built on the LTX-2/AVControl ecosystem. [[paper-cafa|CAFA]] represents an earlier, separate approach to video-to-audio generation that preceded the joint approach in LTX-2.

## Citation

```bibtex
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```
