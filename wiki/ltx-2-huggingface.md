---
title: "LTX-2 HuggingFace Model Card"
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-2
  - https://github.com/Lightricks/LTX-2
  - https://arxiv.org/abs/2601.03233
tags:
  - huggingface
  - model-card
  - ltx-2
  - open-source
  - diffusion-transformer
  - audio-video
  - lightricks
---

# LTX-2 HuggingFace Model Card

LTX-2 is a DiT-based audio-video foundation model developed by [[lightricks]] that generates synchronized video and audio within a single model. It has 19 billion parameters and is available under the LTX-2 Community License Agreement.

For the original video-only model, see [[ltx-video-huggingface]]. For the latest version, see [[ltx-2-3-huggingface]].

## Key Facts

| Property | Value |
|----------|-------|
| Developer | Lightricks |
| Architecture | Diffusion Transformer (DiT) |
| Parameters | 19B |
| Language | English prompts |
| License | LTX-2 Community License Agreement |
| Paper | [LTX-2: Efficient Joint Audio-Visual Foundation Model](https://arxiv.org/abs/2601.03233) |
| Monthly downloads | 907,185+ |
| Community discussions | 52 |
| GitHub | https://github.com/Lightricks/LTX-2 |

## Model Checkpoints

| Checkpoint | Description |
|-----------|-------------|
| `ltx-2-19b-dev` | Full model, flexible and trainable in bf16 |
| `ltx-2-19b-dev-fp8` | Full model in fp8 quantization |
| `ltx-2-19b-dev-fp4` | Full model in nvfp4 quantization |
| `ltx-2-19b-distilled` | Distilled version, 8 steps, CFG=1 |
| `ltx-2-19b-distilled-lora-384` | LoRA version applicable to full model |
| `ltx-2-spatial-upscaler-x2-1.0` | 2x spatial upscaler for latents |
| `ltx-2-temporal-upscaler-x2-1.0` | 2x temporal upscaler for higher FPS |

## Supported Pipelines

- Text-to-video
- Image-to-video
- Video-to-video
- Audio-to-video
- Text-to-audio
- Combinations thereof

## Technical Requirements

| Property | Value |
|----------|-------|
| Python | >= 3.12 |
| CUDA | > 12.7 |
| PyTorch | ~2.7 |
| Resolution | Width/height divisible by 32 |
| Frames | Divisible by 8+1 (e.g., 121 frames) |

## Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync
source .venv/bin/activate
```

For attention optimization: `uv sync --extra xformers` or Flash Attention 3 for Hopper GPUs.

## Usage

The recommended approach is a two-stage pipeline via Hugging Face Diffusers:

1. **Stage 1**: Generate base video and audio latents at lower resolution using `LTX2Pipeline`.
2. **Stage 2**: Upscale video latents with `LTX2LatentUpsamplePipeline`.
3. **Stage 3**: Refine with distilled LoRA at higher resolution, then decode.

The final output includes both video and synchronized audio, exported via `encode_video()`.

## Training and Fine-tuning

The base `dev` model is fully trainable. LoRA training for motion, style, or likeness (sound+appearance) can complete in under 1 hour using the [LTX-2 Trainer](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md).

## Online Demos

- **Text-to-Video**: https://app.ltx.studio/ltx-2-playground/t2v
- **Image-to-Video**: https://app.ltx.studio/ltx-2-playground/i2v

## API Access

LTX-2 powers the `ltx-2-fast` and `ltx-2-pro` model tiers on the [[ltx-video-api]]. See [[ltx-video-api-models]] for details.

## Citation

```bibtex
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```

## Limitations

- Not designed for factual information generation.
- May amplify existing societal biases.
- May not perfectly match prompts.
- May generate inappropriate or offensive content.
- Audio-only generation may produce lower quality audio.

## Related Pages

- [[ltx-video-huggingface]]
- [[ltx-2-3-huggingface]]
- [[ltx-video-api]]
- [[ltx-video-api-models]]
- [[ltx-studio]]
