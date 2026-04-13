# LTX-Video Model Versions and HuggingFace Ecosystem

## Metadata
- **Source:** HuggingFace Model Cards and Documentation
- **Organization:** Lightricks (verified on HuggingFace)
- **Date Accessed:** April 2026
- **HuggingFace URL:** https://huggingface.co/Lightricks
- **Diffusers Docs:** https://huggingface.co/docs/diffusers/main/en/api/pipelines/ltx_video

## Organization Overview

Lightricks is a verified company on HuggingFace focused on Generative AI, Computer Vision, Audio and Speech, LLMs and Diffusion models for content editing and creation. They have 54 team members, 37 models, 8 spaces, 3 datasets, and 3 collections.

## Model Evolution Timeline

### LTX-Video (Original, 2024)
- **Paper:** arXiv:2501.00103 (December 2024)
- **Architecture:** DiT-based video generation
- **Key feature:** First DiT model capable of real-time video generation
- **Parameters:** 2B (original), scaled to 13B in v0.9.8
- **Resolution:** Up to 1216x704 at 30 FPS
- **Compression:** 1:192 Video-VAE

### LTX-Video v0.9.8 (Latest LTX-Video version)
- **Models available:**
  - ltxv-13b-0.9.8-dev (highest quality, more VRAM)
  - ltxv-13b-0.9.8-distilled (faster, less VRAM)
  - ltxv-13b-0.9.8-mix (balanced speed-quality, multi-scale rendering)
  - ltxv-2b-0.9.8-distilled (smallest, lightest)
  - FP8 quantized versions of all above
- **Special versions:** ICLoRA variants (Depth, Pose, Canny), Temporal & Spatial upscalers
- **Community stats:** 567,927 monthly downloads, 100+ Spaces, 24 adapters, 25 finetunes, 16 quantizations

### LTX-2 (2025)
- **Paper:** arXiv:2601.03233
- **Key advance:** Joint audio-video generation
- **Parameters:** 19B (14B video + 5B audio)
- **Checkpoints:** dev, distilled, fp8, fp4, spatial/temporal upscalers
- **Capabilities:** Text-to-Video, Image-to-Video, Text-to-Audio+Video, V2A, A2V

### LTX-2.3 (2025, Latest)
- **Parameters:** 22B
- **Improvements over LTX-2:** Enhanced audio quality, improved visual quality, better prompt adherence
- **Checkpoints:** 22b-dev, 22b-distilled, distilled-lora-384, spatial upscalers (x2, x1.5), temporal upscaler
- **Downloads:** 1.66M monthly

## IC-LoRA Control Models

### LTX-2.3 IC-LoRA Models
- LTX-2.3-22b-IC-LoRA-Union-Control (Canny + Depth + Pose)
- LTX-2.3-22b-IC-LoRA-Motion-Track-Control

### LTX-2 IC-LoRA Models
- LTX-2-19b-IC-LoRA-Union-Control
- LTX-2-19b-IC-LoRA-Detailer (video detail refinement)
- LTX-2-19b-IC-LoRA-Canny-Control

## Technical Requirements
- **Python:** >= 3.10.5 (LTX-Video), >= 3.12 (LTX-2)
- **CUDA:** >= 12.2 (LTX-Video), > 12.7 (LTX-2)
- **PyTorch:** >= 2.1.2 (LTX-Video), ~= 2.7 (LTX-2)
- **Precision:** bfloat16, fp8, fp4 (nvfp4)

## Integration Methods
1. **HuggingFace Diffusers:** LTXPipeline, LTXConditionPipeline, LTXLatentUpsamplePipeline, LTX2Pipeline
2. **ComfyUI:** Built-in LTXVideo nodes via ComfyUI Manager
3. **Local inference scripts:** Official Python codebase
4. **Online demos:** LTX-Studio, Fal.ai, Replicate

## Key Diffusers Integration Details
- Video-VAE performs latent-to-pixel conversion AND final denoising step
- decode_timestep parameter controls denoising decoder behavior (e.g., 0.03)
- decode_noise_scale controls noise injection (e.g., 0.025)
- Supports group_offloading for memory efficiency (~10GB VRAM)
- Supports fp8 layerwise weight-casting

## Datasets Released
- Canny-Control-Dataset
- Cakeify-Dataset
- Squish-Dataset

## Demos and Spaces
- LTX 2.3 Distilled (featured, Running on Zero)
- SAP x FLUX.2-klein-4B (Stage-Aware Prompting)
- LTX Video Fast
- LTX-2 Video Fast
- LTMarX (video watermarking)
- Image Aligner API
