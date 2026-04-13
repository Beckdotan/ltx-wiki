---
title: "LTX-Video: Realtime Video Latent Diffusion"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-ltx-video-arxiv-2501.00103.md
  - raw/paper-ltx-video-arxiv.md
  - raw/research-papers-ltx-video.md
tags:
  - paper
  - ltx-video
  - video-generation
  - diffusion-model
  - vae
  - lightricks
---

# LTX-Video: Realtime Video Latent Diffusion

**Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, Daniel Shalem, Dudu Moshe, Eitan Richardson, Eran Levin, Guy Shiran, Nir Zabari, Ori Gordon, Poriya Panet, Sapir Weissbuch, Victor Kulikov, Yaki Bitterman, Zeev Melumian, Ofir Bibi
**Affiliation:** [[lightricks-research-overview|Lightricks]]
**Date:** December 30, 2024
**arXiv:** [2501.00103](https://arxiv.org/abs/2501.00103)
**Code:** [GitHub](https://github.com/Lightricks/LTX-Video) | [HuggingFace](https://huggingface.co/Lightricks/LTX-Video)

## Summary

LTX-Video is a transformer-based latent diffusion model that takes a holistic approach to video generation by deeply integrating the Video-VAE and the denoising transformer rather than treating them as independent components. The model achieves faster-than-real-time generation -- producing 5 seconds of 24 fps video at 768x512 resolution in just 2 seconds on an Nvidia H100 GPU. It supports both text-to-video and image-to-video generation, trained simultaneously. All source code and pre-trained models are publicly available.

## Key Contributions

1. **Holistic VAE-Transformer integration** -- the Video-VAE and denoising transformer share the denoising objective, optimizing their interaction within a compressed latent space
2. **Ultra-high-compression Video-VAE** -- achieves a 1:192 compression ratio (32x32x8 spatiotemporal downscaling) by relocating the patchifying operation from the transformer input to the VAE encoder input
3. **Denoising VAE decoder** -- the decoder performs dual functions: latent-to-pixel conversion and the final denoising step, producing clean output directly in pixel space without a separate upsampling module
4. **Reconstruction GAN (rGAN)** -- novel adversarial training where the discriminator sees both original and reconstructed versions concatenated and determines which is which
5. **Faster-than-real-time generation** with fewer than 2B parameters

## Architecture

### Video-VAE

| Component | Specification |
|-----------|--------------|
| Compression ratio | 1:192 |
| Spatial downscaling | 32x32 |
| Temporal downscaling | 8 |
| Latent channels | 128 (vs typical 16) |
| Pixels-to-tokens ratio | 1:8192 (vs typical 1:1024 or 1:2048) |
| Encoder type | Causal encoder with 3D causal convolutions |
| Patchifier | Relocated from transformer to VAE (1x1x1 effective) |

The decoder is conditioned on diffusion timestep with multi-layer noise injection (following StyleGAN). It is trained on noise levels in range [0, 0.2] corresponding to the final diffusion timestep. Adaptive normalization layers are conditioned on timestep. The decoder cannot be applied iteratively (different dimensionality input/output) but handles the critical final denoising step.

**VAE loss functions:** Pixel reconstruction (MSE), Video DWT Loss (3D Discrete Wavelet Transform, L1), Perceptual (LPIPS), and the novel Reconstruction GAN (rGAN). A uniform log-variance (single predicted value shared across all channels) prevents the KL loss from "sacrificing" channels.

### Video Transformer (DiT)

| Component | Specification |
|-----------|--------------|
| Base architecture | PixArt-alpha (extension of DiT) |
| Hidden dimension | 2048 |
| Number of blocks | 28 |
| Total parameters | ~1.9B |
| Attention type | Full spatiotemporal self-attention + cross-attention |
| Positional encoding | RoPE with normalized fractional coordinates (exponential frequency spacing) |
| Text encoder | T5-XXL |
| Normalization | RMSNorm, QK normalization on queries and keys |

Cross-attention for text conditioning was found to work better than MM-DiT.

### Image-to-Video Conditioning

Uses a per-token diffusion timestep mechanism: conditioning tokens (first frame) get a small timestep (e.g., t_c = 0), while generation tokens start at t = 1. The first frame is encoded by the causal VAE into a latent tensor with temporal dimension of 1, concatenated with noise latents. No additional parameters or special tokens are required.

## Training

- **Loss:** Rectified Flow (velocity prediction v = epsilon - z_0)
- **Timestep scheduling:** Log-normal distribution shifted toward higher-noise regions depending on token count
- **Multi-resolution:** Simultaneous training on multiple resolution/duration combinations with stochastic token dropping (0-20%)
- **Image training:** Included alongside video as one resolution-duration combination
- **Optimizer:** AdamW
- **Data:** Publicly available data supplemented with licensed material; aesthetic scoring via Siamese Network; motion filtering; automatic captioning

## Evaluation

### Human Survey (20 participants, 1000 prompts per task)

| Model | Text-to-Video Win Rate | Image-to-Video Win Rate |
|-------|----------------------|------------------------|
| **LTX-Video** | **85%** | **91%** |
| PyramidFlow | 51% | 35% |
| CogVideoX 2B | 38% | 47% |
| Open-Sora Plan | 20% | 20% |

### Speed

121 frames at 768x512 pixels in ~2 seconds on H100 (20 diffusion steps). Fastest video generation model of its kind at time of release.

### VBench

Total score: ~71.93 (baseline; later model versions improved significantly).

## Model Version History

| Version | Date | Parameters | Notes |
|---------|------|-----------|-------|
| 0.9 | Nov 2024 | 2B | Initial open-source release |
| 0.9.1 | Dec 2024 | 2B | Improvements |
| 0.9.5 | Mar 2025 | 2B | Quality improvements |
| 0.9.6 | Apr 2025 | 2B | + Distilled variant |
| 0.9.7 | May 2025 | 13B | Multiscale rendering (LTXV-13B) |
| 0.9.8 | Jul 2025 | 13B | 60-second video generation barrier broken |

## Limitations

- Sensitive to prompt formulation quality
- Limited to short videos (up to ~10 seconds)
- Domain-specific generalization not extensively tested

## Relationship to Other Work

LTX-Video is the foundational model in the Lightricks video generation ecosystem. It was directly succeeded by [[paper-ltx-2|LTX-2]] which extended its architecture to joint audio-visual generation, and [[ltx-2.3-technical|LTX-2.3]] which further refined the VAE and text conditioning. The [[paper-avcontrol|AVControl]] framework uses LTX-2 (built on LTX-Video) as a frozen backbone. See [[ltx-video-citations]] for papers citing and building on LTX-Video.

## Citation

```bibtex
@article{hacohen2024ltx,
  title={LTX-Video: Realtime Video Latent Diffusion},
  author={HaCohen, Yoav and Chiprut, Nisan and Brazowski, Benny and others},
  journal={arXiv preprint arXiv:2501.00103},
  year={2024}
}
```
