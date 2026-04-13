# Use Case: Joint Audio-Video Generation

**Source:** https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3, https://arxiv.org/html/2601.03233

---

## Overview

LTX-2 (released January 2026) was the first DiT-based model capable of generating synchronized video and audio jointly. LTX-2.3 (March 2026) improved upon this with better audio quality and prompt adherence.

## Supported Audio-Video Modalities

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
- **Video stream:** 14B parameters (LTX-2) / larger in LTX-2.3
- **Audio stream:** 5B parameters
- Connected via bidirectional audio-video cross-attention layers
- Temporal positional embeddings for synchronization
- Cross-modality AdaLN for shared timestep conditioning

### Key Innovations
- **Modality-Aware CFG:** Independent control over text and cross-modal guidance
- **Thinking Tokens:** Additional tokens for enhanced prompt understanding
- **Multi-Layer Feature Extraction:** Aggregates information across all decoder layers of Gemma 3 text encoder
- **Causal Audio VAE:** Compact neural audio representation optimized for diffusion

## Performance Benchmarks

- **Speed:** ~18x faster than Wan 2.2 on H100 GPU
- **Temporal scope:** Up to 20 seconds of continuous video with stereo audio
- **Exceeds:** Veo 3 (12s), Sora 2 (16s), Ovi (10s), Wan 2.5 (10s)
- **Rankings (Nov 2025):** 3rd in Image-to-Video, 4th in Text-to-Video on Artificial Analysis

## Community Applications

### Lip-Sync and Portrait Animation
- linoyts created popular "LTX 2.3 Sync" Space (120 likes) for portrait animation with lip-sync
- multimodalart created "Ltx2 Audio To Video" (49 likes) for generating lip-synced videos from images

### Audio-Reactive Content
- Community Spaces enable audio-to-video generation
- Content creators use this for music visualization
- Podcast/video producers generate talking head videos

### First-Last Frame Control
- linoyts created "LTX 2.3 First-Last Frame" (96 likes)
- Enables controlled video generation with specified start and end frames
- Combined with audio generation for complete audiovisual clips

## Limitations

- Performance varies across languages for speech synthesis
- Multi-speaker scenarios may confuse character-speech assignment
- Sequences longer than ~20 seconds show temporal drift
- Lower audio quality when generating non-speech audio
- No explicit reasoning or world-modeling capabilities
