---
title: LTX MCP and Agent Integration
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/LTX-Desktop/blob/main/AGENTS.md
  - https://docs.ltx.video/welcome
  - https://ltx.io/model/ltx-developer-program
  - https://github.com/jcd315/comfyui-mcp-muse
tags:
  - mcp
  - claude-code
  - agent-integration
  - ltx
  - developer-tools
---

# LTX MCP and Agent Integration

As of April 2026, [[lightricks-company]] has **not released a dedicated MCP (Model Context Protocol) server** for Claude Code or Claude Desktop. However, there are multiple integration points between the [[ltx-ecosystem]] and AI coding agents.

## LTX Desktop Agent Integration

[[ltx-desktop]] includes an `AGENTS.md` file (symlinked to `CLAUDE.md`) in its repository that provides guidance for AI coding agents working on the codebase. This supports:

- **Claude Code** - via CLAUDE.md conventions
- **Cursor** - reads AGENTS.md as rules
- **OpenAI Codex** - supports AGENTS.md format

The file documents the three-layer architecture (React frontend, Electron main process, Python FastAPI backend), development workflow commands (`pnpm dev`, `pnpm typecheck`, `pnpm backend:test`), and coding conventions for agent-assisted contributions.

## LTX API for Programmatic Video Generation

While not an MCP server, the [[ltx-api]] can be called from any environment including Claude Code via standard HTTP requests:

- Text-to-Video, Image-to-Video, Audio-to-Video endpoints
- Single HTTP call returns a video (no polling or webhooks)
- Documentation: https://docs.ltx.video/welcome

Claude Code users can programmatically generate videos using the LTX API through bash/curl commands or custom scripts.

## Community MCP Integrations

Community-built MCP servers that integrate video generation with Claude Code (none officially from Lightricks):

- **ComfyUI MCP Server** (comfyui-mcp-muse) - An MCP server + Claude Code plugin for ComfyUI that can execute workflows including LTX Video templates, generate images and videos, visualize pipelines, and manage models. GitHub: https://github.com/jcd315/comfyui-mcp-muse
- **Claude Code Video Toolkit** - A video production MCP solution covering Remotion, screen recording, FFmpeg post-processing. Available on mcpworld.com.

## Future Outlook

Given that Lightricks actively supports coding agent integration (AGENTS.md), the [[ltx-api]] is designed for programmatic access, and ComfyUI already has MCP integration with LTX Video support, a dedicated Lightricks MCP server could be a future development. The [[ltx-builders-program]] may be a channel for early access to such integrations.
