# LTX-Video Capabilities: Resolution, FPS, Frame Count, and Generation Modes

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://arxiv.org/abs/2501.00103

---

## Output Specifications

### Resolution
- **Native highlight resolution:** 1216x704 at 30 FPS
- **Paper benchmark resolution:** 768x512 at 24 FPS
- **Works best under:** 720x1280 (HD)
- **Resolution constraint:** Must be divisible by 32 (due to VAE spatial compression ratio of 32)
- **Auto-padding:** Non-compliant inputs padded with -1, then cropped to desired size

### Common Resolutions Used in Examples
- 768x512 (paper benchmark)
- 704x480
- 832x480
- 704x1216 (portrait)
- 1216x704 (landscape)

### Frame Count
- **Maximum recommended:** 257 frames
- **Constraint:** Must be (divisible by 8) + 1
- **Valid frame counts:** 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 105, 113, 121, 129, 137, 145, 153, 161, ..., 257
- **Common choices in examples:** 96, 121, 161, 257

### Frame Rate
- **Native output:** 24-30 FPS
- **Paper default:** 24 FPS
- **Export default:** 24 FPS (in code examples)
- **RoPE temporal embedding** incorporates FPS for natural motion

### Duration
- Up to ~10 seconds (257 frames at 24 FPS ~= 10.7 seconds)
- Paper focused on 5-second generation

## Generation Modes

### 1. Text-to-Video (T2V)
- Generate video entirely from a text prompt
- Supports negative prompts for quality improvement
- Best results with elaborate, detailed English prompts
- Example: detailed scene descriptions with lighting, camera angles, subject descriptions

### 2. Image-to-Video (I2V)
- Animate a still image into a video sequence
- Uses timestep-based conditioning (no extra parameters needed)
- Conditioning image encoded as first frame by causal VAE
- Can specify conditioning strength

### 3. Video-to-Video (V2V)
- Transform/extend existing video based on new prompt
- Uses denoise_strength parameter to control how much of original video to preserve

### 4. Multi-Condition Generation (v0.9.7+)
- Condition on multiple images and/or videos at specified frame positions
- Each condition has a frame_index specifying where in the output it appears
- Adjustable conditioning_strength per condition (default: 1.0)
- Example: first frame from Image A, frame 40 from Image B

### 5. Controlled Generation via ICLoRA (v0.9.7+)
- **Depth conditioning:** Generate video following a depth map sequence
- **Pose conditioning:** Generate video following pose keypoints
- **Canny edge conditioning:** Generate video following edge maps
- **Detailer:** Enhanced fine detail generation

### 6. Upscaling (v0.9.7+)
- **Spatial upscaling:** 2x resolution increase in latent space
- **Temporal upscaling:** Frame interpolation for smoother video
- Both operate on latent representations for efficiency

## Generation Speed

### Paper Benchmark (2B model, H100 GPU)
- 5 seconds of 24 FPS video at 768x512
- Generated in 2 seconds
- 20 diffusion steps
- Faster-than-real-time

### Model-Specific Speed Estimates
| Model | Steps | Speed Characteristic |
|-------|-------|---------------------|
| 2B dev | 20-50 | Faster-than-real-time |
| 2B distilled | 7-10 | 15x faster than dev, real-time |
| 13B dev | 20-30 | Slower, highest quality |
| 13B distilled | 7 | Faster, good quality |
| 13B mix | varies | Balanced speed-quality |

## Prompt Guidelines

- **Language:** English only
- **Style:** Detailed, elaborate descriptions yield best results
- **Structure:** Describe subject, action, environment, lighting, camera angle
- **Prompt elaboration:** More detailed prompts produce better adherence
- **Example of good prompt:**
  > "The turquoise waves crash against the dark, jagged rocks of the shore, sending white foam spraying into the air. The scene is dominated by the stark contrast between the bright blue water and the dark, almost black rocks. The water is a clear turquoise color; the rocks are dark, with patches of green moss. The shore is lined with lush green vegetation, with rolling hills covered in dense forest. The sky is overcast with soft, diffused light."
