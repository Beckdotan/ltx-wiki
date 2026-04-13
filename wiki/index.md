---
title: Wiki Index
type: overview
created: 2026-04-13
updated: 2026-04-13
---

# Wiki Index

Content catalog for the LTX wiki. 206 pages across 236 raw sources ingested on 2026-04-13.

## Overviews

- [[ltx-video-overview]] — Top-level hub for the LTX-Video model family
- [[ltx-2-overview]] — LTX-2 (19B) joint audio-video foundation model
- [[ltx-ecosystem]] — Map of all LTX products, URLs, and how they relate
- [[ltx-video-early-versions-overview]] — Timeline from 0.9.0 through 0.9.6
- [[competitor-landscape-overview]] — Full competitive landscape (model + product)
- [[open-source-video-generation-landscape]] — All open-source video gen models compared
- [[video-generation-architectures]] — Taxonomy of video generation architecture families
- [[comfyui-ltx-integration-overview]] — ComfyUI + LTX Video integration overview
- [[use-cases-overview]] — Commercial, creative, research, prototyping use cases
- [[lightricks-research-overview]] — All Lightricks research papers and themes

## Company & Products

- [[lightricks-company]] — Founded 2013, $1.8B valuation, 600 employees, 730M+ downloads
- [[ltx-studio]] — Web-based AI video production platform (5 pricing tiers)
- [[ltx-desktop]] — Free open-source desktop app (Apache 2.0), LTX-2.3 powered
- [[ltx-api]] — LTX developer API (5 endpoints, per-second billing)
- [[ltx-mcp-integration]] — MCP/agent integration status (AGENTS.md, no dedicated server yet)
- [[ltx-builders-program]] — Invite-only developer program
- [[nvidia-ltx-partnership]] — CES 2026, GDC 2026, NVFP4/NVFP8 quantization
- [[ltx-video-licensing]] — Apache 2.0 for code, custom open-weights for models

## LTX-Video Model Versions

- [[ltx-video-090]] — v0.9.0 (Nov 2024): initial release, 1.9B params, real-time generation
- [[ltx-video-091]] — v0.9.1 (Dec 2024): smoother motion, physics improvements
- [[ltx-video-095]] — v0.9.5 (Mar 2025): multi-keyframe conditioning, commercial license
- [[ltx-video-096]] — v0.9.6 (~Apr 2025): dev/distilled split, 15x speedup
- [[ltx-video-097]] — v0.9.7 (May 2025): 13B scale-up, multiscale rendering, 30x faster
- [[ltx-video-098]] — v0.9.8 (Jul 2025): IC-LoRA control, detailer, 2B distilled variants
- [[ltx-video-versions]] — Complete version timeline
- [[ltx-video-changelog]] — Detailed version-by-version changes
- [[ltx-video-capabilities]] — Capability matrix across all versions
- [[ltx-video-model-variants]] — All model variants inventory
- [[ltxv-model-variants]] — Variant reference (dev, distilled, FP8, mix)
- [[ltx-video-huggingface]] — All HuggingFace repositories and weights

## LTX-2 & LTX-2.3

- [[ltx-2-architecture]] — Asymmetric dual-stream DiT (14B video + 5B audio)
- [[ltx-2.3-model]] — LTX-2.3 (22B): rebuilt VAE, improved quality
- [[ltx-2-version-history]] — Release timeline (Oct 2025 → Mar 2026)
- [[ltx-2-capabilities]] — Native 4K, 50fps, 20s clips, 7 generation modes
- [[ltx-2-model-variants]] — Weights, distilled, GGUF, FP8, NVFP4
- [[ltx-2-benchmarks]] — Elo scores, speed comparisons, competitive positioning
- [[ltx-2-community-reception]] — Reddit 700+ upvotes, 5M+ downloads
- [[ltx-2-api-and-pricing]] — Official API + third-party providers
- [[ltx-2-lora-training]] — Three LoRA training modes, IC-LoRA adapters
- [[ltx-2-nvidia-optimization]] — NVFP4 (3x faster), NVFP8, EasyCache
- [[ltx-2-huggingface-ecosystem]] — Model repos, Spaces, community contributions
- [[ltx-2-huggingface]] — LTX-2 HuggingFace model card
- [[ltx-2-3-huggingface]] — LTX-2.3 HuggingFace model card
- [[ltx-video-to-ltx-2]] — Evolution from LTX-Video to LTX-2
- [[ltx-video-to-ltx-2-evolution]] — Technical migration details
- [[ltx2-open-source-overview]] — Open source vs API comparison

## Architecture & Technical Concepts

