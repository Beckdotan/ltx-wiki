# LTX and MCP / Claude Code Integration

## Overview

As of April 2026, Lightricks does **not appear to have released a dedicated MCP (Model Context Protocol) server** for Claude Code or Claude Desktop. However, there are multiple integration points between the LTX ecosystem and AI coding agents.

## LTX Desktop Agent Integration

LTX Desktop includes an `AGENTS.md` file (symlinked to `CLAUDE.md`) in its repository that provides guidance for AI coding agents working on the codebase. This supports:

- **Claude Code** - via CLAUDE.md conventions
- **Cursor** - reads AGENTS.md as rules
- **OpenAI Codex** - supports AGENTS.md format

The file documents the three-layer architecture (React frontend, Electron main process, Python FastAPI backend), development workflow commands, and coding conventions for agent-assisted contributions.

**Source:** https://github.com/Lightricks/LTX-Desktop/blob/main/AGENTS.md

## LTX API for Programmatic Video Generation

While not an MCP server per se, the LTX API can be called from any environment including Claude Code via standard HTTP requests:

- **Text-to-Video, Image-to-Video, Audio-to-Video** endpoints
- Single HTTP call returns a video (no polling or webhooks)
- Documentation: https://docs.ltx.video/welcome

This means Claude Code users could programmatically generate videos using the LTX API through bash/curl commands or custom scripts.

## Community MCP Integrations for Video

There are community-built MCP servers that integrate video generation with Claude Code, though none are officially from Lightricks:

- **ComfyUI MCP Server** (comfyui-mcp-muse) - An MCP server + Claude Code plugin for ComfyUI that can execute workflows including LTX Video templates, generate images and videos, visualize pipelines, and manage models
  - GitHub: https://github.com/jcd315/comfyui-mcp-muse

- **Claude Code Video Toolkit** - A video production MCP solution covering Remotion, screen recording, FFmpeg post-processing
  - Available on mcpworld.com

## LTX Developer Program

Lightricks offers an invite-only **LTX Builders** developer program for builders actively using LTX who want a closer relationship with the team behind the models.

**Website:** https://ltx.io/model/ltx-developer-program

## Potential Future Integration

Given that:
1. Lightricks actively supports coding agent integration (AGENTS.md in LTX Desktop)
2. The LTX API is designed for programmatic access
3. ComfyUI already has MCP integration with LTX Video support
4. The MCP ecosystem is rapidly growing

A dedicated Lightricks MCP server for Claude Code/Desktop could be a future development, but as of the research date it has not been publicly announced.

## Sources

- [LTX Desktop AGENTS.md](https://github.com/Lightricks/LTX-Desktop/blob/main/AGENTS.md)
- [LTX API Documentation](https://docs.ltx.video/welcome)
- [LTX Developer Program](https://ltx.io/model/ltx-developer-program)
- [ComfyUI MCP Server (community)](https://github.com/jcd315/comfyui-mcp-muse)
- [Claude Code MCP Documentation](https://code.claude.com/docs/en/mcp)
- [Anthropic Claude Plugins Official](https://github.com/anthropics/claude-plugins-official)
