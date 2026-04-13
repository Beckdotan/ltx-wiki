# Research Papers: LTX-Video and LTX-2

**Source:** https://arxiv.org/html/2501.00103, https://arxiv.org/html/2601.03233

---

## Paper 1: LTX-Video: Realtime Video Latent Diffusion

### Citation
```
@article{hacohen2024ltx,
  title={LTX-Video: Realtime Video Latent Diffusion},
  author={HaCohen, Yoav and Chiprut, Nisan and Brazowski, Benny and others},
  journal={arXiv preprint arXiv:2501.00103},
  year={2024}
}
```

### Key Contributions
1. **First DiT-based video generation model** capable of real-time generation
2. **Video-VAE with 1:192 compression ratio** - spatiotemporal downscaling of 32x32x8 pixels per token
3. **Holistic approach** - integrates Video-VAE and denoising transformer responsibilities (VAE decoder handles both latent-to-pixel conversion and final denoising)
4. **Faster-than-real-time generation** - 5 seconds of 24fps video at 768x512 in 2 seconds
5. **Full spatiotemporal self-attention** in highly compressed latent space

### Architecture Innovation
Unlike existing methods that treat VAE and transformer as independent components, LTX-Video relocates the patchifying operation from the transformer's input to the VAE's input. This enables operating in a highly compressed latent space while preserving fine details through the decoder.

### Supported Modes
- Text-to-video generation
- Image-to-video generation
- Both capabilities trained simultaneously

---

## Paper 2: LTX-2: Efficient Joint Audio-Visual Foundation Model

### Citation
```
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```

### Key Contributions

1. **Asymmetric Dual-Stream Architecture:** 14B video stream + 5B audio stream with bidirectional cross-attention
2. **Text Processing Blocks with Thinking Tokens:** Multi-token prediction for enhanced prompt understanding
3. **Compact Neural Audio Representation:** Causal audio VAE producing 128-dimensional tokens at ~1/25 second resolution
4. **Modality-Aware Classifier-Free Guidance:** Independent control over text and cross-modal guidance scales

### Architecture Details

- **Video Stream:** 14B parameters with 3D RoPE (spatial + temporal)
- **Audio Stream:** 5B parameters with 1D temporal RoPE
- **Cross-Attention:** Uses only temporal component of RoPE for audio-visual alignment
- **Text Encoder:** Gemma 3 12B with multi-layer feature extraction
- **Audio:** Causal audio VAE + modified HiFi-GAN vocoder for stereo 24kHz output

### Performance Claims
- **18x faster** than Wan 2.2 on H100 GPU
- **20 seconds** of continuous video with stereo audio (exceeds Veo 3's 12s, Sora 2's 16s)
- **Ranked 3rd** in Image-to-Video and 4th in Text-to-Video on Artificial Analysis (Nov 2025)
- State-of-the-art among open-source audiovisual models

### Design Principles
1. **Decoupled Latent Representations:** Separate VAEs for video and audio (not shared latent space)
2. **Asymmetric Dual Stream:** Video gets more capacity than audio
3. **Cross-Modal Attention:** Bidirectional exchange with temporal RoPE
4. **Deep Multilingual Grounding:** Gemma 3 backbone with thinking tokens for speech synthesis

---

## Paper 3: AVControl (Supporting Paper)

### Citation
```
arXiv: 2603.24793
```

### Relevance
Describes the IC-LoRA (In-Context LoRA) framework used for LTX-2/2.3 control models:
- Union control (canny + depth + pose)
- Motion track control
- Detailer refinement
- Project page: https://matanby.github.io/AVControl/