- [[ltx-video-architecture-overview]] — Full system architecture overview
- [[ltx-video-architecture]] — Architecture as it relates to versioning
- [[video-vae]] — Video-VAE: 1:192 compression, 128-channel latents
- [[denoising-decoder]] — Dual-purpose decoder (pixel conversion + final denoising)
- [[video-dit-transformer]] — DiT transformer: 28 blocks, full spatiotemporal attention
- [[diffusion-transformer]] — DiT architecture paradigm overview
- [[asymmetric-diffusion-transformer]] — Mochi's AsymmDiT (relates to LTX-2)
- [[latent-space-compression]] — 1:192 compression strategy and patchifier relocation
- [[rope-positional-encoding]] — RoPE with normalized fractional coordinates
- [[vae-loss-functions]] — rGAN, DWT, LPIPS, MSE, uniform log-variance
- [[text-conditioning]] — T5-XXL and Gemma 3 text encoders, cross-attention
- [[image-to-video-conditioning]] — Per-token timestep mechanism for I2V
- [[rectified-flow-training]] — Training: velocity prediction, multi-resolution
- [[classifier-free-guidance]] — CFG/STG and how distillation eliminates it
- [[multiscale-rendering]] — Coarse-to-fine generation, 30x speedup
- [[ltx-video-distillation]] — Knowledge distillation: 15x speed gain
- [[fp8-quantization]] — FP8 quantization: VRAM savings, comparison with GGUF
- [[ic-lora]] — IC-LoRA controlled generation (depth, pose, canny, detailer)
- [[ltxv-spatial-upscaler]] — 2x latent-space spatial upscaling
- [[ltxv-13b]] — 13B model architecture and launch
- [[audio-video-generation]] — Joint A/V generation, 9 modalities

## Research Papers

- [[paper-ltx-video]] — "LTX-Video: Realtime Video Latent Diffusion" (arXiv:2501.00103)
- [[paper-ltx-2]] — "LTX-2: Efficient Joint Audio-Visual Foundation Model" (arXiv:2601.03233)
- [[ltx-2.3-technical]] — LTX-2.3 technical improvements
- [[paper-avcontrol]] — "AVControl" — 13 control modalities via LoRA (arXiv:2603.24793)
- [[paper-cafa]] — "CAFA: Controllable Automatic Foley Artist" (arXiv:2504.06778)
- [[paper-just-dub-it]] — "JUST-DUB-IT: Video Dubbing" (arXiv:2601.22143)
- [[paper-id-lora]] — "ID-LoRA: Identity-Driven A/V Personalization" (arXiv:2603.10256)
- [[ltx-video-citations]] — 18+ papers citing LTX-Video
- [[ltx-training-methodology]] — Base model training: data pipeline, filtering, scaling

## Official API

- [[ltx-video-api]] — REST API overview (api.ltx.video)
- [[ltx-video-api-endpoints]] — All 6 endpoints with parameters
- [[ltx-video-api-models]] — Model tiers (fast, pro for LTX-2 and 2.3)
- [[ltx-video-api-pricing]] — Per-second pricing ($0.04–$0.32/s)
- [[ltx-video-api-prompting-guide]] — Official 6-element prompt structure
- [[ltx-video-api-input-formats]] — Supported image/video/audio formats
- [[ltx-video-api-errors]] — Error codes and debugging
- [[ltx-video-api-docs-sitemap]] — Full docs.ltx.video URL structure (28 pages)

## Inference Providers

- [[inference-providers-overview]] — All providers compared (feature matrix, pricing)
- [[fal-ai]] — 15+ endpoints, LoRA training API, ~$9.60/training run
- [[replicate]] — ~$0.021/run on L40S, Docker self-hosting
- [[segmind]] — REST endpoint, 3 model variants
- [[wavespeed-ai]] — 7 endpoint types, no cold starts, ControlNet
- [[huggingface-inference]] — Serverless API, Inference Endpoints, provider routing
- [[modal]] — Serverless GPU deployment, Volume caching
- [[runpod]] — Cloud GPU rental, serverless endpoints
- [[runcomfy]] — Managed ComfyUI hosting with pre-loaded LTX

## Python / Diffusers

