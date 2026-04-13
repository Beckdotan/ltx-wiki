# LTX Video Cloud Platform Integrations

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3

---

## Official Cloud Deployments

### LTX Studio
- **URL:** https://app.ltx.studio/
- Lightricks' own hosted platform for LTX Video generation
- Offers both 13B and 13B distilled models for LTX-Video
- LTX-2 playground available at https://app.ltx.studio/ltx-2-playground/
- Separate text-to-video and image-to-video interfaces

### Console / API Playground
- **URL:** https://console.ltx.video/playground/
- API access to LTX-2.3 and other models
- Official documentation at https://docs.ltx.video/

### fal.ai
- Listed as an official deployment partner
- LTX Video models available via fal.ai API
- **URL:** https://fal.ai/

### Replicate
- Listed as an official deployment partner
- LTX Video available at https://replicate.com/lightricks/ltx-video
- API-based access for integration into applications

## HuggingFace Spaces (Zero GPU)

Multiple community-created Spaces running on HuggingFace's free Zero GPU tier:

| Space | Creator | Purpose | Likes |
|-------|---------|---------|-------|
| LTX-2 Video [Turbo] | alexnasa | Fast generation with FA3 | 398 |
| LTX 2.3 Distilled | Lightricks (official) | Cinematic video + audio | 299 |
| LTX 2.3 Sync | linoyts | Portrait animation/lipsync | 120 |
| LTX 2.3 First-Last Frame | linoyts | Frame-controlled generation | 96 |
| Camera Control Dolly | prithivMLmods | Camera motion control | 58 |
| Audio To Video | multimodalart | Lip-sync from audio | 49 |
| LTX-2.3 Video [Turbo] | ZeroCollabs | Free ZeroGPU generation | 40 |

## Edge/Embedded Deployment

### NVIDIA Jetson AGX Thor
- Community member (Divhanthegray) successfully ran LTX-2 19B on Jetson Thor
- 128GB unified memory required
- Repository: github.com/divhanthelion/ltx2
- Performance: ~15 min diffusion + 2 min VAE decode per clip
- Output: 1920x1088, 161 frames, 24fps with synchronized audio
- Significant memory management challenges on unified memory architecture
