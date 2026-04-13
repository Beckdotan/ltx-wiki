# LTX-2: Efficient Joint Audio-Visual Foundation Model

## Metadata
- **Title:** LTX-2: Efficient Joint Audio-Visual Foundation Model
- **Authors:** Yoav HaCohen, Benny Brazowski, Nisan Chiprut, Yaki Bitterman, Andrew Kvochko, Avishai Berkowitz, Daniel Shalem, Daphna Lifschitz, Dudu Moshe, Eitan Porat, Eitan Richardson, Guy Shiran, Itay Chachy, Jonathan Chetboun, Michael Finkelson, Michael Kupchick, Nir Zabari, Nitzan Guetta, Noa Kotler, Ofir Bibi, Ori Gordon, Poriya Panet, Roi Benita, Shahar Armon, Victor Kulikov, Yaron Inger, Yonatan Shiftan, Zeev Melumian, Zeev Farbman
- **Affiliation:** Lightricks
- **Date:** January 2025 (arXiv: 2601.03233)
- **Source URL:** https://arxiv.org/abs/2601.03233
- **GitHub:** https://github.com/Lightricks/LTX-2
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2

## Abstract

Recent text-to-video diffusion models can generate compelling video sequences, yet they remain silent -- missing the semantic, emotional, and atmospheric cues that audio provides. We introduce LTX-2, an open-source foundational model capable of generating high-quality, temporally synchronized audiovisual content in a unified manner. LTX-2 consists of an asymmetric dual-stream transformer with a 14B-parameter video stream and a 5B-parameter audio stream, coupled through bidirectional audio-video cross-attention layers with temporal positional embeddings and cross-modality AdaLN for shared timestep conditioning. This architecture enables efficient training and inference of a unified audiovisual model while allocating more capacity for video generation than audio generation. We employ a multilingual text encoder for broader prompt understanding and introduce a modality-aware classifier-free guidance (modality-CFG) mechanism for improved audiovisual alignment and controllability. Beyond generating speech, LTX-2 produces rich, coherent audio tracks that follow the characters, environment, style, and emotion of each scene -- complete with natural background and foley elements. In our evaluations, the model achieves state-of-the-art audiovisual quality and prompt adherence among open-source systems, while delivering results comparable to proprietary models at a fraction of their computational cost and inference time. All model weights and code are publicly released.

## Key Contributions

1. **Efficient Asymmetric Dual-Stream Architecture:** A transformer-based backbone featuring modality-specific streams linked via bidirectional cross-attention and cross-modality AdaLN for shared timestep conditioning.

2. **Text Processing Blocks with Thinking Tokens:** A refined text-conditioning module employing multi-token prediction for enhanced prompt understanding and semantic stability.

3. **Compact Neural Audio Representation:** An efficient causal audio VAE that produces a high-fidelity, 1D latent space optimized for diffusion-based training and inference.

4. **Modality-Aware Classifier-Free Guidance:** A novel Bimodal CFG scheme that allows for independent control over cross-modal guidance scales, significantly improving audiovisual alignment.

## Architecture Details

### Asymmetric Dual-Stream Transformer
- **Total parameters:** ~19B (14B video + 5B audio)
- **Video stream:** 14B parameters, processes spatiotemporal video latents
- **Audio stream:** 5B parameters, processes 1D temporal audio latents
- **Design rationale:** Video has fundamentally higher information density than audio, so more parameters are allocated to video

### Each Dual-Stream Block Performs:
1. Self-Attention within same modality
2. Text Cross-Attention for prompt conditioning
3. Audio-Visual Cross-Attention for inter-modal exchange
4. Feed-Forward Network (FFN) for refinement

### Positional Encoding
- **Video stream:** 3D RoPE (spatial x, y + temporal t)
- **Audio stream:** 1D temporal RoPE
- **Cross-attention:** Only temporal component of RoPE used (focus on time synchronization, not spatial alignment)

### Cross-Modality Conditioning
- **Bidirectional cross-attention** layers throughout model depth
- **Cross-modality AdaLN gates:** Scaling and shift parameters for one modality conditioned on hidden states of the other
- **RMS normalization** interleaved between operations
- Enables lip-synchronization, environmental acoustics, and foley sound alignment

### Video VAE
- Built on the same spatiotemporal causal VAE from LTX-Video
- High compression ratio maintained from LTX-Video design