- [[python-installation-setup]] — Installation guide (Diffusers, repo, LTX-2, PyPI)
- [[diffusers-pipeline-overview]] — All Diffusers pipeline classes catalog
- [[diffusers-integration]] — HuggingFace Diffusers overview and status
- [[ltx-pipeline-text-to-video]] — LTXPipeline reference and examples
- [[ltx-condition-pipeline]] — LTXConditionPipeline (multi-condition, I2V, V2V)
- [[ltx-image-to-video-pipeline]] — LTXImageToVideoPipeline for 2B model
- [[ltx-latent-upsample-pipeline]] — LTXLatentUpsamplePipeline reference
- [[ltx-long-multi-prompt-pipeline]] — LTXI2VLongMultiPromptPipeline
- [[ltx-native-pytorch-api]] — LTX-2 native ltx-pipelines (8 pipeline classes)
- [[ltx-2-pipeline-api]] — All LTX-2 pipeline classes detailed
- [[two-stage-inference-pattern]] — Recommended two-stage generation workflow
- [[python-two-stage-pipeline]] — Diffusers latent upscaling pattern
- [[python-conditioning]] — Multi-condition generation types
- [[python-lora-loading]] — LoRA loading, management, fusing, PEFT merge
- [[python-ic-lora]] — IC-LoRA usage in Python
- [[python-performance-speed]] — torch.compile, FP8, attention backends
- [[python-performance-memory]] — Group offloading, GGUF, tiling, multi-GPU
- [[ltxv-memory-optimization]] — Complete ~10GB VRAM recipe
- [[python-batch-generation]] — Batch processing patterns
- [[python-long-video-generation]] — Temporal tiling, multi-prompt
- [[python-guidance-parameters]] — CFG, STG, MultiModalGuiderParams
- [[python-schedulers]] — Scheduler configuration and custom timesteps
- [[ltxv-schedulers]] — Scheduler API reference
- [[python-callbacks]] — Progress monitoring, intermediate inspection
- [[python-output-formats]] — Output types, resolution helpers
- [[python-integration-patterns]] — FastAPI, Gradio, OpenCV, MoviePy, Modal
- [[ltxv-transformer-architecture]] — LTXVideoTransformer3DModel API
- [[ltxv-video-vae]] — AutoencoderKLLTXVideo API
- [[sdk-ltx-video]] — Original LTX-Video repo SDK
- [[sdk-ltx-2]] — LTX-2 monorepo SDK
- [[ltx2-diffusers-pipeline]] — LTX-2 Diffusers and PyTorch API guide

## ComfyUI

- [[comfyui-ltxvideo-official-nodes]] — Official Lightricks extension (3.4K stars)
- [[comfyui-ltx-node-reference]] — Per-node reference (inputs, outputs, parameters)
- [[comfyui-ltx-community-nodes]] — Third-party node packs (VRAM mgmt, WaveSpeed, GGUF)
- [[comfyui-ltxtricks]] — Deprecated LTXTricks (merged into official)
- [[comfyui-ltx-workflows]] — Workflows: txt2vid, img2vid, vid2vid
- [[comfyui-ltx-two-stage-pipeline]] — Two-stage upscaling pipeline
- [[comfyui-ltx-workflow-tutorials]] — 14+ tutorial catalog
- [[comfyui-ltx-performance-tips]] — VRAM optimization (8GB–32GB+ GPUs)
- [[comfyui-ltx-lora-training-control]] — LoRA/IC-LoRA in ComfyUI
- [[comfyui-ltx-model-comparison]] — LTX vs Wan vs CogVideo vs Hunyuan in ComfyUI
- [[comfyui-manager-ltx-setup]] — Manager installation and setup
- [[ltx2-comfyui-integration]] — LTX-2 specific ComfyUI guide
- [[ltx2-comfyui-nodes-reference]] — LTX-2 custom nodes reference

## Training & Fine-Tuning

- [[lora-training]] — Core LoRA training guide (all 3 paths)
- [[ltx2-training]] — LTX-2 training overview
- [[ltx-video-trainer]] — Official + community training tools
- [[training-dataset-preparation]] — 8n+1 frame rule, captioning, directories
- [[training-hyperparameters]] — Full config reference (YAML, args, accelerate)
- [[third-party-training-services]] — WaveSpeedAI, fal.ai, Oxen.ai, RunComfy
- [[lora-community-ecosystem]] — Community LoRAs, cross-version transfer
- [[lora-ecosystem]] — Full LoRA inventory (official + community)
- [[lora-training-and-finetuning]] — Use case perspective on training

## Community Projects

- [[gguf-quantizations]] — city96's GGUF variants (985MB–7.69GB, 3-4GB VRAM)
- [[jetson-thor-deployment]] — LTX-2 19B on NVIDIA Jetson AGX Thor
- [[promptgen]] — mushroomfleet's prompt toolkit (adopted by Lightricks)
- [[turbo-space]] — alexnasa's Flash Attention 3 Space (398 likes)
- [[lipsync-portrait-animation]] — Portrait animation and lip-sync Spaces
- [[camera-control-loras]] — Dolly, jib, static, motion track LoRAs
- [[gemini-comfyui-workflow]] — Gemini + LTX-Video ComfyUI integration
- [[community-models-finetunes]] — 90+ community models on HuggingFace
- [[render-benchmarks]] — RTX 5090 benchmarks, FP8 vs full-dev quality

