# LTX-2: Efficient Joint Audio-Visual Foundation Model

## Metadata

- **Title:** LTX-2: Efficient Joint Audio-Visual Foundation Model
- **Authors:** Yoav HaCohen, Benny Brazowski, Nisan Chiprut, Yaki Bitterman, Andrew Kvochko, Avishai Berkowitz, Daniel Shalem, Daphna Lifschitz, and 21 additional authors from Lightricks (29 total)
- **Affiliation:** Lightricks
- **Date:** January 6, 2026
- **arXiv ID:** 2601.03233
- **URL:** https://arxiv.org/abs/2601.03233
- **License:** CC BY 4.0
- **Field:** Computer Vision and Pattern Recognition (cs.CV)
- **Code:** https://github.com/Lightricks/LTX-2

## Abstract

LTX-2 is an open-source foundational model capable of generating high-quality, temporally synchronized audiovisual content in a unified manner. The system addresses a significant gap in existing text-to-video models by incorporating quality audio generation alongside video. It employs an asymmetric dual-stream diffusion transformer with a 14B-parameter video stream and a 5B-parameter audio stream, linked through bidirectional audio-video cross-attention, temporal positional embeddings, cross-modality AdaLN conditioning, and modality-aware classifier-free guidance. The model achieves state-of-the-art audiovisual quality and prompt adherence among open-source systems while delivering results comparable to proprietary models at a fraction of their computational cost and inference time. All model weights and code are publicly released under open-source licensing.

## Key Contributions

1. **First Open-Source Joint Audio-Visual Foundation Model:** Generates synchronized video and audio in a single unified pass, a capability previously only available in proprietary systems.

2. **Asymmetric Dual-Stream Architecture:** 19B total parameters with purposeful asymmetric allocation (14B video, 5B audio) reflecting different information densities between modalities.

3. **Bidirectional Audio-Visual Cross-Attention:** Enables sub-frame precision synchronization between visual cues and auditory events using 1D temporal RoPE during cross-modal interactions.

4. **Modality-Aware Classifier-Free Guidance (Modality-CFG):** Extended CFG formulation enabling independent control of text guidance and cross-modal guidance strengths.

5. **Multi-Scale, Multi-Tile Inference:** Three-stage high-resolution generation pipeline achieving native 4K output.

6. **Unprecedented Speed:** 18x faster than comparable models (e.g., Wan 2.2-14B) despite handling both modalities.

## Architecture Details

### Dual-Stream Transformer

| Component | Specification |
|-----------|--------------|
| Total Parameters | 19B |
| Video Stream Parameters | 14B |
| Audio Stream Parameters | 5B |
| Number of Layers | 48 |
| Video Positional Encoding | 3D RoPE (x, y, temporal) |
| Audio Positional Encoding | 1D temporal RoPE |
| Text Encoder | Gemma 3 (12B parameters) |

**Each dual-stream block performs four sequential operations:**
1. **Self-Attention:** Within-modality attention for each stream
2. **Text Cross-Attention:** Textual prompt conditioning for both streams
3. **Audio-Visual Cross-Attention:** Bidirectional inter-modal exchange
4. **Feed-Forward Networks:** Refinement

### Cross-Modality AdaLN

The system employs cross-modality AdaLN gates where the scaling and shift parameters for one modality are conditioned on the hidden states of the other, enabling temporal synchronization even when modalities operate at different resolutions or timesteps.

### Cross-Attention Positional Encoding

During audio-visual cross-attention, only temporal RoPE components are used. This enforces synchronization in time rather than spatial alignment, allowing the model to map visual cues (e.g., object impact) to auditory events (e.g., foley sound) with sub-frame precision.

### Text Conditioning Pipeline

**Multi-Layer Feature Extraction from Gemma 3:**
1. Extracts features across all decoder layers (not just final layer)
2. Applies mean-centered scaling across sequence and embedding dimensions
3. Flattens intermediate outputs into shape [B, T, D x L]
4. Projects to target dimension D using learnable matrix W

