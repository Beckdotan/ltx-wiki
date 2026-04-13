# LTX-Video: Training Methodology and Data Preparation

## Source
- Extracted from: LTX-Video paper (arXiv:2501.00103), Sections 2.5, 3, 4.1
- Additional context from: LTX-2 paper (arXiv:2601.03233), Section 5

## Training Framework

### Rectified Flow

LTX-Video uses Rectified Flow for training, where:
- **Forward process:** z_t = (1-t)*z_0 + t*epsilon, with t in [0,1]
- **Noise:** epsilon sampled from standard normal N(0,I)
- **Prediction target:** velocity v = epsilon - z_0 (following SD3)
- **Denoising step:** z_{t-dt} = z_t - dt * v_t^theta

### Loss Function
- Standard velocity prediction MSE loss for the transformer
- VAE trained with combined losses: MSE, Video-DWT (L1), LPIPS, Reconstruction-GAN

### Timestep Scheduling During Training
- Sampled from **log-normal distribution** (replacing uniform distribution)
- Shifted toward higher-noise regions based on token count
  - Higher resolutions/longer videos get more high-noise training
  - Maintains proper SNR across different scales
- PDF clamped at percentiles 0.5 and 99.9 to prevent starvation

### Optimizer
- **ADAM-W** optimizer
- Specific learning rate and schedule not disclosed in paper

### Multi-Resolution Training Strategy
- Simultaneous training on multiple resolution and duration combinations
- All input samples contain approximately the same number of tokens (via resizing)
- **Stochastic token dropping:** 0-20% tokens randomly dropped per sample
  - Fixes token counts across all sequences
  - Eliminates need for complex packing or padding
  - Preserves training data diversity
- Model generalizes to unseen resolution/duration combinations after training

### Joint Image-Video Training
- Images included as one of the resolution-duration combinations
- Enriches concept diversity beyond what video datasets provide
- Enabled by causal VAE design (first frame = image)

### Training Stages
1. **Pre-training:** Full training on diverse dataset
2. **Fine-tuning:** Subset of high-aesthetic videos for quality improvement

## Data Preparation Pipeline

### Dataset Composition
- **Publicly available data** supplemented with **licensed material**
- Designed for diverse and comprehensive training coverage

### Quality Control and Filtering

**Aesthetic Model:**
- Trained on tens of thousands of manually tagged image pairs
- Manual tagging identifies superior image in each pair for aesthetic quality
- To sample pairs for tagging: millions of samples labeled via multi-labeling network, pairs sampled sharing at least one of top three labels (minimizes distribution shifts)
- Architecture: Siamese Network predicting aesthetic score preserving order relations
- All training samples scored; those below threshold filtered out

**Motion Filtering:**
- Videos with insignificant motion actively removed
- Focus on dynamic content relevant to model capabilities

**Aspect Ratio Filtering:**
- Black bars cropped out
- Standardizes aspect ratios
- Enhances usable visual data

### Captioning and Metadata Enhancement

**LTX-Video Captioning:**
- Internal automatic image and video captioner
- Re-captions entire training set
- Ensures accurate and relevant text descriptions
- Improves alignment between visual content and textual annotations

**LTX-2 Captioning (Enhanced):**
- New captioning system describing BOTH visual and auditory tracks
- Captures every meaningful action, appearance, and sound
- Comprehensive yet factual (no emotional interpretation)
- Captures:
  - Full soundscape (music, ambient sounds, dialogue)
  - Precise transcriptions with speaker, language, accent identification
  - Camera motion, lighting, subject behavior
  - Visual information at fine-grained level

### Data Statistics (from LTX-Video paper)
- Word cloud visualization shows diverse vocabulary distribution
- Clip durations and words-per-caption distributions provided
- Captions tend to be detailed and elaborate (model works best with detailed prompts)

## VAE Training

### Losses Used
1. **Pixel reconstruction (MSE):** Standard pixel-wise loss
2. **Video DWT (L1):** 8 3D Discrete Wavelet Transform pairs, L1 distance
3. **Perceptual (LPIPS):** Feature-space perceptual similarity
4. **Reconstruction GAN:** Novel discriminator seeing both original and reconstruction

### Denoising Decoder Training
- Decoder trained to map noisy latents to clean pixels at varying noise levels
- Noise levels in range [0, 0.2]
- Adaptive normalization layers conditioned on timestep
- Multi-layer noise injection with learned per-channel noise levels
- Uniform log-variance across all latent channels

## LTX-2 Training Specifics

### Data
- Subset of LTX-Video dataset
- Focus on clips with significant and informative audio components
- Balanced distribution of visual and auditory content

### Architecture Training
- Flow-matching loss for velocity field matching
- Progressive joint training of audio and video streams
- Text encoder (Gemma3-12B) kept frozen
- Feature extractor projection matrix W jointly optimized briefly, then frozen
- Text connector blocks trained with main DiT blocks

### Distillation
- Distilled versions available (8 steps, CFG=1)
- LoRA adapters for distilled models
- Significant speedup with manageable quality reduction

## Key Training Insights from Ablations

### Reconstruction GAN vs Traditional GAN
- At 1:192 compression, standard GAN losses fail to achieve consistent reconstruction
- Artifacts particularly visible in high-motion frames with intricate detail
- rGAN significantly reduces visible artifacts

### RoPE Frequency Spacing
- Controlled experiments with same architecture, hyperparameters, and data
- Training loss with inverse-exponential spacing remains consistently higher
- Exponential spacing confirmed as better choice

### Denoising VAE Decoder
- Internal user study comparing denoising decoder (t=0.05) vs standard decoder (t=0.0)
- Videos with denoising strongly preferred
- Improvement particularly evident in high-motion videos
- Compression artifacts mitigated by decoder's last-step denoising
