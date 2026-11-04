# Changelog

All notable changes to this plugin will be documented here.

## 1.1.0

- The `padlet` MCP server now uses `https://mcp.padlet.com/` instead of `?toolsets=core`, so the
  plugin gets the same tool list as every other MCP client. That list is short, and its action
  tools find and run every other Padlet action, including padlet settings and library
  administration.
- Removed the `/padlet-toolsets` skill. The default list fits inside agent clients' tool limits,
  so there is no toolset to choose. A toolset choice made with the skill goes back to the default
  URL when the plugin updates.

## 1.0.0 — initial release

- Added the `padlet` MCP server pointing at `https://mcp.padlet.com`, requesting the `core`
  toolset. Authentication is OAuth 2.1 with PKCE, performed by the host client in the browser.
- Added the `/padlet-toolsets` skill for viewing and changing which Padlet toolsets are exposed
  (`core`, `utility`, `board_admin`, `library`).
- Logo: the Padlet mark. `assets/logo.svg` is transparent; `assets/logo-white-plate-512.png` is
  the same mark on an opaque white tile for host listings.
