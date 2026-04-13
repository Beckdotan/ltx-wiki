# Graph Report - ./raw  (2026-04-13)

## Corpus Check
- Large corpus: 239 files · ~151,263 words. Semantic extraction will be expensive (many Claude tokens). Consider running on a subfolder, or use --no-semantic to run AST-only.

## Summary
- 772 nodes · 1038 edges · 54 communities detected
- Extraction: 96% EXTRACTED · 4% INFERRED · 0% AMBIGUOUS · INFERRED: 38 edges (avg confidence: 0.8)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_LTX-Video Core & Ecosystem|LTX-Video Core & Ecosystem]]
- [[_COMMUNITY_Model Variants & Hosting|Model Variants & Hosting]]
- [[_COMMUNITY_API & Competitive Landscape|API & Competitive Landscape]]
- [[_COMMUNITY_Research & AVControl|Research & AVControl]]
- [[_COMMUNITY_ComfyUI Nodes & Workflows|ComfyUI Nodes & Workflows]]
- [[_COMMUNITY_Architecture & NVIDIA|Architecture & NVIDIA]]
- [[_COMMUNITY_Community Tools & GGUF|Community Tools & GGUF]]
- [[_COMMUNITY_API Pricing & ComfyUI Integration|API Pricing & ComfyUI Integration]]
- [[_COMMUNITY_Product Competitors & MCP|Product Competitors & MCP]]
- [[_COMMUNITY_Diffusers & Python SDK|Diffusers & Python SDK]]
- [[_COMMUNITY_VAE Architecture & Training|VAE Architecture & Training]]
- [[_COMMUNITY_Desktop App & Licensing|Desktop App & Licensing]]
- [[_COMMUNITY_LTX-2 Monorepo & Pipelines|LTX-2 Monorepo & Pipelines]]
- [[_COMMUNITY_Distillation & Quantization|Distillation & Quantization]]
- [[_COMMUNITY_VAE Innovation|VAE Innovation]]
- [[_COMMUNITY_GGUF Low-VRAM|GGUF Low-VRAM]]
- [[_COMMUNITY_Audio-to-Video|Audio-to-Video]]
- [[_COMMUNITY_Acceleration Extensions|Acceleration Extensions]]
- [[_COMMUNITY_Core Package|Core Package]]
- [[_COMMUNITY_IC-LoRA Pipeline|IC-LoRA Pipeline]]
- [[_COMMUNITY_Two-Stage Pipeline|Two-Stage Pipeline]]
- [[_COMMUNITY_ComfyUI Wiki Tutorial|ComfyUI Wiki Tutorial]]
- [[_COMMUNITY_LTX Blog Guide|LTX Blog Guide]]
- [[_COMMUNITY_WaveSpeed Tutorial|WaveSpeed Tutorial]]
- [[_COMMUNITY_Tiled VAE|Tiled VAE]]
- [[_COMMUNITY_Temporal Upscaler|Temporal Upscaler]]
- [[_COMMUNITY_Download Stats|Download Stats]]
- [[_COMMUNITY_Style LoRAs|Style LoRAs]]
- [[_COMMUNITY_Billing Model|Billing Model]]
- [[_COMMUNITY_v0.9.8 Release|v0.9.8 Release]]
- [[_COMMUNITY_Image Animation|Image Animation]]
- [[_COMMUNITY_Martini Desktop|Martini Desktop]]
- [[_COMMUNITY_A1111 Legacy|A1111 Legacy]]
- [[_COMMUNITY_HunyuanVideo|HunyuanVideo]]
- [[_COMMUNITY_Stable Diffusion 3|Stable Diffusion 3]]
- [[_COMMUNITY_FLUX.1|FLUX.1]]
- [[_COMMUNITY_BridgeDiT|BridgeDiT]]
- [[_COMMUNITY_ThinkDiffusion|ThinkDiffusion]]
- [[_COMMUNITY_Distilled Pipeline|Distilled Pipeline]]
- [[_COMMUNITY_Wan 2.5|Wan 2.5]]
- [[_COMMUNITY_fal.ai Endpoints|fal.ai Endpoints]]
- [[_COMMUNITY_v0.9.8|v0.9.8]]
- [[_COMMUNITY_MaxVideoAI|MaxVideoAI]]
- [[_COMMUNITY_VHS Extension|VHS Extension]]
- [[_COMMUNITY_Studio Reviews|Studio Reviews]]
- [[_COMMUNITY_FLUX.1 Alt|FLUX.1 Alt]]
- [[_COMMUNITY_Documentation Hub|Documentation Hub]]
- [[_COMMUNITY_RunPod|RunPod]]
- [[_COMMUNITY_ModelScope|ModelScope]]
- [[_COMMUNITY_Latte|Latte]]
- [[_COMMUNITY_Analytics Agents|Analytics Agents]]
- [[_COMMUNITY_LTX-2.3 HF Card|LTX-2.3 HF Card]]
- [[_COMMUNITY_NVIDIA A100|NVIDIA A100]]
- [[_COMMUNITY_MCP Video Tool|MCP Video Tool]]

