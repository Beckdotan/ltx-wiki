# LTX-2.3 Audio-Video Generation in ComfyUI

## Sources
- [LTX-2.3: Introducing LTX's Latest AI Video Model | LTX](https://ltx.io/model/ltx-2-3)
- [LTX-2.3 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [LTX 2.3 Image-to-Video with Custom Audio in ComfyUI | Next Diffusion](https://www.nextdiffusion.ai/tutorials/ltx-2-3-image-to-video-with-custom-audio-in-comfyui)
- [ComfyUI LTX 2.3 Workflow: Audio-Synced Video Tutorial | Apatero](https://apatero.com/blog/comfyui-ltx-2-3-audio-video-workflow-tutorial-2026)
- [LTX-2 ComfyUI Nodes | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes)
- [LTX 2.3 Video+Audio (GGUF/FP8/NVF4/BF16) | Stable Diffusion Tutorials](https://www.stablediffusiontutorials.com/2026/03/ltx-2.3-video-audio-gen.html)
- [GitHub - ID-LoRA/ID-LoRA-LTX2.3-ComfyUI](https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI)
- [Try using the LTXV audio text encoder node | GitHub Issue](https://github.com/Lightricks/LTX-2/issues/106)

## Overview

LTX-2.3 is the first open-source model to generate synchronized audio and video within a single generative system. This is a major differentiator from all other open-source video models (Wan, CogVideoX, HunyuanVideo, Mochi, AnimateDiff), none of which offer integrated audio generation.

---

## Audio-Video Architecture

LTX-2.3 is a DiT-based audio-video foundation model that generates both modalities in a unified latent space. A single text prompt can simultaneously dictate:
- Visual content of the scene
- Environmental acoustics
- Speaking style and vocal characteristics
- Audio-visual synchronization

### Key Components

- **Video VAE:** `LTX2_video_vae_bf16.safetensors` — Encodes/decodes video
- **Audio VAE:** `LTX2_audio_vae_bf16.safetensors` — Encodes/decodes audio
- **Unified Transformer:** Processes both modalities jointly
- **Gemma 3 12B Text Encoder:** Handles text conditioning for both audio and video

---

## MultimodalGuider Node

The central node for controlling audio-video generation. Provides independent, per-modality control:

### Three Guidance Channels

1. **CFG Guidance** (`cfg > 1`)
   - Controls overall prompt adherence and semantic accuracy
   - Higher values = stronger prompt following
   - Applies to both audio and video

2. **Spatio-Temporal Guidance** (`stg > 0`)
   - Reduces structural artifacts in video
   - Prevents object breakup and temporal inconsistency
   - Video-specific quality control

3. **Cross-Modal Guidance** (`modality_scale > 1`)
   - Manages tightness of audio-video synchronization
   - Higher values = stronger sync between audio events and visual events
   - Critical for dialogue/lip-sync applications

### Per-Modality Parameters

Each modality (audio, video) can have independent settings:

| Parameter | Range | Effect |
|-----------|-------|--------|
| `skip_step` | 0-2 | Periodically skip diffusion steps (speed optimization) |
| `rescale` | 0-1 | Post-guidance normalization intensity |
| `perturb_attn` | Boolean | Enable/disable STG perturbation |
| `cross_attn` | Boolean | Enable/disable cross-modal attention |

---

## LTXVNormalizingSampler for Audio

The normalizing sampler is particularly important for audio generation:

**Problems it prevents:**
- Audio clipping and distortion
- Oversaturated visual outputs
- Inconsistent quality across generations

**Mechanism:** Monitors latent statistics via percentile-based analysis and maintains optimal value ranges throughout the denoising process.

**Best Practice:** Use as drop-in replacement for standard samplers when generating audio-video content.

---

## ID-LoRA: Identity-Preserving Audio-Video

### Overview

ID-LoRA enables generation of videos where a specific person's:
- **Visual appearance** is preserved from a reference image
- **Voice** is preserved from a reference audio clip

### How It Works

1. Provide a reference image of the subject
2. Provide a short reference audio clip of the subject's voice
3. Write a text prompt describing the scene
4. ID-LoRA generates video with the subject's likeness AND voice

### ComfyUI Integration

- **LTXVReferenceAudio** node — Merged into upstream ComfyUI (PR #13111, March 2026)
- Original ID-LoRA weights work as-is without conversion
- Supports both one-stage and two-stage pipelines

### Available Models

| Model | Source | Description |
|-------|--------|-------------|
| CelebVHQ-3K | HuggingFace | Trained on celebrity video dataset |
| TalkVid-3K | HuggingFace | Trained on talking video dataset |

---

## Audio Generation Workflow

### Basic Audio-Video Text-to-Video

1. **Load LTX-2.3 Checkpoint** — 22B dev or distilled model
2. **Encode Text** — Use Gemma 3 12B (describe both visual and audio content in prompt)
3. **Configure MultimodalGuider** — Set CFG, STG, and cross-modal guidance
4. **Sample with LTXVNormalizingSampler** — Prevents audio clipping
5. **Decode Video** — Video VAE decode
6. **Decode Audio** — Audio VAE decode
7. **Combine** — Merge audio and video streams

### Audio-Specific Prompting

Include audio descriptions in your text prompt:
- "A person speaking clearly in a quiet room"
- "Rain falling on a tin roof with distant thunder"
- "Upbeat jazz music playing in a cafe scene"
- "Birds chirping in a forest with wind through leaves"

The model interprets audio cues from the text prompt alongside visual descriptions.

---

## Custom Audio with Image-to-Video

LTX-2.3 supports combining a reference image with custom audio generation:

1. Load reference image
2. Describe desired audio in the prompt
3. Use MultimodalGuider to control audio-video sync strength
4. Generate video that animates the image with synchronized audio

---

## Audio Quality Tips

1. **Use LTXVNormalizingSampler** — Prevents clipping artifacts
2. **Moderate cross-modal guidance** — Too high causes artifacts; too low causes desync
3. **Be specific in audio prompts** — Vague audio descriptions lead to generic results
4. **Use distilled model with 8 steps** for iteration, dev model for final audio quality
5. **Check audio VAE is loaded** — Ensure `LTX2_audio_vae_bf16.safetensors` is present
6. **Monitor for clipping** — If audio sounds distorted, reduce guidance values

---

## Model Files for Audio-Video

| File | Location | Purpose |
|------|----------|---------|
| `ltx-2.3-22b-dev.safetensors` | `models/checkpoints/` | Full model with audio |
| `LTX2_audio_vae_bf16.safetensors` | (auto-downloaded) | Audio decoder |
| `LTX2_video_vae_bf16.safetensors` | (auto-downloaded) | Video decoder |
| Gemma 3 12B IT | `models/text_encoders/` | Text encoder |

---

## Limitations

- Audio generation is available only in LTX-2.3 (not older LTX-2 or LTX Video versions)
- Audio quality is still evolving — not yet at commercial production level
- Higher VRAM requirements when generating both modalities
- LTXVNormalizingSampler is incompatible with inpainting or masked workflows
- Optimized for Distilled model with 8-step schedule