## Adoption & Community Feedback

- [[adoption-metrics]] — 3.8M+ monthly downloads, 10K GitHub stars, 70K Discord
- [[ltx-adoption-metrics]] — Quantitative adoption data across platforms
- [[huggingface-spaces-ecosystem]] — 468+ Spaces with engagement metrics
- [[civitai-community]] — Civitai models, workflows, LoRAs
- [[discord-community]] — 70K+ member Discord server
- [[community-feedback]] — Praise (speed, quality) and complaints (config, VRAM)
- [[community-feature-requests]] — Top 10 most requested features
- [[community-sentiment-overview]] — Aggregated sentiment across all platforms
- [[creative-showcases]] — User examples, official LoRA demos, galleries

## Social Media & Press

- [[reddit-community-discussions]] — r/StableDiffusion, r/comfyui, r/aivideo, r/LocalLLaMA
- [[x-twitter-ltx-announcements]] — Official announcements, influencer coverage
- [[youtube-ltx-coverage]] — Tutorials from Kijai, Wes Roth, Stable Diffusion Art
- [[blog-reviews-ltx]] — Curious Refuge (7.3 visual), Awesome Agents (8.2/10)
- [[hackernews-ltx-discussions]] — 4 HN threads (13B, 60-second barrier)
- [[ltx-desktop-community-reception]] — Desktop app reception
- [[ltx-studio-platform-reviews]] — Studio G2/Trustpilot reviews
- [[ltx-model-comparisons]] — Community consensus vs Wan, HunyuanVideo
- [[ltx-speed-vs-quality-tradeoff]] — The dominant community theme
- [[ltx-audio-video-community-reception]] — Audio-video as differentiator
- [[ltx-version-history-community-timeline]] — Full timeline with community reaction

## Model Competitors

- [[wan-video]] — Alibaba Wan 2.1/2.2: best motion realism, Apache 2.0, but slower
- [[hunyuan-video]] — Tencent HunyuanVideo: 13B, best multi-person scenes
- [[cogvideo]] — Zhipu CogVideoX: strong text-video alignment, slower
- [[mochi]] — Genmo Mochi 1: 10B, great physics, stuck at 480p
- [[open-sora]] — HPC-AI Tech: MIT license, $200K training, near-Sora quality
- [[open-sora-plan]] — PKU Yuan Group: minute-scale synthesis, research-focused
- [[animatediff]] — SD 1.5 plugin, largely obsolete
- [[stable-video-diffusion]] — Stability AI SVD: I2V only, historically influential

## Product Competitors

- [[competitor-runway]] — Gen-3/Gen-4: industry standard, $12–76/mo
- [[competitor-pika]] — Pika Labs: social media focus, Pikaffects/Pikaswaps
- [[competitor-kling]] — Kling AI: 22M users, $240M ARR, 3-min duration
- [[competitor-sora]] — Sora (OpenAI): SHUTTING DOWN Apr 2026
- [[competitor-luma]] — Luma Dream Machine: reasoning-based, native HDR/EXR
- [[competitor-veo]] — Veo 3.1 (Google): strongest closed-source, native 4K
- [[desktop-video-tools]] — ComfyUI, A1111, Martini, Fooocus vs LTX Desktop
- [[mcp-video-integrations]] — Pictory, Veo2, MiniMax MCP vs LTX MCP

## Guides & Tutorials

- [[prompt-engineering]] — Prompt templates, negative prompts, model-specific guidance
- [[hardware-requirements]] — VRAM tables per model variant, GPU recommendations
- [[hardware-accessibility]] — Running on GTX 1650 (4GB), Apple Silicon, cloud
- [[apple-silicon-setup]] — Torch 2.4.1 requirement, Mac performance
- [[installation-quickstart]] — All installation methods (API, local, Diffusers, ComfyUI)
- [[upscaler-pipelines]] — Spatial + temporal upscalers, multi-step workflow
- [[tutorials-and-community-guides]] — Directory of 15+ tutorials and blog posts
- [[ltx-video-dev-vs-distilled]] — Head-to-head dev vs distilled comparison
- [[ltx2-text-to-video-guide]] — LTX-2 text-to-video usage guide
- [[ltx2-image-to-video-guide]] — LTX-2 image-to-video guide
- [[ltx2-ic-lora-guide]] — LTX-2 IC-LoRA guide
- [[ltx2-system-requirements]] — LTX-2 GPU, memory, storage requirements
- [[ltx-integration-projects]] — Full integration landscape
