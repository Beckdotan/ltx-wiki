# LTX-Video ICLoRA: Controlled Video Generation Adapters

**Source:** https://huggingface.co/Lightricks/LTX-Video (main page references)
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev (lists ICLoRA variants)

---

## Overview

Starting with v0.9.7, LTX-Video introduced ICLoRA (Image-Conditioned LoRA) adapters that enable controlled video generation. These are LoRA-based adapters that work with the base 13B model to add conditioning from structural inputs like depth maps, pose keypoints, and edge maps.

## Available ICLoRA Adapters

### 1. ICLoRA-Depth
- **Model:** `ltxv-13b-0.9.7-ICLoRA-depth`
- **Purpose:** Condition video generation on depth map sequences
- **Use case:** 3D-aware generation, maintaining spatial depth relationships
- **Input:** Depth map video (e.g., from MiDaS, DPT, or other depth estimators)

### 2. ICLoRA-Pose
- **Model:** `ltxv-13b-0.9.7-ICLoRA-pose`
- **Purpose:** Condition video generation on human pose keypoints
- **Use case:** Human motion control, dance/action generation
- **Input:** Pose keypoint video (e.g., from OpenPose, MediaPipe)

### 3. ICLoRA-Canny
- **Model:** `ltxv-13b-0.9.7-ICLoRA-canny`
- **Purpose:** Condition video generation on edge maps
- **Use case:** Structure preservation, guided generation from outlines
- **Input:** Canny edge detection video

### 4. ICLoRA-Detailer
- **Model:** `ltxv-13b-0.9.7-ICLoRA-detailer`
- **Purpose:** Enhanced fine detail generation
- **Use case:** Improving detail quality in generated videos
- **Input:** Works as a quality enhancement adapter

## Technical Details

### Architecture
- LoRA (Low-Rank Adaptation) adapters applied to the base transformer
- Likely applied to attention layers (Q, K, V projections)
- Condition signal injected through the LoRA weights
- Compatible with both dev and distilled base models

### Compatibility
- Works with `ltxv-13b-0.9.7-dev` and `ltxv-13b-0.9.7-distilled`
- Updated versions available for 0.9.8

### Access
- Most ICLoRA models are gated on Hugging Face (require authentication/acceptance)

## Integration with Multi-Condition Generation

ICLoRA adapters work alongside the multi-condition generation system introduced in 0.9.7:
- Can specify conditioning media at specific frame positions
- Adjustable conditioning strength
- Compatible with the `LTXConditionPipeline` API

## Comparison to Other Controlled Generation Approaches

LTX-Video's ICLoRA approach differs from:
- **ControlNet:** Uses a separate parallel network; ICLoRA uses lightweight LoRA adapters
- **IP-Adapter:** Focuses on image style/content; ICLoRA focuses on structural control
- **T2I-Adapter:** Similar concept but ICLoRA is specifically designed for video

## Use Cases

1. **Character animation:** Pose conditioning to match specific movements
2. **Scene reconstruction:** Depth conditioning to maintain spatial layout
3. **Style transfer with structure:** Canny edges preserve structure while changing appearance
4. **Quality enhancement:** Detailer adapter for finer output quality
