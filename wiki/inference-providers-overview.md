---
title: LTX Video Inference Providers Overview
type: comparison
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-inference-providers-overview.md
  - raw/inference-providers-overview.md
  - raw/cloud-deployment-platforms.md
  - raw/integration-cloud-platforms.md
tags:
  - inference
  - cloud
  - api
  - providers
  - comparison
---

# LTX Video Inference Providers Overview

LTX Video models are available through multiple inference providers, from fully managed serverless APIs to self-deployed GPU infrastructure. This page compares all known providers and helps guide selection.

## Provider Summary

| Provider | Type | Auth Method | Pricing Model |
|----------|------|-------------|---------------|
| [[fal-ai]] | Serverless API | FAL_KEY env var | Per-second GPU billing |
| [[replicate]] | Serverless API | REPLICATE_API_TOKEN | Per-second GPU billing |
| [[segmind]] | Serverless API | x-api-key header | Per-request |
| [[wavespeed-ai]] | Serverless API | Bearer token | Per-second billing |
| [[huggingface-inference]] | API gateway + endpoints | HF token | Free tier + paid endpoints |
| [[modal]] | Self-deployed serverless | Modal account | Pay for GPU time |
| [[runpod]] | Cloud GPU rental + serverless | RunPod account | Per-hour GPU rental |
| [[runcomfy]] | Managed ComfyUI hosting | RunComfy account | Pay-as-you-go + Pro plan |

## Feature Availability by Provider

| Feature | fal.ai | Replicate | Segmind | WaveSpeed | HF Endpoints |
|---------|--------|-----------|---------|-----------|-------------|
| Text-to-Video | Yes | Yes | Yes | Yes | Yes |
| Image-to-Video | Yes | Yes | Yes | Yes | Yes |
| Audio-to-Video | Yes | No | No | Yes | Yes |
| Video Extend | Yes | No | No | Yes | Yes |
| Video Retake | Yes | Yes | No | Yes | Yes |
| LoRA Training | Yes | No | No | No | No |
| LoRA Inference | Yes | No | No | No | No |
| ControlNet | Yes | No | No | Yes | Yes |
| 4K Output | Yes | No | No | Yes | Yes |
| Audio Generation | Yes | No | No | Yes | Yes |
| Webhooks | Yes | Yes | No | No | Yes |
| Batch / Queue | Yes | Yes | No | No | Yes |

## Pricing Comparison (Text-to-Video)

| Provider | Fast/Standard | Pro/Quality |
|----------|--------------|-------------|
| [[fal-ai]] (1080p) | $0.04/s | $0.06/s |
| [[replicate]] | ~$0.021/run (~22s) | -- |
| [[segmind]] | Varies | Varies |
| [[wavespeed-ai]] | Varies | Varies |
| [[huggingface-inference]] Endpoints | ~$1-5/hour (dedicated) | -- |
| Self-hosted | GPU electricity only | GPU electricity only |

Prices are approximate as of early 2026. Check provider websites for current rates.

## Model Availability by Provider

| Provider | LTX-Video (original) | LTX-2 | LTX-2.3 | LTX-2 19B |
|----------|---------------------|-------|---------|-----------|
| [[fal-ai]] | Yes | Yes (Pro/Fast) | Yes | Yes |
| [[replicate]] | Yes | Yes (Retake) | No | No |
| [[segmind]] | Yes | Yes (Fast/Pro) | No | No |
| [[wavespeed-ai]] | Yes (0.9.7) | No | Yes | Yes |
| [[huggingface-inference]] | All variants | All | All | All |
| [[modal]] | Any (user deploys) | Any | Any | Any |
| [[runpod]] | Any (user deploys) | Any | Any | Any |

## Decision Guide

- **Just want to try it:** LTX Studio web UI (no setup) or [[huggingface-inference]] Spaces
- **Building an app (Python/JS):** [[fal-ai]] or [[replicate]]
- **Need LoRA training via API:** [[fal-ai]] (only provider with training API)
- **Need audio-video generation:** [[fal-ai]] or [[wavespeed-ai]]
- **Need maximum control:** [[modal]] or [[runpod]] (self-deployed)
- **Complex visual workflows:** [[runcomfy]] or [[runpod]] with ComfyUI
- **Research / fine-tuning:** Self-hosted from [[huggingface-inference]]
- **Production at scale:** [[fal-ai]], [[huggingface-inference]] Endpoints, or [[modal]]

## Self-Hosting VRAM Requirements

| Model | Min VRAM | Recommended VRAM |
|-------|----------|-----------------|
| LTX-Video 2B (distilled) | 8 GB | 16 GB |
| LTX-Video 13B | 16 GB | 24 GB |
| LTX-2 19B | 24 GB | 48 GB |
| LTX-2.3 22B | 24 GB | 48 GB |
| With FP8 quantization | ~40% less | ~40% less |

## Licensing for Cloud Deployment

- Free for academic research
- Free for companies with less than $10M ARR
- Commercial license required above $10M ARR threshold