## God Nodes (most connected - your core abstractions)
1. `LTX-Video` - 121 edges
2. `LTX-2 (19B)` - 86 edges
3. `Lightricks` - 55 edges
4. `LTX-2.3 (22B)` - 48 edges
5. `LTX-Video` - 28 edges
6. `LTX-2 Model (19B Parameters)` - 25 edges
7. `ComfyUI` - 25 edges
8. `IC-LoRA (In-Context LoRA)` - 22 edges
9. `LoRA Training` - 21 edges
10. `LTX Studio` - 21 edges

## Surprising Connections (you probably didn't know these)
- `CAFA: Controllable Automatic Foley Artist (arXiv 2504.06778)` --conceptually_related_to--> `LTX-2 Model (19B Parameters)`  [INFERRED]
  raw/papers-citing-ltx-video.md → raw/github-ltx-2-official.md
- `Portrait Animation & Lip-Sync Use Case` --references--> `LTX-2 Model (19B Parameters)`  [INFERRED]
  raw/use-cases-commercial-creative-research.md → raw/github-ltx-2-official.md
- `Gemma 3 (12B) Text Encoder` --conceptually_related_to--> `T5-XXL Text Encoder (LTX-Video)`  [INFERRED]
  raw/github-ltx-2-official.md → raw/github-ltx-video-official.md
- `Community Consensus: LTX Speed vs Quality Trade-off` --references--> `LTX-Video Model (Original, 2B Parameters)`  [EXTRACTED]
  raw/social-reddit-stablediffusion-discussions.md → raw/github-ltx-video-official.md
- `Community Consensus: LTX Speed vs Quality Trade-off` --semantically_similar_to--> `LTX-2.3 Speed Niche: 18x Faster Than Wan 2.2 on H100`  [INFERRED] [semantically similar]
  raw/social-reddit-stablediffusion-discussions.md → raw/social-reddit-model-comparisons.md

