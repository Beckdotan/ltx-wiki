# LTX-Video Training Data Preparation and Methodology

**Source:** https://arxiv.org/abs/2501.00103 (Sections 3, 4.1)

---

## Training Data Overview

The training dataset comprises:
- Publicly available data
- Licensed material
- Designed for diversity and comprehensiveness

## Data Processing Pipeline

### 1. Quality Control and Filtering

#### Aesthetic Model
- Trained on tens of thousands of manually-tagged image pairs
- Evaluates both videos and images
- **Training approach:**
  - Millions of samples labeled using a multi-labeling network
  - Pairs sampled that share at least one of the top three labels (minimizes distribution shifts)
  - Siamese Network trained to predict aesthetic scores preserving order relations
- **Application:** Samples below aesthetic threshold are filtered out

#### Motion Filtering
- Videos with insignificant motion are removed
- Ensures dataset focuses on dynamic content

#### Aspect Ratio Processing
- Black bars cropped from videos
- Standardizes aspect ratios
- Enhances usable visual data

### 2. Captioning and Metadata

#### Automatic Captioner
- Internal automatic image and video captioner
- Re-captions the entire training set
- Produces detailed, descriptive captions
- Example caption style: detailed descriptions of subjects, actions, environments, camera work, lighting

#### Caption Characteristics
- English language
- Detailed and elaborate (matching the recommended prompt style)
- Include: subject description, action, environment, lighting, camera angle/movement

### 3. Fine-Tuning Data

- Subset of most aesthetic content from filtering
- Used for post-pre-training fine-tuning
- Produces more visually appealing generation results

## Training Methodology

### Optimizer
- ADAM-W optimizer

### Training Stages
1. **Pre-training:** Full dataset with diverse content
2. **Fine-tuning:** High-aesthetic subset for quality refinement

### Multi-Resolution Training
- Simultaneously trains on multiple resolution/duration combinations
- All samples resized to comparable token counts
- Stochastic token dropping (0-20%) to fix token counts
- Model generalizes to unseen resolution/duration configurations

### Image-Video Co-Training
- Images included alongside videos
- Treated as one resolution-duration combination
- Enriches concept coverage beyond video datasets

### Timestep Scheduling
- Log-normal distribution (not uniform)
- Shifted toward higher noise for larger token counts
- Clamped at percentiles 0.5 and 99.9

### Evaluation Setup (from paper)
- 1,000 text prompts for T2V evaluation
- 1,000 image+prompt pairs for I2V evaluation (images from FLUX.1)
- 5-second videos at 768x512
- 40 diffusion steps
- 20-person human survey
- Compared against: Open-Sora Plan, CogVideoX (2B), PyramidFlow
- Evaluation criteria: visual quality, motion fidelity, prompt adherence
