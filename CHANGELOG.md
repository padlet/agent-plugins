# Changelog

All notable changes to this plugin will be documented here.

## 1.1.0 — multi-host plugin and teacher skills

- New hosts: Claude Code (`.claude-plugin/plugin.json` + marketplace), Codex
  (`.codex-plugin/plugin.json` + `.agents/plugins/marketplace.json`), and Gemini CLI /
  Antigravity (`gemini-extension.json`). Cursor and Grok Build manifests unchanged apart from
  the version. Every host points at Padlet's hosted server, `https://mcp.padlet.com/`.
- New teacher skills: `padlet-lesson-board`, `padlet-exit-ticket`, `padlet-student-feedback`,
  `padlet-class-digest`, `padlet-remake-board`, `padlet-attach-files`. All treat student-written
  content as untrusted data; `padlet-student-feedback` always shows drafted comments and scores
  and waits for the teacher's approval before writing.
- `/padlet-toolsets` now also updates the Gemini registration in `gemini-extension.json`.
- README: per-client install instructions — plugin installs, server-only one-liners (Claude
  Code, Cursor and VS Code deep links, Gemini CLI), and ChatGPT / Claude.ai connectors — plus a
  note that community `padlet-mcp` packages are unofficial.
- Added `AGENTS.md` with the repository layout and validation steps.

## 1.0.0 — initial release

- Added the `padlet` MCP server pointing at `https://mcp.padlet.com`, requesting the `core`
  toolset. Authentication is OAuth 2.1 with PKCE, performed by the host client in the browser.
- Added the `/padlet-toolsets` skill for viewing and changing which Padlet toolsets are exposed
  (`core`, `utility`, `board_admin`, `library`).
- Logo: the Padlet mark. `assets/logo.svg` is transparent; `assets/logo-white-plate-512.png` is
  the same mark on an opaque white tile for host listings.
