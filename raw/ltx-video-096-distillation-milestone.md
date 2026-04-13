# LTX-Video 0.9.6: The Distillation Milestone

**Source:** https://huggingface.co/Lightricks/LTX-Video (main page references 0.9.6 variants)
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev (lists 0.9.6 as predecessor)

---

## Overview

Version 0.9.6 marks a pivotal release in LTX-Video's history: the introduction of distilled model variants. While maintaining the 2B parameter architecture, it split into two distinct variants optimized for different use cases.

## Model Variants

### ltxv-2b-0.9.6-dev
- **Size:** 2B parameters
- **Quality:** Good quality, improved over 0.9.5
- **VRAM:** Lower requirement than 13B models (~12 GB)
- **Inference steps:** 20-50 (standard)
- **Guidance:** STG/CFG used

### ltxv-2b-0.9.6-distilled
- **Size:** 2B parameters
- **Speed:** 15x faster than the dev model
- **Real-time capable:** Generates video faster than playback
- **No STG/CFG required:** Major simplification of inference pipeline
- **Inference steps:** Significantly fewer (likely ~7-10)
- **VRAM:** ~8 GB estimated

## Hugging Face Repositories
- `Lightricks/LTX-Video-0.9.6-dev` (gated access)
- `Lightricks/LTX-Video-0.9.6-distilled` (gated access)

## Significance

### 15x Speed Improvement
The distilled variant achieved a 15x speedup over the dev model. This was accomplished through:
- Knowledge distillation from the dev model
- Elimination of STG (Spatiotemporal Guidance) / CFG (Classifier-Free Guidance) requirements
- Dramatically reduced inference step count

### No STG/CFG Requirement
This is a major practical improvement:
- Standard models need guidance (CFG) which roughly doubles computation per step
- The distilled model internalizes the guidance, removing this overhead
- Combined with fewer steps, this explains the 15x speedup

### Practical Impact
- Made real-time video generation accessible on consumer GPUs (8 GB VRAM)
- Opened up interactive use cases
- Demonstrated that the LTX-Video architecture responds well to distillation

## Continued Availability

The 0.9.6 variants remain available and recommended for users who:
- Need a smaller model (2B vs 13B)
- Have limited VRAM (8-12 GB)
- Prioritize speed over absolute quality
- Want a proven, stable baseline

The 0.9.6 models are referenced alongside 0.9.7/0.9.8 on the main model page, suggesting they remain actively supported and recommended for resource-constrained setups.
