# Padlet for AI coding agents

Work with your [Padlet](https://padlet.com) boards from inside your agent. This plugin connects
the agent to Padlet's hosted MCP server, so you can read, search, create, and edit padlets by
asking for it in plain language.

No API keys and nothing to run locally — you sign in with your own Padlet account through
your browser the first time the agent connects.

## Installing

The plugin supports **Cursor** and **Grok Build**, both of which install plugins from a source
repository — point your host at this repository:

```
https://github.com/padlet/agent-plugins
```

You can also skip the plugin and register the MCP server directly with any client that speaks
MCP over streamable HTTP:

```json
{
  "mcpServers": {
    "padlet": { "type": "http", "url": "https://mcp.padlet.com/?toolsets=core" }
  }
}
```

The first time the agent uses a Padlet tool it will prompt you to sign in. Approve the request
in the browser tab that opens and you're connected.

Every action runs as *you*. The plugin can only see and change what you could see and change
in Padlet itself.

## What you can ask for

- "Find my board about the Q3 retro and summarise what people posted."
- "Create a padlet called Release Checklist with a section per service."
- "Add each item in TODO.md as a post on my Sprint Planning board."
- "What's on the Design Feedback board? Group the posts by theme."
- "Comment on the top post asking for a repro case."

Out of the box the plugin exposes Padlet's **core** toolset — listing and searching your
padlets, reading board content, creating padlets, and creating, editing, connecting, and
commenting on posts. Attachments (images, video, audio, documents) can be analysed too.

The core set is intentionally small. Agent clients limit how many tools a model can see across
*all* of your connected MCP servers, so this plugin stays out of the way of the rest of your
setup.

## Using more of Padlet

Padlet's MCP server also offers per-board administration (appearance, access, automations,
grading) and school/team library administration (members, SSO, approved domains, content
safety). Those aren't in the default set.

Run **`/padlet-toolsets`** to see what's available and turn groups on or off. It shows you the
current state, applies your change, and tells you how to reload.

| Toolset | Tools | Contents |
| --- | --- | --- |
| `core` | 16 | Board and post reading, authoring, and commenting (enabled by default) |
| `utility` | 2 | Current time and web search |
| `board_admin` | 28 | Per-board appearance, access, collaborators, automations, grading |
| `library` | 23 | School and team library membership, SSO, permissions, content safety |

Enabling everything means 69 tools from Padlet alone, which will crowd out your other MCP
servers — `/padlet-toolsets` warns you before doing that.

The setting lives inside the installed plugin, so updating or reinstalling the plugin resets it
to `core`. Re-run `/padlet-toolsets` to restore your selection.

## Limitations

- Padlet's Sandbox (whiteboard) format is only partially supported; other board formats are
  fully supported.
- Deleting posts and sections is a soft delete and can be undone. A few operations —
  deleting comments, automations, or custom fields, and removing collaborators or
  invitations — are permanent, and are marked so that the agent asks you to confirm first.

## What this plugin talks to

Declared for review and for anyone auditing the plugin before installing it:

- **Network endpoint:** `https://mcp.padlet.com` — Padlet's hosted MCP server, and the only host
  this plugin contacts.
- **Credentials:** none stored in this repo. Authentication is OAuth 2.1 with PKCE, performed by
  your agent client in the browser; tokens are held by the client, never by these files.
- **Code execution:** none. This repository contains configuration and a skill document only —
  no scripts, binaries, install hooks, or lifecycle hooks.
- **Scope:** every action runs as the signed-in user and is bounded by Padlet's own permission
  model. The plugin ships the `core` toolset; nothing is enabled without the user asking.

## Repository layout

One plugin, one MCP registration, one manifest per host:

```
mcp.json                         MCP server registration (shared by all hosts)
.cursor-plugin/plugin.json       Cursor manifest
.grok-plugin/plugin.json         Grok Build manifest
skills/padlet-toolsets/SKILL.md  /padlet-toolsets skill
assets/logo.svg                  Padlet mark, transparent background
assets/logo-white-plate-512.png  Padlet mark on an opaque white plate, 1:1, for host listings
```

## Support

- Padlet help centre: https://padlet.help
- Issues with this plugin: https://github.com/padlet/agent-plugins/issues

## License

MIT — see [LICENSE](LICENSE). This repository contains plugin configuration only; the MCP
server itself is hosted by Padlet at `https://mcp.padlet.com`.

### Trademarks

Padlet and the Padlet logo are trademarks of Wallwisher, Inc. The MIT license above covers the
configuration and documentation in this repository; it does **not** grant any right to use
Padlet's name or marks. `assets/logo.svg` is excluded from that license and is provided solely
so hosts can display it when listing this plugin.
