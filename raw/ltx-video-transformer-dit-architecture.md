# LTX-Video Transformer (DiT) Architecture Details

**Source:** https://arxiv.org/abs/2501.00103 (Sections 2.2, 2.3, 2.4, 2.5)

---

## Base Architecture

LTX-Video's transformer builds upon the Pixart-alpha architecture, which extends the DiT (Diffusion Transformer) framework to accept open text inputs rather than ImageNet class labels.

### Model Size
- **Original paper model:** < 2B parameters
- **Later versions:** Scaled to 13B parameters (v0.9.7+)

## Key Architectural Enhancements

### 1. Rotary Positional Embeddings (RoPE)

Replaces traditional absolute positional embeddings:

- **Coordinate system:** Normalized fractional coordinates
  - Spatial coordinates computed in pixels relative to predefined max resolution
  - Temporal coordinates computed in seconds relative to predefined max duration
  - Incorporating original FPS into temporal embedding produces more natural motion
- **Frequency spacing:** Exponential (NOT inverse-exponential)
  - Exponential spacing consistently produces lower training loss
  - Aligns with theory that truncating lower frequencies improves performance
- **Three variants tested:**
  1. Absolute positional embeddings (baseline)
  2. RoPE with fractional coordinates
  3. RoPE with normalized fractional coordinates (winner)
- **Benefit:** Enables generalization to unseen resolution/duration combinations

### 2. QK Normalization

- **Method:** RMSNorm applied to queries and keys before dot-product attention
- **Purpose:** Prevents extremely large attention logit values that cause near-zero entropy weights
- **Comparison:** RMSNorm outperformed LayerNorm for this purpose
- **Based on:** Findings from ScalingViT and LargeDiT

### 3. RMSNorm (Throughout)

- Replaces LayerNorm throughout the architecture
- More efficient and empirically better performing

### 4. Full Spatiotemporal Self-Attention

- The highly compressed latent space (1:8192 pixels-to-tokens) makes this feasible
- Essential for generating high-resolution videos with temporal consistency
- Quadratic in token count, hence the importance of high compression

## 3D Transformer Block

The transformer block architecture:
1. RMSNorm
2. Self-Attention (with RoPE and QK normalization)
3. Cross-Attention (for text conditioning)
4. Feed-Forward Network
5. AdaLN modulation (for timestep and conditioning)

## Text Conditioning

### Text Encoder
- **Model:** T5-XXL (pre-trained, frozen)
- **Learnable projection layer** applied to text embeddings

### Conditioning Method
- **Cross-attention** (not MM-DiT)
- Tested MM-DiT (used by SD3, FLUX.1, CogVideoX) but found cross-attention better
- In MM-DiT, text and image tokens share attention layers; cross-attention keeps them separate
- Cross-attention uses dedicated layers for text-image interaction

## Image Conditioning (Image-to-Video)

### Training Approach
- Uses per-token timestep conditioning (no special tokens or additional parameters)
- Based on and extends Open-Sora's approach
- During training: occasionally set first-frame token timesteps to small random values with corresponding noise
- The model learns to use this as a conditioning signal

### Inference Pipeline
1. Conditioning image encoded by causal VAE (temporal dimension = 1)
2. Image latent concatenated with random noise latents
3. Flattened to form initial token set
4. Per-token timesteps: t_c (small value, e.g., 0) for conditioning tokens, t=1 for generation tokens
5. Standard denoising loop with per-token noise levels

### Multi-Condition Support (v0.9.7+)
- Can condition on multiple images/videos at specified frame positions
- Uses `LTXVideoCondition` class with frame_index parameter
- Conditioning strength adjustable per condition (default: 1.0)

## Rectified-Flow Training

### Velocity Prediction
- Follows SD3's approach: predicts velocity v = epsilon - z_0
- More evenly-difficult prediction task than noise prediction

### Timestep Scheduling
- Log-normal distribution (not uniform)
- Shifted toward higher-noise regions depending on token count
- Follows SimpleDiffusion finding: higher resolution needs more noising to maintain SNR
- Clamped at percentiles 0.5 and 99.9 to prevent starvation at distribution tails

### Multi-Resolution Training
- Simultaneously trained on multiple resolution/duration combinations
- All inputs resized to comparable token counts
- Stochastic token dropping at 0-20% rate (eliminates need for complex packing)
- Model generalizes well to unseen configurations after diverse training

### Image Training
- Images included alongside videos as one resolution-duration combination
- Image datasets enrich concept coverage beyond what video datasets provide

## Inference Guidance

### STG/CFG
- Dev models: Typically use STG (Spatiotemporal Guidance) or CFG (Classifier-Free Guidance)
- Distilled models (v0.9.6+): No STG/CFG required
- Negative prompts supported for quality improvement

### Inference Steps
- Dev models: 20-50 steps typical
- Distilled models: As few as 7 steps
- Paper benchmark: 20 steps at 768x512 in 2 seconds on H100
