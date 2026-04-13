# LTX-2.3: Latest Audio-Visual Foundation Model

## Metadata
- **Model Name:** LTX-2.3
- **Developer:** Lightricks
- **Parameters:** 22 billion (22B)
- **Type:** DiT-based audio-video foundation model
- **Date:** March 2026 (latest updates)
- **Paper:** Same as LTX-2 (arXiv:2601.03233), with incremental improvements
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2.3
- **Monthly Downloads:** 1.66M (as of April 2026)

## Key Improvements over LTX-2

- **Enhanced audio quality** in generated videos
- **Improved visual quality**
- **Better prompt adherence**
- **Larger model:** 22B parameters (vs 19B for LTX-2)
- Support for **spatial and temporal upscaling** with updated upscalers

## Model Checkpoints

| Checkpoint | Purpose |
|-----------|---------|
| ltx-2.3-22b-dev | Full model, flexible, trainable in bf16 |
| ltx-2.3-22b-distilled | Distilled version (8 steps, CFG=1) |
| ltx-2.3-22b-distilled-lora-384 | LoRA for distilled/full models |
| ltx-2.3-spatial-upscaler-x2-1.1 | 2x spatial upscaling |
| ltx-2.3-spatial-upscaler-x1.5-1.0 | 1.5x spatial upscaling |
| ltx-2.3-temporal-upscaler-x2-1.0 | 2x temporal upscaling (higher FPS) |

## IC-LoRA Control Models (LTX-2.3)

| Model | Type | Controls |
|-------|------|----------|
| LTX-2.3-22b-IC-LoRA-Union-Control | Any-to-Any | Canny + Depth + Pose |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | Any-to-Any | Motion tracking |

## Capabilities

- Text-to-Video
- Image-to-Video
- Audio-to-Video
- Video-to-Video
- Text-to-Audio and Audio-to-Audio
- Combined audio-visual generation
- Multi-condition generation (multiple images/video segments)

## Technical Constraints

- Width and height must be divisible by 32
- Frame count must be divisible by 8 + 1
- Padding with -1 for non-conforming dimensions
- Works best under 720x1280 resolution for base generation
- Multi-scale inference for higher resolutions

## Requirements

- Python >= 3.12
- CUDA > 12.7
- PyTorch ~= 2.7
- Supports bfloat16 and quantized formats (fp8, fp4)

## Training

- Base model fully trainable
- LoRA and IC-LoRA training supported
- Training times: <1 hour for motion, style, or likeness tasks
- Uses LTX-2 trainer: https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md

## Online Demos

- LTX API Playground: https://console.ltx.video/playground/
- Documentation: https://docs.ltx.video

## Relationship to LTX-2

LTX-2.3 represents an incremental improvement over LTX-2 with a larger model size (22B vs 19B), improved audio quality, and better overall generation quality. It maintains the same architectural principles (asymmetric dual-stream DiT, bidirectional cross-attention, modality-aware CFG) while benefiting from additional training and scaling.