**Thinking Tokens Mechanism:**
- Learnable thinking tokens appended to input tokens, replacing padded positions
- Function as global information carriers with aggregated contextual information
- Addresses limitations of causal attention in decoder-only LLMs

### Audio VAE Architecture

| Component | Specification |
|-----------|--------------|
| Input | Stereo audio at 16 kHz |
| Process | Mel-spectrogram per channel, concatenated |
| Latent Dimensions | 128-dimensional feature vectors |
| Temporal Resolution | ~1/25 seconds per token |

### Vocoder (HiFi-GAN)

| Component | Specification |
|-----------|--------------|
| Architecture | Modified HiFi-GAN V1 |
| Input | Two-channel mel-spectrograms at 16 kHz |
| Output | Two-channel (stereo) waveform at 24 kHz |
| Enhancement | Doubled channel count for stereo modeling |

## Training Methodology

### Data
- Subset of the same dataset used in LTX-Video
- Focus on video clips with significant audio components
- Balanced visual and auditory content distribution

### Captioning System
Custom multimodal captioning generating descriptions that capture:
- **Audio:** Music, ambient sounds, precise dialogue transcriptions with speaker/language/accent identification
- **Visual:** Camera motion, lighting conditions, subject behavior
- **Integration:** Factual, emotionally-neutral descriptions of what is seen and heard

### Training Process
- Flow-matching loss for velocity field prediction
- Joint optimization of projection matrix W during initial stage with frozen Gemma 3 weights
- Text connector blocks trained simultaneously with DiT blocks

## Inference Details

### Modality-Aware CFG Formula

```
M_hat(x,t,m) = M(x,t,m) + s_t * (M(x,t,m) - M(x,empty,m)) + s_m * (M(x,t,m) - M(x,t,empty))
```

Where s_t controls text guidance strength and s_m controls cross-modal guidance.

**Empirical settings:**
- Video stream: s_t=3, s_m=3
- Audio stream: s_t=7, s_m=3

### Multi-Scale, Multi-Tile Inference Pipeline

1. **Base Generation:** ~0.5 Megapixels establishing global composition and motion dynamics
2. **Latent Upscaling:** Increase spatial resolution while maintaining temporal consistency and auditory alignment
3. **Tiled Refinement:** Partition upscaled latents into overlapping tiles for 1080p detail synthesis

## Benchmark Results

### Inference Speed

| Model | Modality | Parameters | Seconds/Step |
|-------|----------|-----------|--------------|
| Wan 2.2-14B | Video Only | 14B | 22.30s |
| **LTX-2** | **Audio + Video** | **19B** | **1.22s** |

**18x speedup** despite handling both modalities with more total parameters.

### Visual Performance (Artificial Analysis, Nov 2025)
- **3rd** in Image-to-Video generation
- **4th** in Text-to-Video generation
- Surpasses Sora 2 Pro and Wan 2.2-14B

### Audiovisual Evaluation
- Significantly outperforms open-source alternatives such as Ovi
- Comparable to leading proprietary models including Veo 3 and Sora 2

### Temporal Capacity Comparison

| Model | Max Duration |
|-------|-------------|
| **LTX-2** | **20 seconds** |
| Veo 3 | 12 seconds |
| Sora 2 | 16 seconds |
| Ovi | 10 seconds |
| Wan 2.5 | 10 seconds |

## Output Specifications

| Feature | Specification |
|---------|--------------|
| Max Resolution | 4K (native) via multi-tile inference |
| Base Resolution | ~0.5 MP for initial generation |
| Max Frame Rate | 50 fps |
| Audio Output | Stereo 24 kHz |
| Max Duration | 20 seconds synchronized A/V |
| Audio Capabilities | Dialogue, music, foley, ambient, lip sync |

## Limitations

- Language performance varies; underrepresented languages show weaker speech synthesis
- Multi-speaker scenarios may inconsistently assign dialogue to characters
- Temporal sequences exceeding ~20 seconds risk temporal drift, degraded synchronization, or reduced scene diversity
- Model lacks explicit reasoning or world-modeling capabilities; requires external LLM support for such tasks
