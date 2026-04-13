---
title: LTX Desktop
type: product
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/LTX-Desktop
  - https://github.com/Lightricks/ltx-desktop/releases
  - https://ltx.io/ltx-desktop
  - https://github.com/Lightricks/LTX-Desktop/blob/main/AGENTS.md
  - https://ltx.io/model/model-blog/how-to-set-up-ltx-desktop
  - https://crepal.ai/blog/aivideo/ltx-2-3-desktop-app-review/
  - raw/ltx2-nvidia-optimization-and-desktop.md
tags:
  - ltx-desktop
  - open-source
  - ai-video
  - electron
  - lightricks
---

# LTX Desktop

LTX Desktop is a free, open-source desktop application for generating videos with LTX models. Part of the [[ltx-ecosystem]], it combines a full non-linear video editor with on-device AI generation. Currently in beta (v1.0.4 as of April 2026).

- **GitHub:** https://github.com/Lightricks/LTX-Desktop
- **Website:** https://ltx.io/ltx-desktop
- **License:** Apache 2.0
- **Model:** [[ltx-2-overview]] (LTX-2.3, 20.9B/22B parameters)
- **Initial Release:** March 2026

## Video Generation Modes

- **Text-to-Video** - Generate videos from text prompts
- **Image-to-Video** - Create videos from static images
- **Audio-to-Video** - Generate visuals synchronized to audio
- **Video-to-Video** - Enhance and transform existing footage

## Timeline Editor

- Full non-linear video editor interface
- Generate multiple takes per clip directly in the timeline
- Switch between takes non-destructively
- Context-aware gap fill (automatically generates content matching surrounding clips)
- Retake: regenerate specific sections without leaving the timeline
- Timeline import from professional NLEs

## Generation Capabilities

- Up to 50fps at native 4K resolution
- Videos up to 20 seconds in Fast mode (quicker inference)
- Videos up to 10 seconds in Pro mode (higher quality)
- Image generation support
- Keyframe control
- LoRA support

## Platform Support

| Platform | Local Inference | API Mode |
|----------|----------------|----------|
| Windows 10/11 (NVIDIA CUDA GPU, 16GB+ VRAM) | Yes | Yes |
| Linux Ubuntu 22.04+ (NVIDIA CUDA GPU, 16GB+ VRAM) | Yes | Yes |
| macOS 13+ Ventura (Apple Silicon) | No | Yes |

### System Requirements for Local Generation

- 16GB+ RAM (32GB recommended)
- 160GB+ free disk space
- NVIDIA GPU with CUDA support and minimum 16GB VRAM

### API-Only Mode (macOS / unsupported GPU)

- Apple Silicon processor or any system
- Stable internet connection
- Requires [[ltx-api]] key (video generation is paid)

## Technical Architecture

Three-layer architecture:

1. **Frontend** - React 18 + TypeScript, communicating via HTTP to localhost:8000. Uses React contexts for state management (ProjectContext, AppSettingsContext, KeyboardShortcutsContext). Tailwind CSS with semantic tokens.
2. **Electron** - TypeScript main process handling OS integration, app lifecycle, IPC. File dialogs, native export via ffmpeg. Starts and manages the Python backend.
3. **Backend** - Python FastAPI server orchestrating generation and GPU tasks. Thin routes delegating to handlers. Centralized AppHandler as composition root. State machines using discriminated union types. Protocol-based services with real and fake implementations for testing.

## API Keys and Costs

- **LTX API Key** (free for text encoding): text encoding is free; video generation is paid; prompt enhancement included
- **fal.ai** (optional): for image generation features
- **Google Gemini** (optional): for AI prompt suggestions

## Data Storage

- Windows: `%LOCALAPPDATA%\LTXDesktop\`
- macOS: `~/Library/Application Support/LTXDesktop/`
- Linux: `$XDG_DATA_HOME/LTXDesktop/`

## Privacy

After initial model download, the application can run entirely offline (unless optional API features are enabled). When running locally, footage, prompts, and generated content never leave the machine.

## Licensing and Cost

- LTX Desktop app: free and open source under Apache 2.0
- LTX-2.3 model weights: free for companies under $10M annual revenue (see [[ltx-video-licensing]])
- Above that threshold, contact [[lightricks-company]] for commercial terms

## Agent Integration

LTX Desktop includes an `AGENTS.md` file (symlinked to `CLAUDE.md`) providing guidance for AI coding agents. See [[ltx-mcp-integration]] for details on how this supports Claude Code, Cursor, and OpenAI Codex.

## Community Reception

Divided opinions across the community:
- Praised for making [[ltx-2-overview|LTX-2]] accessible without ComfyUI setup
- Some prefer ComfyUI's flexibility and node-based workflow
- Hardware requirements remain a recurring concern
- Featured in The Neuron Daily: "BREAKING: LTX 2.3 is an open video production studio on your desktop"

See [[ltx-2-community-reception]] for broader reception context.

## Local vs Cloud Trade-offs

### Benefits of Local Generation
- No per-generation cost after hardware purchase
- Complete privacy -- data stays on machine
- No internet dependency
- Full control over model settings
- Compatible with [[ltx-2-nvidia-optimization|GGUF/FP8/NVFP4 quantizations]]

### Trade-offs
- Requires capable GPU hardware
- Generation speed limited by local GPU
- No access to "Ultra" tier quality (API-only)

## Roadmap

- IC-LoRA integration for precise structural control
- Bridge Shots feature (powered by Gemini) for generating missing transition footage between clips

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2.3-model]] -- Model version released alongside Desktop
- [[ltx-2-nvidia-optimization]] -- GPU optimizations that make local inference viable
- [[ltx-2-api-and-pricing]] -- Cloud API alternative
- [[ltx-2-community-reception]] -- Community feedback
