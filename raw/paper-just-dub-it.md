# JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion

## Metadata

- **Title:** JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion
- **Authors:** Anthony Chen (Tel Aviv University and Lightricks), Naomi Ken Korem, Tavi Halperin, Matan Ben Yosef, Urska Jelercic, Ofir Bibi, Or Patashnik, Daniel Cohen-Or
- **Affiliations:** Tel Aviv University, Lightricks
- **Date:** January 2026
- **arXiv ID:** 2601.22143
- **URL:** https://arxiv.org/abs/2601.22143
- **Project Page:** https://justdubit.github.io/
- **Code:** https://github.com/justdubit/just-dub-it

## Abstract

JUST-DUB-IT introduces a single-model approach that adapts a foundational audio-video diffusion model for video-to-video dubbing via a lightweight LoRA. The approach enables the model to condition on an input audio-video while jointly generating translated audio and synchronized facial motion.

## Key Contributions

1. **Single-Model Dubbing Pipeline:** Replaces complex multi-stage dubbing pipelines with a single model adapted via lightweight LoRA fine-tuning.

2. **Joint Audio-Visual Generation:** Simultaneously generates translated audio and synchronized facial motion, maintaining consistency between modalities.

3. **Self-Supervised Training Data:** Leverages the generative model itself to synthesize paired multilingual videos of the same speaker by generating multilingual videos with language switches within a single clip, then inpainting the face and audio in each half.

4. **Identity and Lip Sync Preservation:** By leveraging the rich generative prior of the audio-visual foundation model, preserves speaker identity and lip synchronization while being robust to complex motion and real-world dynamics.

## Methodology

### Foundation Model
Built on top of LTX-2's audio-visual diffusion model architecture.

### LoRA Adaptation
- Lightweight LoRA (Low-Rank Adaptation) applied to the foundation model
- Enables video-to-video dubbing without full model fine-tuning
- Conditions on input audio-video for generating translated output

### Training Data Generation
Novel self-supervised approach:
1. Use the generative model to create multilingual videos of the same speaker
2. Generate language switches within a single clip
3. Inpaint face and audio in each half of the clip
4. Use resulting pairs for LoRA training

### Results
- Higher visual fidelity than existing dubbing pipelines
- Better lip synchronization
- Greater robustness to complex motion and real-world dynamics
- Produces high-quality dubbed videos compared to existing approaches

## Relationship to LTX Model Family

JUST-DUB-IT builds directly on the LTX-2 foundation model, demonstrating the versatility of the joint audio-visual architecture for downstream tasks beyond generation. The work shows that LTX-2's bidirectional audio-visual cross-attention and synchronized generation capabilities can be effectively adapted for video editing/dubbing tasks via parameter-efficient fine-tuning.

## Sources

- https://arxiv.org/abs/2601.22143
- https://justdubit.github.io/
- https://github.com/justdubit/just-dub-it