## Hyperedges (group relationships)
- **LTX Model Evolution Lineage (v0.9 -> 13B -> LTX-2 -> LTX-2.3)** — comfyui-ltx-version-history_v09, comfyui-ltx-version-history_v097_13b, github-ltx-2-official_ltx2_model, github-ltx-2-official_ltx23_model [EXTRACTED 1.00]
- **IC-LoRA Control Ecosystem (Technique + AVControl + ComfyUI Nodes)** — ic-lora-control-framework_ic_lora_technique, papers-citing-ltx-video_avcontrol, comfyui-ltx-node-reference_iclora_union_control, ic-lora-control-framework_13_modalities [EXTRACTED 1.00]
- **LTX Cloud Deployment Ecosystem (Studio + fal.ai + Replicate + HF Spaces + RunPod)** — integration-cloud-platforms_ltx_studio, integration-cloud-platforms_fal_ai, integration-cloud-platforms_replicate, integration-cloud-platforms_hf_spaces, hosting-runpod-runcomfy_runpod [EXTRACTED 0.90]
- **** — sdk-diffusers-integration_ltx_video, community-project-jetson-thor-deployment_ltx2, sdk-diffusers-integration_ltx23 [EXTRACTED 1.00]
- **** — api-inference-providers-overview_fal_ai, api-inference-providers-overview_replicate, api-inference-providers-overview_huggingface, inference-providers-overview_wavespeed_ai, ltx-api_segmind, ltx-api_modal [EXTRACTED 1.00]
- **** — ltx2-lora-training-and-iclora_standard_lora, ltx2-lora-training-and-iclora_av_lora, ltx2-lora-training-and-iclora_iclora_mode [EXTRACTED 1.00]
- **** — ltx_2, ltx_2_3, wan_2_2, sora_2_pro, veo_3, runway_gen_4_5, ovi, wan_2_5 [EXTRACTED 1.00]
- **** — comfyui_ltxvideo, diffusers, ltx_rest_api, fal_ai [EXTRACTED 1.00]
- **** — nvidia, lightricks, comfyui, blender, rtx_video_super_resolution [EXTRACTED 1.00]
- **** — ltx_2_3, hunyuan_video_1_5, wan_2_2 [INFERRED 0.85]
- **** — fal_ai, replicate, modal, segmind, digitalocean, vast_ai, runcomfy [EXTRACTED 1.00]
- **** — video_vae, audio_vae, dit_architecture, gemma_3_12b, rgan_innovation, video_dwt_loss [EXTRACTED 0.95]
- **** — ltx_video, video_vae, video_dit, denoising_decoder, rope_positional_encoding [EXTRACTED 1.00]
- **** — ltx_2_3, gemma_3_12b, audio_vae, video_vae, multimodal_guider_node, normalizing_sampler [EXTRACTED 1.00]
- **** — lora_training, wavespeed_ai, fal_ai, oxen_ai, ltx_video_trainer, ltx_trainer_package [EXTRACTED 1.00]
- **** — ltx_2_3, video_vae, gated_attention_text_connector, hifi_gan_vocoder, gemma_3_12b [EXTRACTED 1.00]
- **** — ltx_video, ltxv_097, ltxv_13b, spatial_upscaler, distilled_model, fp8_quantization [EXTRACTED 1.00]
- **** — ic_lora, style_lora, motion_lora, camera_control_lora, id_lora, union_ic_lora [EXTRACTED 1.00]
- **** — ltx_video, distillation_technique, fp8_quantization, gguf_quantization, nvfp4_quantization [EXTRACTED 1.00]
- **** — ltx_2, asymmetric_dual_stream_transformer, causal_audio_vae, modality_aware_cfg, thinking_tokens, gemma3_12b [EXTRACTED 1.00]
- **** — ltx_2_3, cogvideox, wan_2_2, veo_3, sora_2, ovi [EXTRACTED 1.00]
- **** — ltx_video, video_vae, dit_architecture, t5_xxl, flow_match_euler_scheduler [EXTRACTED 1.00]
- **** — ltx_video, comfyui, huggingface_diffusers, fal_ai, replicate, ltx_desktop [EXTRACTED 1.00]
- **** — ltx_video_090, ltx_video_091, ltx_video_095, ltx_video_096, ltx_video_097, ltx_video_098, ltx_2, ltx_23 [EXTRACTED 1.00]
- **** — video_vae, patchification_relocation, denoising_decoder, reconstruction_gan, video_dwt_loss [EXTRACTED 1.00]
- **** — ltx_2_github_repo, ltx_core_package, ltx_pipelines_package, ltx_trainer_package [EXTRACTED 1.00]
- **** — ltx_studio, veo_3_1, kling_model, flux_image_model [EXTRACTED 1.00]
- **** — video_vae, patchifier_relocation, denoising_decoder, compression_ratio_1_192 [EXTRACTED 1.00]
- **** — asymmetric_dual_stream_dit, gemma_3_12b, bimodal_cfg, audio_vae, audio_video_sync [EXTRACTED 1.00]
- **** — ltx_desktop, davinci_resolve, premiere_pro, final_cut_pro, ltx_2_3 [EXTRACTED 1.00]
- **** — ltx_video, video_vae, diffusion_transformer, denoising_vae_decoder, rectified_flow, t5_xxl [EXTRACTED 1.00]
- **** — ltx_2, dual_stream_dit, bidirectional_cross_attention, audio_vae, gemma_3_12b, thinking_tokens, modality_aware_cfg [EXTRACTED 1.00]
- **** — avcontrol, parallel_canvas, just_dub_it, id_lora, ltx_2 [EXTRACTED 1.00]

## Communities

