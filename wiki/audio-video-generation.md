---
title: Audio-Video Generation
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/use-cases-audio-video-generation.md
  - raw/comfyui-ltx-audio-video-generation.md
tags:
  - audio
  - video
  - ltx2
  - multimodal
  - lip-sync
  - use-cases
  - comfyui
---

# Audio-Video Generation

[[ltx-2-overview|LTX-2]] (released January 2026) was the first DiT-based model capable of generating synchronized video and audio jointly. [[ltx-2-overview|LTX-2.3]] (March 2026) improved upon this with better audio quality and prompt adherence.

## Supported Modalities

| Mode | Description |
|------|-------------|
| Text-to-Video | Generate video from text prompt |
| Text-to-Audio | Generate audio from text description |
| Text-to-Audio-Video | Generate synchronized video and audio from text |
| Image-to-Video | Generate video from initial image |
| Image-to-Audio-Video | Generate synchronized video and audio from image |
| Video-to-Audio | Generate audio track for existing video |
| Audio-to-Video | Generate video synchronized to audio input |
| Video-to-Video | Transform video using text guidance |
| Audio-to-Audio | Transform audio using text guidance |

## Technical Architecture

LTX-2 uses an asymmetric dual-stream transformer:

- **Video stream:** 14B parameters (LTX-2), larger in LTX-2.3
- **Audio stream:** 5B parameters
- Connected via bidirectional audio-video cross-attention layers
- Temporal positional embeddings for synchronization
- Cross-modality AdaLN for shared timestep conditioning

### Key Innovations

- **Modality-Aware CFG:** Independent control over text and cross-modal guidance
- **Thinking Tokens:** Additional tokens for enhanced prompt understanding
- **Multi-Layer Feature Extraction:** Aggregates information across all decoder layers of Gemma 3 text encoder
- **Causal Audio VAE:** Compact neural audio representation optimized for diffusion

## Performance

- **Speed:** ~18x faster than Wan 2.2 on H100 GPU
- **Temporal scope:** Up to 20 seconds of continuous video with stereo audio
- **Exceeds duration limits of:** Veo 3 (12s), Sora 2 (16s), Ovi (10s), Wan 2.5 (10s)

## Community Applications

### Lip-Sync and Portrait Animation

- linoyts created "LTX 2.3 Sync" Space (120 likes) for portrait animation with lip-sync
- multimodalart created "Ltx2 Audio To Video" (49 likes) for generating lip-synced videos from images

### Audio-Reactive Content

- Content creators use audio-to-video for music visualization
- Podcast and video producers generate talking head videos

### First-Last Frame Control

- linoyts created "LTX 2.3 First-Last Frame" (96 likes)
- Enables controlled video generation with specified start and end frames
- Combined with audio generation for complete audiovisual clips

## Prompting for Audio-Video

When using LTX-2/2.3 for audio-video generation, prompts should describe:

- Sound effects and ambient audio
- Speech content (for lip-sync)
- Music or background audio
- Language and accent specifications (for speech)

See [[prompt-engineering]] for full prompting guidance.

## ComfyUI Audio-Video Workflow

LTX-2.3 audio-video generation in [[comfyui-ltx-integration-overview|ComfyUI]] requires specific nodes and model files. For detailed node documentation see [[comfyui-ltx-node-reference]].

### Required Model Files

| File | Location | Purpose |
|------|----------|---------|
| `ltx-2.3-22b-dev.safetensors` | `models/checkpoints/` | Full model with audio |
| `LTX2_audio_vae_bf16.safetensors` | (auto-downloaded) | Audio decoder |
| `LTX2_video_vae_bf16.safetensors` | (auto-downloaded) | Video decoder |
| Gemma 3 12B IT | `models/text_encoders/` | Text encoder |

### Basic Audio-Video Pipeline

1. **Load LTX-2.3 Checkpoint** -- 22B dev or distilled model
2. **Encode Text** -- Use Gemma 3 12B; describe both visual and audio content in the prompt
3. **Configure [[comfyui-ltx-node-reference|MultimodalGuider]]** -- Set CFG, STG, and cross-modal guidance
4. **Sample with [[comfyui-ltx-node-reference|LTXVNormalizingSampler]]** -- Prevents audio clipping
5. **Decode Video** -- Video VAE decode
6. **Decode Audio** -- Audio VAE decode
7. **Combine** -- Merge audio and video streams

### Custom Audio with Image-to-Video

LTX-2.3 supports combining a reference image with custom audio generation:

1. Load reference image
2. Describe desired audio in the prompt
3. Use [[comfyui-ltx-node-reference|MultimodalGuider]] to control audio-video sync strength
4. Generate video that animates the image with synchronized audio

### Audio Quality Tips

1. **Use [[comfyui-ltx-node-reference|LTXVNormalizingSampler]]** -- Prevents clipping artifacts
2. **Moderate cross-modal guidance** -- Too high causes artifacts; too low causes desync
3. **Be specific in audio prompts** -- Vague audio descriptions lead to generic results
4. **Use distilled model with 8 steps** for iteration, dev model for final audio quality
5. **Confirm audio VAE is loaded** -- Ensure `LTX2_audio_vae_bf16.safetensors` is present
6. **Monitor for clipping** -- If audio sounds distorted, reduce guidance values

## ID-LoRA: Identity-Preserving Audio-Video

ID-LoRA enables generation of videos where a specific person's visual appearance is preserved from a reference image and voice is preserved from a reference audio clip. Write a text prompt describing the scene and ID-LoRA generates video with the subject's likeness AND voice.

### Available ID-LoRA Models

| Model | Source | Description |
|-------|--------|-------------|
| CelebVHQ-3K | HuggingFace | Trained on celebrity video dataset |
| TalkVid-3K | HuggingFace | Trained on talking video dataset |

The **LTXVReferenceAudio** node was merged into upstream ComfyUI (PR #13111, March 2026). Original ID-LoRA weights work without conversion. Supports both one-stage and two-stage pipelines.

See [[comfyui-ltx-community-nodes]] for the ID-LoRA-LTX2.3-ComfyUI node pack.

## Limitations

- Performance varies across languages for speech synthesis
- Multi-speaker scenarios may confuse character-speech assignment
- Sequences longer than ~20 seconds show temporal drift
- Lower audio quality when generating non-speech audio
- No explicit reasoning or world-modeling capabilities
- Audio generation is available only in LTX-2.3, not older LTX-2 or LTX Video versions
- Higher VRAM requirements when generating both modalities
- [[comfyui-ltx-node-reference|LTXVNormalizingSampler]] is incompatible with inpainting or masked workflows

## See Also

- [[use-cases-overview]] -- Full use case landscape
- [[prompt-engineering]] -- Writing effective prompts for audio-video
- [[ltx-2-overview]] -- LTX-2 model overview
- [[comfyui-ltx-node-reference]] -- Node reference (MultimodalGuider, LTXVNormalizingSampler)
- [[comfyui-ltx-workflows]] -- ComfyUI workflow types
- [[ltx-awesome-resources]] -- Community resource directory
