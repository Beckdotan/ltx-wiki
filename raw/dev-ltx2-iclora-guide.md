# LTX-2 IC-LoRA (In-Context LoRA) Guide

**Source:** https://docs.ltx.video/open-source-model/usage-guides/ic-lo-ra
**Fetched:** 2026-04-13

---

## What is IC-LoRA?

IC-LoRA (In-Context LoRA) enables precise video-to-video control in LTX-2 by conditioning generation on reference signals like depth maps, pose skeletons, or edge detections. Unlike standard LoRAs that modify visual style, IC-LoRA adapters dictate spatial structure and motion with frame-level precision.

---

## Applications

- Animating depth sequences into realistic video
- Retargeting character movement via pose data
- Generating videos following edge-based compositions
- Controlling object motion via trajectory splines

---

## Supported Control Types

### Union Control (Single Model, Multiple Types)
One model handles three conditioning types:
- **Depth mapping** - 3D spatial structure
- **Canny edge detection** - Structural outlines
- **Pose skeleton guidance** - Character articulation

### Motion Control
- **Spline-based trajectory rendering**
- Single or multiple simultaneous motion paths

---

## Available Models

| Model | Purpose |
|-------|---------|
| `LTX-2.3-20b-IC-LoRA-Union-Control` | Multi-type control (depth, canny, pose) |
| `LTX-2.3-20b-IC-LoRA-Motion-Track-Control` | Object motion guidance via trajectories |

---

## Configuration Parameters

### Attention Strength
Global scalar controlling IC-LoRA influence (0.0-1.0):
- **1.0** - Full adherence to control signal (default)
- **0.5** - Balanced blending between control and free generation
- **0.0** - Completely disables the signal

### Attention Mask
Optional spatial or spatiotemporal mask enabling region-level control:
- Values range 0.0 to 1.0
- Supports gradual fade-ins
- Supports foreground-only control
- Supports multi-region application

### Resolution & Frame Rate
- Match control signal resolution to generation resolution
- Control FPS should align with target generation FPS
- Recommended: 704x1216 at 24-30 FPS

---

## ComfyUI Implementation

### Setup
1. Install ComfyUI-LTXVideo custom nodes
2. Download `.safetensors` checkpoint from Hugging Face
3. Place in `ComfyUI/models/loras/` directory
4. Load IC-LoRA workflow from repository

### Key Nodes
- **LTX IC-LoRA Loader Model Only** - Load IC-LoRA adapter
- **LTX Add Video IC-LoRA Guide Advanced** - Configure strength and mask
- **LTX Draw Tracks** - Create motion trajectories
- **LTX Sparse Track Editor** - Keypoint interpolation
- **LTX Sampler** - Generate video

---

## Preparing Control Signals

### Depth Maps
- **Tools:** Depthcrafter, Blender, MiDaS
- Ensure consistent depth range
- Smooth temporal transitions between frames

### Pose Skeletons
- **Tools:** OpenPose, MediaPipe Pose, DWPose
- Extract at consistent frame rates
- 17-18 keypoints as skeleton visualization
- Reliable keypoint detection required

### Canny Edges
- **Tools:** OpenCV, PIL, ComfyUI preprocessor nodes
- Adjust thresholds to capture essential edges without noise
- Maintain consistent edge thickness across frames

### Sparse Tracks (Motion)
- Define motion via keypoints at specific frames
- System interpolates smooth spline trajectories rendered as circles
- 3-4 keypoints per track typically sufficient

---

## Best Practices

### Control Signal Quality
- Use high-quality extraction tools
- Ensure temporal consistency across frames
- Remove noise and compression artifacts
- Match resolution to generation target

### Prompt Alignment
- Describe visual style, not control type
  - Good: "ornate architecture with golden details"
  - Bad: "depth map shows building outline"
- Align prompts with control motion and composition
- Specify materials, lighting, and atmosphere

### Performance
- IC-LoRAs add minimal overhead (less than 10% compute)
- Compatible with FP8 quantized and distilled models
- Use Video Detailer IC-LoRA for upscaling efficiency

---

## Troubleshooting

| Issue | Solutions |
|-------|----------|
| Control ignored | Verify IC-LoRA loaded correctly; check attention_strength; validate control format/contrast |
| Temporal flickering | Smooth transitions; increase inference steps (40-50); match FPS; apply temporal filtering |
| Poor quality | Improve extraction tools; ensure adequate resolution; refine prompts; check for artifacts |
| Control too rigid | Reduce attention_strength to 0.5-0.8; apply spatial masks; use flexible prompts |
| Memory errors | Use FP8 quantization; reduce resolution; process in smaller batches |

---

## LoRA Strength Guide (Standard LoRA)

For standard (non-IC) LoRAs:
- **0.9-1.1:** Subtle effect, preserves base model characteristics
- **1.2-1.4:** Balanced approach (most common)
- **1.5-1.6:** Maximum style influence
- Keep combined LoRA strength below 2.0
- Effect LoRAs combine better than control LoRAs
- Avoid mixing multiple IC-LoRA control types
- LoRAs add minimal compute overhead (less than 5% typically)

### Available Camera LoRAs
Seven camera control LoRAs are provided:
- Dolly In / Dolly Out
- Dolly Left / Dolly Right
- Jib Up / Jib Down
- Static Camera

---

## Related Links

- **IC-LoRA Guide:** https://docs.ltx.video/open-source-model/usage-guides/ic-lo-ra
- **LoRA Guide:** https://docs.ltx.video/open-source-model/usage-guides/lo-ra
- **LTX-2 Trainer:** https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
- **HuggingFace IC-LoRA Models:** https://huggingface.co/Lightricks