### Community 0 - "LTX-Video Core & Ecosystem"
Cohesion: 0.02
Nodes (112): Adaptive Layer Normalization (AdaLN), LTX Video Adoption Metrics, alexnasa (Community Creator), AnimateDiff, Apache 2.0 License, Apple Silicon, LTX-Video Paper (arXiv:2501.00103), LTX-Video Benchmark Results (+104 more)

### Community 1 - "Model Variants & Hosting"
Cohesion: 0.02
Nodes (99): fal.ai Platform, fal.ai LTX LoRA Training, ltxv-13b-0.9.8-dev, ltxv-2b-0.9.8-distilled, LTX-Video HuggingFace Model Card, fal.ai, Hugging Face, LTX Studio (+91 more)

### Community 2 - "API & Competitive Landscape"
Cohesion: 0.03
Nodes (87): Alibaba Tongyi Lab, Audio-to-Video API Endpoint, Extend API Endpoint, Image-to-Video API Endpoint, Retake API Endpoint, Text-to-Video API Endpoint, Artificial Analysis Rankings, Artificial Analysis Leaderboard (+79 more)

### Community 3 - "Research & AVControl"
Cohesion: 0.03
Nodes (83): ai-toolkit (Ostris), Amit Dahan, Anthony Chen, AVControl Framework, AVControl Paper (arXiv:2603.24793), Lightricks x Banodoco ADOS Hackathon, Benny Brazowski, CAFA (Controllable Automatic Foley Artist) (+75 more)

