# Community Project: Camera Control LoRAs

**Source:** https://huggingface.co/spaces/prithivMLmods/LTX-2-LoRAs-Camera-Control-Dolly, https://huggingface.co/Lightricks/LTX-2

---

## Overview

Multiple community members and Lightricks have created camera control LoRAs for LTX-2/2.3, enabling precise camera movement control in generated videos.

## prithivMLmods Camera Control Space

- **Space:** LTX-2-LoRAs-Camera-Control-Dolly
- **Creator:** prithivMLmods
- **Likes:** 58
- **Status:** Running on Zero
- **Feature:** Camera Control Dolly effect using distilled model

## Official Camera Control Support

The LTX-2 ecosystem supports several camera movement types via LoRAs:
- **Dolly** - Camera moves toward or away from subject
- **Jib** - Vertical camera movement (crane shot)
- **Static** - Locked camera position

## Motion Track Control (LTX-2.3)

### LTX-2.3-22b-IC-LoRA-Motion-Track-Control
- **Creator:** Lightricks (official)
- **Purpose:** Guide motion of objects using sparse point trajectories
- **Method:** Users provide reference video with colored spline overlays indicating desired motion paths
- **Input:** Tracks can be extracted from existing videos using point tracking (e.g., SpatialTrackerV2) or drawn manually
- **Reference downscale factor:** 2 (reference at 0.5x output resolution)
- Detailed ComfyUI workflow provided

## Union Control (LTX-2.3)

### LTX-2.3-22b-IC-LoRA-Union-Control
- **Creator:** Lightricks (official)
- **Likes:** 43
- **Purpose:** Multiple control signals in a single LoRA
- **Control types:** Canny edge detection + Depth maps + Pose information
- **Method:** IC-LoRA (In-Context LoRA) conditioning on reference frames
- **Reference downscale factor:** 2

## Community LoRA Ecosystem

### Community-Created LoRAs
| Creator | LoRA Name | Focus |
|---------|-----------|-------|
| Burgstall | amgery-lora | Video expressions (15 likes) |
| Burgstall | nhl-smile-lora | Smile effects |
| Burgstall | sorpresa-lora | Surprise expressions |
| Burgstall | headbanger-lora | Head movement |
| eisneim | bullet_time | Bullet time effect |
| svjack | anime_landscape_early_lora | Anime landscapes |
| svjack | pixel_early_lora | Pixel art style |
| Pierre-Jean | trajectory-lora | Camera trajectory |
| Sameric934 | ltx2-video-lora | General video |

## Training Support

Lightricks provides official LoRA training support:
- **Repository:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Motion, style, or likeness (sound+appearance) LoRAs can train in under an hour
- Full documentation provided in the LTX-2 monorepo
