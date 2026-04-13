# LTX-2 Capabilities and Specifications

## Core Specifications

### Resolution
- **Native 4K** (3840x2160) output
- Supports multiple resolutions: 720p, 1080p, 4K
- Default generation resolution: 1216x704 at 30 FPS (base mode)
- LTX-2.3 adds native portrait support: up to 1080x1920 (9:16 aspect ratio)
- Portrait mode trained on actual vertical data, not cropped from landscape

### Frame Rate
- Up to **50 FPS** maximum
- LTX-2.3 practical smooth motion at 48 FPS
- Standard generation at 24-30 FPS
- 48 FPS useful for product spins and macro detail

### Duration
- Up to **20 seconds** of continuous video with synchronized stereo audio
- LTX-2.3 Fast: up to 20-second clips
- LTX-2.3 Pro: up to 10-second clips (prioritizes fidelity)

### Audio
- Synchronized stereo audio generation in a single pass
- Generates: dialogue, background noise, foley elements, ambient sounds, music
- Expressive lip sync and audio fidelity
- LTX-2.3 features filtered training data and new vocoder for cleaner audio

## Generation Modes

### Text-to-Video
- Generate video from text descriptions
- Supports detailed, chronological prompts (~200 words recommended)
- Multilingual prompt support via Gemma 3 text encoder

### Image-to-Video
- Animate static images with realistic motion
- Preserves source image characteristics
- Useful for logo bumps, mockup animation

### Audio-to-Video
- Generate video driven by an audio track
- Supply dialogue, music, or ambient sound
- API produces visuals synchronized to the audio

### Video Extension (Extend)
- Lengthen existing videos from beginning or end
- Preserves audio and visual continuity
- Specify duration and context window

### Video Region Regeneration (Retake)
- Selectively edit specific video segments
- Fix jittery motion or odd camera moves without starting over
- Constraints from original preserved

### Multi-Keyframe Conditioning
- Keyframe-based animation control
- Frame-level precision for complex scenes
- 3D camera logic support

### Video Interpolation
- Generate frames between keyframes
- Smooth transitions between specified states

### Video-to-Video Transformations
- Transform existing footage using IC-LoRA adapters
- Style transfer, motion transfer, edge/depth/pose guidance

## Model Variants

### LTX-2 (January 2026)
- **19B parameters** (14B video + 5B audio)
- Two performance modes:
  - **LTX-2 Fast** -- optimized for speed, rapid iteration
  - **LTX-2 Pro** -- optimized for maximum quality

### LTX-2.3 (March 2026)
- **22B parameters**
- Two performance modes:
  - **LTX-2.3 Fast** -- 2x faster inference, 1/10 compute cost, up to 20-second clips
  - **LTX-2.3 Pro** -- best quality, supports all endpoints (audio-to-video, retake, extend), up to 10-second clips

## Weight Variants on Hugging Face

### LTX-2 Weights
| Variant | Description | Size |
|---------|-------------|------|
| ltx-2-dev (bf16) | Full-precision model | ~38GB |
| ltx-2-fp8 | FP8 quantized | ~18-20GB |
| ltx-2-distilled | 8-step distilled, CFG=1 | Smaller |

### LTX-2.3 Weights
| Variant | Description | Notes |
|---------|-------------|-------|
| ltx-2.3-22b-dev.safetensors | Full BF16 model | ~42GB on disk, for fine-tuning |
| ltx-2.3-22b-distilled.safetensors | 8-step distilled | Faster inference |
| ltx-2.3-fp8 | FP8 quantized | Lower VRAM |
| Spatial upscaler (1.5x) | Latent-space spatial upscaling | |
| Spatial upscaler (2x) | Latent-space spatial upscaling | |
| Temporal upscaler | Frame rate doubling in latent space | |

## Performance Characteristics

### Speed
- **18x faster** than Wan 2.2 at comparable settings
- Fast mode: 2x inference speed vs Pro mode
- RTX 4090: 10-second 4K clip in 9-12 minutes (30-36 diffusion steps)
- RTX 3090: Same clip in 20-25 minutes
- Up to 50% lower compute cost than competing models

### VRAM Requirements
| Setup | VRAM | Notes |
|-------|------|-------|
| Full bf16 model | 32GB+ | Recommended for full quality |
| FP8 model | 16GB+ | Comfortable 1080p |
| GGUF Q4_K_S | 10GB | RTX 3080; 960x544, 5s clip in 2-3 min |
| Minimum viable | 12GB | RTX 3060, slower with RAM offloading |
| NVFP4 (RTX 50 Series) | ~13GB | 3x faster, 60% less VRAM |
| NVFP8 | ~19GB | 2x faster, 40% less VRAM |

## Sources

- [LTX-2 Product Page](https://ltx.io/model/ltx-2)
- [LTX-2.3 Product Page](https://ltx.io/model/ltx-2-3)
- [LTX-2 HuggingFace](https://huggingface.co/Lightricks/LTX-2)
- [LTX-2.3 HuggingFace](https://huggingface.co/Lightricks/LTX-2.3)
- [MindStudio -- What Is LTX-2 19b](https://www.mindstudio.ai/blog/what-is-ltx-2-19b-lightricks-video)
- [LTX-2.3 Fast vs Pro](https://ltx.io/model/model-blog/ltx-2-3-fast-vs-pro)
- [LTX-2 GGUF Models](https://huggingface.co/unsloth/LTX-2-GGUF)
- [NVIDIA RTX Accelerates LTX-2](https://blogs.nvidia.com/blog/rtx-ai-garage-ces-2026-open-models-video-generation/)
