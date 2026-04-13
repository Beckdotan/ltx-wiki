# LTX Desktop - Open-Source AI Video Production Suite

## Overview

LTX Desktop is a free, open-source desktop application for generating videos with LTX models. It runs locally on supported hardware or via API for unsupported systems. Currently in beta (v1.0.4 as of April 2026), it combines a full non-linear video editor with on-device AI generation.

**GitHub:** https://github.com/Lightricks/LTX-Desktop
**Website:** https://ltx.io/ltx-desktop
**License:** Apache 2.0

## Core Features

### Video Generation Modes
- **Text-to-Video** - Generate videos from text prompts
- **Image-to-Video** - Create videos from static images
- **Audio-to-Video** - Generate visuals synchronized to audio
- **Video-to-Video** - Enhance and transform existing footage

### Timeline Editor
- Full non-linear video editor interface
- Generate multiple takes per clip directly in the timeline
- Switch between takes non-destructively
- Context-aware gap fill (automatically generates content matching surrounding clips)
- **Retake** - Regenerate specific sections of a clip without leaving the timeline
- Timeline import from professional NLEs

### Generation Capabilities
- Up to 50fps at native 4K resolution
- Videos up to 20 seconds in Fast mode (quicker inference)
- Videos up to 10 seconds in Pro mode (higher quality)
- Image generation support
- Keyframe control
- LoRA support

### Project Management
- Project-based editing workflows
- Built-in video editor interface

## Powered By

LTX Desktop runs on **LTX-2.3**, a 20.9B (22B) parameter multimodal model from Lightricks.

## Platform Support & Operating Modes

### Local Generation (downloads model weights to machine)
- **Windows 10/11** with NVIDIA CUDA GPU (16GB+ VRAM)
- **Linux (Ubuntu 22.04+)** with NVIDIA CUDA GPU (16GB+ VRAM)

### API-Only Mode (requires LTX API key)
- Windows without qualifying GPU
- Linux without qualifying GPU
- **macOS (Apple Silicon)** - macOS 13+ (Ventura)

## System Requirements

### For Local Generation
- 16GB+ RAM (32GB recommended)
- 160GB+ free disk space
- NVIDIA GPU with CUDA support and minimum 16GB VRAM

### For API-Only Mode (macOS)
- Apple Silicon processor
- macOS 13+ (Ventura)
- Stable internet connection

## Technical Architecture

Three-layer architecture:
1. **Frontend** - React 18 + TypeScript, communicating via HTTP
2. **Electron** - Main process handling OS integration, app lifecycle, IPC
3. **Backend** - Python FastAPI server orchestrating generation and GPU tasks

### Frontend Details
- React contexts for state (ProjectContext, AppSettingsContext, KeyboardShortcutsContext)
- View-based routing without external state libraries
- Tailwind CSS with semantic tokens

### Backend Design
- Thin routes that parse input and delegate to handlers
- Centralized AppHandler as the composition root
- State machines using discriminated union types
- Protocol-based services with real and fake implementations for testing
- Thread pool concurrency with RLock management

## API Keys & Costs

### LTX API Key (free for text encoding)
- Text encoding: completely free
- Video generation: paid service
- Prompt enhancement: included

### Optional APIs
- **fal.ai** - for image generation features
- **Google Gemini** - for AI prompt suggestions

## Data Storage Locations
- Windows: `%LOCALAPPDATA%\LTXDesktop\`
- macOS: `~/Library/Application Support/LTXDesktop/`
- Linux: `$XDG_DATA_HOME/LTXDesktop/`

## Privacy

After initial model download, the application can run entirely offline (unless optional API features are enabled). When running locally, footage, prompts, and generated content never leave the machine.

## Licensing & Cost

- LTX Desktop is free and open source under Apache 2.0
- LTX-2.3 model is free for companies under $10M annual revenue
- Above that threshold, contact Lightricks for commercial terms
- Zero cost for local inference if hardware requirements are met

## Agent Integration (AGENTS.md / CLAUDE.md)

LTX Desktop includes an AGENTS.md file (symlinked to CLAUDE.md) that provides guidance for AI coding agents (Claude Code, Cursor, Codex) working on the repository. It documents the three-layer architecture, development workflow (`pnpm dev`, `pnpm typecheck`, `pnpm backend:test`), and coding conventions.

## Release History

- **v1.0.4** - April 3, 2026 (latest)
- Initial release accompanied LTX-2.3 launch in March 2026

## Upcoming Features (Roadmap)

- IC-LoRA integration for precise structural control
- Bridge Shots feature (powered by Gemini) for generating missing transition footage between clips

## Sources

- [LTX Desktop GitHub Repository](https://github.com/Lightricks/LTX-Desktop)
- [LTX Desktop README](https://github.com/Lightricks/ltx-desktop/blob/main/README.md)
- [LTX Desktop AGENTS.md](https://github.com/Lightricks/LTX-Desktop/blob/main/AGENTS.md)
- [LTX Desktop Releases](https://github.com/Lightricks/ltx-desktop/releases)
- [LTX Desktop on ltx.io](https://ltx.io/ltx-desktop)
- [LTX Desktop Setup Guide](https://ltx.io/model/model-blog/how-to-set-up-ltx-desktop)
- [LTX 2.3 Desktop App Review - CrePal](https://crepal.ai/blog/aivideo/ltx-2-3-desktop-app-review/)
- [LTX Desktop - MindStudio](https://www.mindstudio.ai/blog/ltx-desktop-free-open-source-ai-video-editor)