### Community 4 - "ComfyUI Nodes & Workflows"
Cohesion: 0.04
Nodes (57): Camera Control LoRA Nodes (Dolly, Jib), GemmaAPITextEncode Node, IC-LoRA Union Control Node (Canny + Depth + Pose), LTXVLatentUpsampler Node, MultimodalGuider Node, LTXVReferenceAudio Node (ID-LoRA, PR #13111), ComfyUI-Box LTX Complete Guide (LTX-2.3), Audio-Synced Video Workflow (MultimodalGuider) (+49 more)

### Community 5 - "Architecture & NVIDIA"
Cohesion: 0.05
Nodes (57): LTX-2 Paper (arXiv:2601.03233), CES 2026, ControlNet, Distilled Model Variants, Distilled Model Variants, FP8 Quantization, GDC 2026, Gemma 3 (12B Text Encoder) (+49 more)

### Community 6 - "Community Tools & GGUF"
Cohesion: 0.05
Nodes (47): awesome-ltx2 (wildminder), Blender, city96 (GGUF Creator), city96/LTX-Video-gguf, Civitai, ComfyUI, ComfyUI-GGUF Custom Node, ComfyUI-GGUF Custom Nodes (city96) (+39 more)

### Community 7 - "API Pricing & ComfyUI Integration"
Cohesion: 0.05
Nodes (46): ltx-2-3-fast API Pricing Tier, ltx-2-3-pro API Pricing Tier, ltx-2-fast API Pricing Tier, ltx-2-pro API Pricing Tier, ComfyUI (Node-Based Workflow Interface), ComfyUI-LTXVideo GitHub Repository, Day-1 Native ComfyUI Support (Design Decision), ComfyUI-LTXTricks (Community Nodes, Now Deprecated) (+38 more)

### Community 8 - "Product Competitors & MCP"
Cohesion: 0.06
Nodes (39): Adobe Firefly, ComfyUI-MCP Server, ComfyUI MCP Server (comfyui-mcp-muse), LTX Creative Showcases, FLUX Image Model, Gemini API, Google AI Studio, Google DeepMind (+31 more)

### Community 9 - "Diffusers & Python SDK"
Cohesion: 0.08
Nodes (31): a-r-r-o-w (Diffusers Converter), HuggingFace Diffusers, HuggingFace Diffusers, FastAPI, FlowMatchEulerDiscreteScheduler, FluxPipeline, Gradio, Hugging Face Diffusers (+23 more)

### Community 10 - "VAE Architecture & Training"
Cohesion: 0.1
Nodes (22): ADAM-W Optimizer, Aesthetic Quality Model (Siamese Network), Aesthetic Model (Siamese Network) Filtering, AutoencoderKLLTXVideo, 1:192 Compression Ratio, Denoising Decoder, Denoising VAE Decoder, Log-Normal Timestep Scheduling (+14 more)

### Community 11 - "Desktop App & Licensing"
Cohesion: 0.1
Nodes (22): ACE-HOLOFS Prompting Technique, AGENTS.md (LTX Desktop), Apache 2.0 License (Code), Apache 2.0 License, ComfyUI-LTXVideo (Custom Nodes), DaVinci Resolve, LTX Discord Server, Electron (Desktop Framework) (+14 more)

### Community 12 - "LTX-2 Monorepo & Pipelines"
Cohesion: 0.19
Nodes (15): A2VidPipelineTwoStage, DistilledPipeline, ICLoraPipeline, ICLoraPipeline, KeyframeInterpolationPipeline, LTX-2 GitHub Repository, ltx-core Package, LTX-2 Monorepo (+7 more)

### Community 13 - "Distillation & Quantization"
Cohesion: 0.29
Nodes (7): LTXVNormalizingSampler Node, DistilledPipeline (8-Step Fastest Inference), FP8 Quantization Support, LTX-Video 0.9.6 Dev Variant (2B), LTX-Video 0.9.6 Distilled Variant (15x Faster), Knowledge Distillation Technique (No STG/CFG Required), NVFP8 Quantization (RTX 30/40/50 Series, 2x Faster)

### Community 14 - "VAE Innovation"
Cohesion: 0.5
Nodes (4): Denoising Decoder (Latent-to-Pixel + Final Denoise), Reconstruction GAN (Novel VAE Loss), VAE Patchification Relocation Innovation, VAE 1:192 Compression Ratio Innovation

### Community 15 - "GGUF Low-VRAM"
Cohesion: 0.67
Nodes (3): ComfyUI-GGUF Extension (Quantized Models), Community GGUF Quantizations (unsloth, vantagewithai), GGUF Quantization for Low VRAM

### Community 16 - "Audio-to-Video"
Cohesion: 1.0
Nodes (2): A2VidPipelineTwoStage (Audio-to-Video), Audio-Reactive Content Creation

### Community 17 - "Acceleration Extensions"
Cohesion: 1.0
Nodes (2): Comfy-WaveSpeed Acceleration Extension, TeaCache Acceleration for LTX-Video

### Community 18 - "Core Package"
Cohesion: 1.0
Nodes (2): ltx-core Package, QuantizationPolicy

### Community 19 - "IC-LoRA Pipeline"
Cohesion: 1.0
Nodes (1): ICLoraPipeline

### Community 20 - "Two-Stage Pipeline"
Cohesion: 1.0
Nodes (1): TI2VidTwoStagesPipeline (Text/Image-to-Video)

### Community 21 - "ComfyUI Wiki Tutorial"
Cohesion: 1.0
Nodes (1): ComfyUI Wiki LTX Workflow Tutorial

### Community 22 - "LTX Blog Guide"
Cohesion: 1.0
Nodes (1): LTX Blog ComfyUI Workflow Guide (LTX-2.3)

### Community 23 - "WaveSpeed Tutorial"
Cohesion: 1.0
Nodes (1): WaveSpeedAI LTX-2 Quickstart Tutorial

### Community 24 - "Tiled VAE"
Cohesion: 1.0
Nodes (1): Tiled VAE Decode Node

### Community 25 - "Temporal Upscaler"
Cohesion: 1.0
Nodes (1): Temporal Upscaler (2x Frame Rate)

### Community 26 - "Download Stats"
Cohesion: 1.0
Nodes (1): LTX-2 Download Statistics (5M+ total, 796K/month peak)

### Community 27 - "Style LoRAs"
Cohesion: 1.0
Nodes (1): Style Transfer & Art (Anime, Pixel Art LoRAs)

### Community 28 - "Billing Model"
Cohesion: 1.0
Nodes (1): Per-Second-of-Output Billing Model

### Community 29 - "v0.9.8 Release"
Cohesion: 1.0
Nodes (1): LTX-Video v0.9.8 (13B, Distilled, Up to 60s Video)

### Community 30 - "Image Animation"
Cohesion: 1.0
Nodes (1): Prompt Structure for Image Animation

### Community 31 - "Martini Desktop"
Cohesion: 1.0
Nodes (1): Martini (Desktop AI Workflow Tool)

### Community 32 - "A1111 Legacy"
Cohesion: 1.0
Nodes (1): Automatic1111 / SD WebUI (Obsolete for Video)

### Community 33 - "HunyuanVideo"
Cohesion: 1.0
Nodes (1): HunyuanVideo

### Community 34 - "Stable Diffusion 3"
Cohesion: 1.0
Nodes (1): Stable Diffusion 3

### Community 35 - "FLUX.1"
Cohesion: 1.0
Nodes (1): FLUX.1

### Community 36 - "BridgeDiT"
Cohesion: 1.0
Nodes (1): BridgeDiT

### Community 37 - "ThinkDiffusion"
Cohesion: 1.0
Nodes (1): ThinkDiffusion (Review)

### Community 38 - "Distilled Pipeline"
Cohesion: 1.0
Nodes (1): DistilledPipeline

### Community 39 - "Wan 2.5"
Cohesion: 1.0
Nodes (1): Wan 2.5

### Community 40 - "fal.ai Endpoints"
Cohesion: 1.0
Nodes (1): fal.ai LTX Video Endpoints

### Community 41 - "v0.9.8"
Cohesion: 1.0
Nodes (1): LTX-Video v0.9.8

### Community 42 - "MaxVideoAI"
Cohesion: 1.0
Nodes (1): MaxVideoAI

### Community 43 - "VHS Extension"
Cohesion: 1.0
Nodes (1): ComfyUI-VideoHelperSuite (VHS)

### Community 44 - "Studio Reviews"
Cohesion: 1.0
Nodes (1): LTX Studio Platform Reviews

### Community 45 - "FLUX.1 Alt"
Cohesion: 1.0
Nodes (1): FLUX.1

### Community 46 - "Documentation Hub"
Cohesion: 1.0
Nodes (1): LTX Documentation Hub (docs.ltx.video)

### Community 47 - "RunPod"
Cohesion: 1.0
Nodes (1): RunPod

### Community 48 - "ModelScope"
Cohesion: 1.0
Nodes (1): ModelScope / ZeroScope

### Community 49 - "Latte"
Cohesion: 1.0
Nodes (1): Latte (Latent Diffusion Transformer)

### Community 50 - "Analytics Agents"
Cohesion: 1.0
Nodes (1): ltx-analytics-agents

### Community 51 - "LTX-2.3 HF Card"
Cohesion: 1.0
Nodes (1): LTX-2.3 HuggingFace Model Card

### Community 52 - "NVIDIA A100"
Cohesion: 1.0
Nodes (1): NVIDIA A100

### Community 53 - "MCP Video Tool"
Cohesion: 1.0
Nodes (1): mcp-video Tool

## Knowledge Gaps
- **442 isolated node(s):** `Audio VAE (Causal Audio Variational AutoEncoder)`, `Video VAE (Encoder/Decoder)`, `ICLoraPipeline`, `A2VidPipelineTwoStage (Audio-to-Video)`, `TI2VidTwoStagesPipeline (Text/Image-to-Video)` (+437 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **Thin community `Audio-to-Video`** (2 nodes): `A2VidPipelineTwoStage (Audio-to-Video)`, `Audio-Reactive Content Creation`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Acceleration Extensions`** (2 nodes): `Comfy-WaveSpeed Acceleration Extension`, `TeaCache Acceleration for LTX-Video`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Core Package`** (2 nodes): `ltx-core Package`, `QuantizationPolicy`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `IC-LoRA Pipeline`** (1 nodes): `ICLoraPipeline`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Two-Stage Pipeline`** (1 nodes): `TI2VidTwoStagesPipeline (Text/Image-to-Video)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `ComfyUI Wiki Tutorial`** (1 nodes): `ComfyUI Wiki LTX Workflow Tutorial`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `LTX Blog Guide`** (1 nodes): `LTX Blog ComfyUI Workflow Guide (LTX-2.3)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `WaveSpeed Tutorial`** (1 nodes): `WaveSpeedAI LTX-2 Quickstart Tutorial`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Tiled VAE`** (1 nodes): `Tiled VAE Decode Node`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Temporal Upscaler`** (1 nodes): `Temporal Upscaler (2x Frame Rate)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Download Stats`** (1 nodes): `LTX-2 Download Statistics (5M+ total, 796K/month peak)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Style LoRAs`** (1 nodes): `Style Transfer & Art (Anime, Pixel Art LoRAs)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Billing Model`** (1 nodes): `Per-Second-of-Output Billing Model`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `v0.9.8 Release`** (1 nodes): `LTX-Video v0.9.8 (13B, Distilled, Up to 60s Video)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Image Animation`** (1 nodes): `Prompt Structure for Image Animation`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Martini Desktop`** (1 nodes): `Martini (Desktop AI Workflow Tool)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `A1111 Legacy`** (1 nodes): `Automatic1111 / SD WebUI (Obsolete for Video)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `HunyuanVideo`** (1 nodes): `HunyuanVideo`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Stable Diffusion 3`** (1 nodes): `Stable Diffusion 3`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `FLUX.1`** (1 nodes): `FLUX.1`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `BridgeDiT`** (1 nodes): `BridgeDiT`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `ThinkDiffusion`** (1 nodes): `ThinkDiffusion (Review)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Distilled Pipeline`** (1 nodes): `DistilledPipeline`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Wan 2.5`** (1 nodes): `Wan 2.5`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `fal.ai Endpoints`** (1 nodes): `fal.ai LTX Video Endpoints`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `v0.9.8`** (1 nodes): `LTX-Video v0.9.8`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `MaxVideoAI`** (1 nodes): `MaxVideoAI`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `VHS Extension`** (1 nodes): `ComfyUI-VideoHelperSuite (VHS)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Studio Reviews`** (1 nodes): `LTX Studio Platform Reviews`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `FLUX.1 Alt`** (1 nodes): `FLUX.1`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Documentation Hub`** (1 nodes): `LTX Documentation Hub (docs.ltx.video)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `RunPod`** (1 nodes): `RunPod`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `ModelScope`** (1 nodes): `ModelScope / ZeroScope`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Latte`** (1 nodes): `Latte (Latent Diffusion Transformer)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Analytics Agents`** (1 nodes): `ltx-analytics-agents`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `LTX-2.3 HF Card`** (1 nodes): `LTX-2.3 HuggingFace Model Card`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `NVIDIA A100`** (1 nodes): `NVIDIA A100`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `MCP Video Tool`** (1 nodes): `mcp-video Tool`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `LTX-Video` connect `LTX-Video Core & Ecosystem` to `API & Competitive Landscape`, `Research & AVControl`, `Architecture & NVIDIA`, `Community Tools & GGUF`, `Product Competitors & MCP`, `Diffusers & Python SDK`, `VAE Architecture & Training`, `Desktop App & Licensing`, `LTX-2 Monorepo & Pipelines`?**
  _High betweenness centrality (0.222) - this node is a cross-community bridge._
- **Why does `LTX-2 (19B)` connect `API & Competitive Landscape` to `LTX-Video Core & Ecosystem`, `Research & AVControl`, `Architecture & NVIDIA`, `Community Tools & GGUF`, `Product Competitors & MCP`, `Diffusers & Python SDK`, `VAE Architecture & Training`, `Desktop App & Licensing`, `LTX-2 Monorepo & Pipelines`?**
  _High betweenness centrality (0.142) - this node is a cross-community bridge._
- **Why does `Lightricks` connect `Research & AVControl` to `LTX-Video Core & Ecosystem`, `API & Competitive Landscape`, `Architecture & NVIDIA`, `Community Tools & GGUF`, `Product Competitors & MCP`, `VAE Architecture & Training`, `Desktop App & Licensing`, `LTX-2 Monorepo & Pipelines`?**
  _High betweenness centrality (0.080) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `LTX-2 (19B)` (e.g. with `ID-LoRA` and `CAFA (Controllable Automatic Foley Artist)`) actually correct?**
  _`LTX-2 (19B)` has 2 INFERRED edges - model-reasoned connections that need verification._
- **Are the 5 inferred relationships involving `LTX-2.3 (22B)` (e.g. with `HunyuanVideo` and `Gemma 3 12B IT (Text Encoder)`) actually correct?**
  _`LTX-2.3 (22B)` has 5 INFERRED edges - model-reasoned connections that need verification._
- **What connects `Audio VAE (Causal Audio Variational AutoEncoder)`, `Video VAE (Encoder/Decoder)`, `ICLoraPipeline` to the rest of the system?**
  _442 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `LTX-Video Core & Ecosystem` be split into smaller, more focused modules?**
  _Cohesion score 0.02 - nodes in this community are weakly interconnected._