### Audio VAE
- **Input:** Stereo mel-spectrograms at 16 kHz
- **Two-channel processing:** Mel-spectrogram computed per channel, concatenated along channel dimension
- **Latent representation:** Each token ~1/25 seconds of audio, 128-dimensional feature vector
- **Causal architecture** for streaming capability
- **Vocoder:** Modified HiFi-GAN for joint stereo synthesis and upsampling (16 kHz -> 24 kHz output)
  - Doubled channels relative to HiFi-GAN V1 for stereo modeling

### Text Conditioning (Deep Text Conditioning)
- **Text encoder:** Gemma3-12B (multilingual, decoder-only LLM)
- **Multi-Layer Feature Extractor:** Extracts features across ALL decoder layers (not just final layer)
  - Mean-centered scaling applied across sequence and embedding dimensions per layer
  - Flattened to [B, T, D x L], then projected via learnable dense projection matrix W
  - W jointly optimized with LTX-2, then frozen for all subsequent training
  - LLM weights kept entirely frozen throughout
- **Thinking Tokens:** Learnable tokens appended to text connector input
  - Replace padded positions for computational utility
  - Serve as global information carriers
  - Allow aggregation of contextual information
  - Separate text connectors for video and audio streams
- **Bidirectional refinement:** Text connector uses full bidirectional attention (compensating for causal-only LLM)

### Inference
- **Modality-aware CFG formula:** M_hat(x,t,m) = M(x,t,m) + s_t*(M(x,t,m) - M(x,empty,m)) + s_m*(M(x,t,m) - M(x,t,empty))
  - s_t controls textual guidance strength
  - s_m controls cross-modal guidance strength
  - Video stream defaults: s_t=3, s_m=3
  - Audio stream defaults: s_t=7, s_m=3
- **Multi-scale, multi-tile inference** for 1080p output:
  1. Base generation at ~0.5 MP
  2. Latent upscaling (dedicated latent upscaler)
  3. Tiled refinement with overlapping spatial/temporal tiles
  4. Blending in latent space before final VAE decoding

### Training
- **Data:** Subset of LTX-Video dataset focusing on clips with significant audio
- **Captioning:** New system describing both visual and auditory tracks in exhaustive detail
  - Captures full soundscape (music, ambient sounds, dialogue transcription)
  - Speaker, language, and accent identification
  - Camera motion, lighting, subject behavior
- **Flow-matching loss** for velocity field matching

## Evaluation Results

### Audiovisual Quality
- **Significantly outperforms** open-source alternatives (Ovi)
- **Comparable to** proprietary models (Veo 3, Sora 2) in human preference studies
- Evaluated on visual realism, audio fidelity, and temporal synchronization

### Video-Only Benchmarks (Artificial Analysis rankings, November 2025)
- **3rd in Image-to-Video** generation
- **4th in Text-to-Video** generation
- Surpassed Sora 2 Pro and Wan 2.2-14B

### Inference Speed
- **~18x faster** than Wan 2.2 (14B) on H100 GPU
- Also faster than Ovi (which uses two 5B streams)
- Performance gap widens at higher resolutions and longer durations

### Temporal Scope
- Up to **20 seconds** continuous video with synchronized stereo audio
- Exceeds Veo 3 (12s), Sora 2 (16s), Ovi (10s), Wan 2.5 (10s)

## Model Checkpoints
| Checkpoint | Parameters | Notes |
|-----------|-----------|-------|
| ltx-2-19b-dev | 19B | Full model, trainable in bf16 |
| ltx-2-19b-dev-fp8 | 19B | Quantized to fp8 |
| ltx-2-19b-dev-fp4 | 19B | Quantized to nvfp4 |
| ltx-2-19b-distilled | 19B | Distilled (8 steps, CFG=1) |
| ltx-2-spatial-upscaler-x2 | - | 2x spatial upscaler |
| ltx-2-temporal-upscaler-x2 | - | 2x temporal upscaler |

## Capabilities
- Text-to-Video
- Image-to-Video
- Text-to-Audio+Video (joint generation)
- Video-to-Audio
- Audio-to-Video
- Synchronized speech, foley, music, ambient sound generation

## Limitations
- Performance varies across languages (underrepresented dialects)
- Multi-speaker scenarios may have inconsistent speech assignment
- Sequences >20 seconds can show temporal drift
- No explicit reasoning or world-modeling capabilities
