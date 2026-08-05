# Changelog

All notable changes to this plugin will be documented here.

## 1.0.0 — initial release

- Added the `padlet` MCP server pointing at `https://mcp.padlet.com`, requesting the `core`
  toolset. Authentication is OAuth 2.1 with PKCE, performed by the host client in the browser.
- Added the `/padlet-toolsets` skill for viewing and changing which Padlet toolsets are exposed
  (`core`, `utility`, `board_admin`, `library`).
- Logo: the Padlet mark. `assets/logo.svg` is transparent; `assets/logo-white-plate-512.png` is
  the same mark on an opaque white tile for host listings.
