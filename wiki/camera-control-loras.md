---
title: Camera Control LoRAs
type: community-project
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-camera-control-loras.md
  - raw/community-project-ic-lora-controls.md
tags:
  - community
  - lora
  - camera-control
  - ic-lora
  - ltx-2
  - ltx-2.3
---

# Camera Control LoRAs

Multiple community members and [[lightricks-company]] have created camera control LoRAs for [[ltx-2-overview|LTX-2]] and [[ltx-2.3-model|LTX-2.3]], enabling precise camera movement control in generated videos.

## prithivMLmods Camera Control Space

- **Space:** LTX-2-LoRAs-Camera-Control-Dolly
- **Creator:** prithivMLmods
- **Likes:** 58
- **Runtime:** HuggingFace Zero
- **Feature:** Camera Control Dolly effect using the distilled model

## Supported Camera Movements

The LTX-2 ecosystem supports several camera movement types via LoRAs:

- **Dolly** -- Camera moves toward or away from subject
- **Jib** -- Vertical camera movement (crane shot)
- **Static** -- Locked camera position

These camera control LoRAs are also used in the [[jetson-thor-deployment|Jetson Thor edge deployment pipeline]].

## Motion Track Control (LTX-2.3)

The official **LTX-2.3-22b-IC-LoRA-Motion-Track-Control** enables guiding the motion of objects using sparse point trajectories:

- Users provide a reference video with colored spline overlays indicating desired motion paths
- Tracks can be extracted from existing videos using SpatialTrackerV2, drawn manually, or defined programmatically
- Reference downscale factor: 2 (reference at 0.5x output resolution)
- Detailed [[comfyui-ltx-integration-overview|ComfyUI]] workflow provided

See [[ic-lora]] for technical details on the underlying In-Context LoRA methodology.

## Union Control (LTX-2.3)

The official **LTX-2.3-22b-IC-LoRA-Union-Control** combines multiple control signals in a single LoRA:

- **Control types:** Canny edge detection + Depth maps + Pose information
- **Likes:** 43
- **Used in:** 21+ HuggingFace Spaces
- Reference downscale factor: 2

This enables users to guide video generation using edge maps, depth information, and human pose simultaneously. See [[ic-lora]] for details.

## Community-Created Camera and Motion LoRAs

| Creator | LoRA | Focus |
|---------|------|-------|
| Pierre-Jean | trajectory-lora | Camera trajectory control |
| eisneim | bullet_time | Bullet time camera effect |
| Burgstall | headbanger-lora | Head movement animation |

See [[lora-ecosystem]] for the complete inventory of community and official LoRAs.

## See Also

- [[ic-lora]] -- In-Context LoRA technology powering advanced controls
- [[lora-ecosystem]] -- Full LoRA ecosystem including training tools
- [[jetson-thor-deployment]] -- Edge deployment using camera control LoRAs
- [[ltx-2-overview]] -- LTX-2 base model
