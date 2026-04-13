# Community Project: IC-LoRA Control Models

**Source:** https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Union-Control, https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control, https://huggingface.co/Lightricks/LTX-Video-ICLoRA-detailer-13b-0.9.8

---

## What is IC-LoRA?

In-Context LoRA (IC-LoRA) is a technique developed for LTX Video that enables:
- Video context injection into the generation process
- Fine-grained video-to-video control on text-to-video models
- Precise content control by conditioning on reference video frames during inference
- Reference downscale factor for efficiency (smaller references, same control quality)

**Paper:** "AVControl: Efficient Framework for Training Audio-Visual Controls" (arXiv: 2603.24793)
**Project Page:** https://matanby.github.io/AVControl/

## Available IC-LoRA Control Models

### 1. Union Control (LTX-2.3)
- **Model:** LTX-2.3-22b-IC-LoRA-Union-Control
- **Likes:** 43
- **Control Signals:** Canny edge detection + Depth maps + Pose information
- **Reference Downscale:** 2x (0.5x output resolution)
- **Used in:** 21+ HuggingFace Spaces

Combines multiple control signals in a single LoRA, allowing users to guide video generation using edge maps, depth information, and human pose simultaneously.

### 2. Motion Track Control (LTX-2.3)
- **Model:** LTX-2.3-22b-IC-LoRA-Motion-Track-Control
- **Likes:** 32
- **Control Method:** Sparse point trajectories via colored spline overlays
- **Reference Downscale:** 2x

Users provide a reference video with colored spline overlays indicating desired motion paths. Tracks can be:
- Extracted from existing videos using SpatialTrackerV2
- Drawn manually
- Defined programmatically

### 3. Detailer (LTX-Video 0.9.8)
- **Model:** LTX-Video-ICLoRA-detailer-13b-0.9.8
- **Downloads:** 29,812/month
- **Likes:** 29
- **Purpose:** Enhance fine details and textures in generated videos

### 4. Detailer (LTX-2)
- **Model:** LTX-2-19b-IC-LoRA-Detailer
- **Likes:** 62
- **Used in:** 29+ Spaces
- **Purpose:** Video detail refinement for LTX-2 outputs

### 5. Canny Control (LTX-Video 0.9.7)
- **Model:** LTX-Video-ICLoRA-canny-13b-0.9.7
- **Downloads:** 405/month
- **Purpose:** Edge-based video generation control

### 6. Depth Control (LTX-Video 0.9.7)
- **Model:** LTX-Video-ICLoRA-depth-13b-0.9.7
- **Downloads:** 362/month
- **Purpose:** Depth-map-based video generation control

## ComfyUI Integration

All IC-LoRA models integrate with ComfyUI through:
1. `LTXICLoRALoaderModelOnly` node - Loads the LoRA and extracts downscale factor
2. `LTXAddVideoICLoRAGuide` node - Adds reference video as guide
3. Official example workflows provided in the ComfyUI-LTXVideo repository

## Significance

IC-LoRA represents a major innovation in controllable video generation:
- Enables ControlNet-like functionality without requiring separate ControlNet models
- Union control combines multiple modalities in a single lightweight adapter
- Motion tracking enables precise object trajectory specification
- The detailer pipeline enables a two-pass workflow: generate, then refine
- All controls work with LTX-2/2.3's audio-video generation capability
