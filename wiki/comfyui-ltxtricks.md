---
title: ComfyUI-LTXTricks (Deprecated)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/logtd/ComfyUI-LTXTricks
  - https://github.com/m4rio/ComfyUI-LTXTricks-RUNWAY
  - https://www.runcomfy.com/comfyui-nodes/ComfyUI-LTXTricks
tags:
  - comfyui
  - ltx-video
  - community
  - deprecated
---

# ComfyUI-LTXTricks (Deprecated)

ComfyUI-LTXTricks was a community-created node pack by logtd providing advanced controls for [[ltx-video-overview|LTX Video]] in ComfyUI. All functionality has been migrated to the [[comfyui-ltxvideo-official-nodes|official ComfyUI-LTXVideo repository]].

**Repository:** https://github.com/logtd/ComfyUI-LTXTricks
**Stars:** ~512 | **Forks:** ~23 | **License:** GPL-3.0
**Status:** DEPRECATED

## Techniques Implemented

### RF-Inversion
Semantic image inversion using rectified stochastic differential equations. Allowed inverting existing videos into the model's latent space for editing with new prompts.

### RF-Edit (RF-Solver-Edit)
Fine-grained video editing through flow-based inversion while maintaining temporal consistency.

### FlowEdit
Inversion-free text-based video editing using pre-trained flow models. Simplified workflow by eliminating explicit inversion.

### Image + Video to Video (I+V2V)
Combined reference images with video-to-video generation -- using a reference image for style/content while preserving motion from input video.

### STG (Spatiotemporal Skip Guidance)
Enhanced video diffusion sampling for improved quality through advanced sampling strategies.

## Migration

Users must switch to the official repository:
- **New location:** https://github.com/Lightricks/ComfyUI-LTXVideo
- Use `LTXVKeyFrameConditioning` instead of legacy interpolation nodes
- Use `LTXVSequenceConditioning` instead of legacy frame setting nodes

## Fork: ComfyUI-LTXTricks-RUNWAY

A fork by m4rio at https://github.com/m4rio/ComfyUI-LTXTricks-RUNWAY adapts the nodes for additional runway-style workflows.

## Significance

This project exemplifies a successful community-to-official pipeline. An independent developer created advanced nodes that proved valuable enough for Lightricks to adopt and merge into their official ComfyUI integration, demonstrating healthy open-source collaboration.

## Related Pages

- [[comfyui-ltxvideo-official-nodes]] -- Official extension (migration target)
- [[comfyui-ltx-community-nodes]] -- Other community node packs
- [[comfyui-ltx-node-reference]] -- Current node reference
