# IC-LoRA: In-Context LoRA Control Framework for LTX Models

## Source
- HuggingFace model cards for IC-LoRA models
- AVControl paper (arXiv:2603.24793)
- LTX-2 HuggingFace documentation

## What is IC-LoRA?

IC-LoRA (In-Context LoRA) is a fine-tuning technique developed by Lightricks that enables conditioning video generation on reference video frames at inference time. It allows fine-grained video-to-video control on top of the LTX text-to-video base models without requiring full model retraining.

## Core Mechanism

### Parallel Canvas Conditioning
- Reference signal provided as **additional tokens in self-attention layers**
- NOT spatial concatenation (which fails for structural controls in video)
- LTX-2's per-token timestep mechanism distinguishes reference (clean) from generation (noisy) tokens
- Reference influence can be continuously modulated at inference time

### Reference Downscale Factor
- Reference video can be smaller than output resolution to save tokens
- Example: ref0.5 means reference at 0.5x output resolution (downscale factor 2)
- Reduces computational cost for control modalities that don't need full resolution

### LoRA Training
- Each control modality: separate lightweight LoRA adapter
- Trained on frozen LTX-2 backbone
- No architectural changes required
- Default rank: 128
- Convergence: 1,000-3,000 steps for spatial controls

## Available IC-LoRA Models

### LTX-2.3 (22B base)
| Model | Control Type | Description |
|-------|-------------|-------------|
| LTX-2.3-22b-IC-LoRA-Union-Control | Depth + Pose + Canny | Unified structural control |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | Motion tracking | Sparse point tracking control |

### LTX-2 (19B base)
| Model | Control Type | Description |
|-------|-------------|-------------|
| LTX-2-19b-IC-LoRA-Union-Control | Depth + Pose + Canny | Unified structural control |
| LTX-2-19b-IC-LoRA-Detailer | Video refinement | Fine detail and texture enhancement |
| LTX-2-19b-IC-LoRA-Canny-Control | Canny edges | Edge-guided generation |

### LTX-Video v0.9.8 (13B base)
| Model | Control Type | Description |
|-------|-------------|-------------|
| ICLoRA-Depth | Depth maps | Depth-guided generation |
| ICLoRA-Pose | Pose estimation | Pose-guided generation |
| ICLoRA-Canny | Canny edges | Edge-guided generation |

## Full Modality List (from AVControl paper)

13 independently trained modalities:

**Spatial Controls:**
1. Depth-guided generation
2. Pose-guided generation
3. Canny edge control

**Camera Control:**
4. Camera trajectory from image
5. Camera trajectory from video

**Motion Control:**
6. Sparse point tracking

**Video Editing:**
7. Cut-on-action
8. Inpainting
9. Outpainting

**Audio-Visual Controls:**
10. Who-is-talking (speaker-aware)
11. Audio intensity to waveform
12-13. Additional audio-visual modalities

## Implementation

### ComfyUI Integration
1. Copy LoRA weights to `models/loras/`
2. Use official IC-LoRA workflow from LTX-2 ComfyUI repository
3. Key nodes:
   - `LTXICLoRALoaderModelOnly`: Loads LoRA and extracts downscale factor
   - `LTXAddVideoICLoRAGuide`: Adds reference latent as guide

### Diffusers Integration
```python
from diffusers import LTXConditionPipeline
# Load base model
pipe = LTXConditionPipeline.from_pretrained("Lightricks/LTX-Video-0.9.8-dev")
# Apply LoRA for control
```

## Key Papers
- **LTX-2:** arXiv:2601.03233 -- Foundation model
- **AVControl:** arXiv:2603.24793 -- Control framework methodology
- **LTX-Video Community Trainer** (GitHub) -- Training tooling by Matan Ben Yosef, Naomi Ken Korem, Tavi Halperin

## Training Data
- Canny-Control-Dataset (released on HuggingFace)
- Additional proprietary datasets for specific modalities
- Some modalities trained on synthetic/generated data (generalize to real-world)

## Performance
- VACE Benchmark: outperforms all baselines on depth, pose, inpainting, outpainting
- VBench depth scores: 81.6 with rank-128 LoRA at 3K steps
- Total training budget: ~55K steps across all 13 modalities
