# LTX-Video: Video Diffusion Transformer Architecture Deep Dive

## Source
- Extracted from: LTX-Video paper (arXiv:2501.00103), Sections 2.2-2.5
- Additional context from: LTX-2 paper (arXiv:2601.03233), Section 3.1

## Video Transformer Architecture (LTX-Video)

### Base Design
- Built upon **PixArt-alpha** architecture, which extends **DiT** (Diffusion Transformers) for open text conditioning
- ~2B parameters (original release, before scaling to 13B)
- Full spatiotemporal self-attention across all video tokens

### 3D Transformer Block
Each block performs:
1. Self-attention with RoPE and QK normalization
2. Cross-attention with text embeddings
3. Feed-forward network
- Uses RMSNorm (replacing LayerNorm from PixArt-alpha)
- Timestep conditioning via AdaLN (Adaptive Layer Normalization)

### Rotary Positional Embedding (RoPE)

**Three variants tested:**
1. Absolute positional embeddings -- baseline
2. RoPE with fractional coordinates
3. RoPE with fractional coordinates normalized by predefined maximum coordinates -- **winner**

**Key design choices:**
- Spatial coordinates computed in **pixels** relative to predefined max resolution
- Temporal coordinates computed in **seconds** relative to predefined max duration
- Incorporating original FPS into temporal embedding produces more natural motion
- **Exponential frequency spacing** (not inverse-exponential as in many open-source implementations)
  - Ablation showed consistently lower training loss with exponential spacing
  - Aligns with theoretical work suggesting truncating lower frequencies improves performance

### QK Normalization
- RMSNorm applied to queries and keys before dot-product attention
- Prevents extremely large attention logit values
- Avoids near-zero entropy attention weights
- RMSNorm found to perform better than LayerNorm for this purpose
- Following findings from ScalingViT and LargeDiT

### Text Conditioning
- **Method:** Cross-attention (separate from self-attention)
- **Text encoder:** T5-XXL (frozen, pre-trained)
- Learnable projection layer on input text embeddings
- Cross-attention found to work **better than MM-DiT** (used by SD3, FLUX.1, CogVideoX)
  - MM-DiT processes text in parallel with image tokens via unified attention
  - LTX-Video found cross-attention more effective

### Image Conditioning (Image-to-Video)
- **Per-token timestep mechanism:** Different diffusion timestep per token
- Conditioning image encoded by causal VAE into latent with temporal dimension of 1
- Concatenated with noise latents, flattened to form initial token set
- Conditioning tokens: timestep t_c (small value, e.g., 0)
- Generation tokens: timestep t = 1 (pure noise)
- No additional parameters or special tokens needed
- During training: occasionally set first-frame token timestep to small random value
- Model quickly learns to use this as conditioning signal

## Rectified-Flow Training

### Formulation
- Forward process: z_t = (1-t)*z_0 + t*epsilon
- Prediction target: velocity v = epsilon - z_0
- Denoising: z_{t-dt} = z_t - dt * v_t^theta

### Timestep Scheduling
- Sampled from **log-normal distribution** (following SD3)
- **Shifted toward higher-noise regions** depending on number of tokens
- Rationale: higher resolutions need more noising to maintain SNR (SimpleDiffusion)
- PDF clamped at percentiles 0.5 and 99.9 to prevent starvation at tails

### Multi-Resolution Training
- Trained simultaneously on multiple width/height/duration combinations
- All samples resized to approximately same token count
- **Stochastic token dropping** at rates 0-20% to fix token counts across sequences
- Eliminates need for complex token-packing or padding strategies
- Model generalizes well to unseen resolution/duration combinations

### Image Training
- Images included alongside video training as one resolution-duration combination
- Enriches concept diversity (concepts not typically present in video datasets)
- Enabled by causal VAE design

### Optimizer
- ADAM-W
- Post-pretraining fine-tuning on high-aesthetic video subset

## Dual-Stream Architecture (LTX-2 Extension)

### Asymmetric Design
- **Video stream:** 14B parameters, processes 3D spatiotemporal latents
- **Audio stream:** 5B parameters, processes 1D temporal latents
- Shared depth but different widths

### Each Dual-Stream Block:
1. Self-Attention within same modality
2. Text Cross-Attention for prompt conditioning
3. Audio-Visual Cross-Attention for inter-modal exchange
4. Feed-Forward Network (FFN)

### Cross-Modal Attention
- Bidirectional: video queries attend to audio keys/values AND vice versa
- Temporal 1D RoPE used during cross-attention (only temporal alignment, not spatial)
- AdaLN modulation conditioned on stream's diffusion timestep scales Q and (K,V) independently
- Additional AdaLN gate with parameters from other modality's timestep

### Text Conditioning (LTX-2)
- **Encoder:** Gemma3-12B (multilingual, decoder-only LLM, frozen)
- **Multi-Layer Feature Extractor:**
  - Extracts features from ALL decoder layers
  - Mean-centered scaling per layer
  - Projected via learnable matrix W to target dimension
  - W jointly optimized initially, then frozen
- **Thinking Tokens:**
  - Learnable tokens appended to text connector
  - Replace padding positions
  - Serve as global information carriers
  - Separate text connectors for video and audio streams
- **Bidirectional refinement blocks** compensate for causal-only LLM limitation

### Modality-Aware CFG (LTX-2)
- Extended CFG with separate text and cross-modal guidance terms
- M_hat(x,t,m) = M(x,t,m) + s_t*(text guidance) + s_m*(cross-modal guidance)
- Video: s_t=3, s_m=3; Audio: s_t=7, s_m=3
- Increasing s_m promotes mutual information refinement between modalities

## Key Architecture Decisions Summary

| Decision | LTX-Video Choice | Alternative | Rationale |
|----------|------------------|-------------|-----------|
| Positional encoding | RoPE (normalized fractional) | Absolute PE, unnormalized RoPE | Better multi-resolution generalization |
| RoPE frequency | Exponential spacing | Inverse-exponential | Lower training loss, theory-supported |
| QK normalization | RMSNorm | LayerNorm, None | Better performance, prevents entropy collapse |
| Text conditioning | Cross-attention | MM-DiT | Found to work better empirically |
| Patchifier location | In VAE encoder | In transformer input | Enables higher effective compression |
| VAE convolutions | 3D convolutions | Separable 2D+1D | Slightly better quality |
| VAE type | Causal | Non-causal | Enables image+video joint training |
