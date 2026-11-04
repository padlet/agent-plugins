# Padlet for AI agents

Work with your [Padlet](https://padlet.com) boards from inside your agent. This plugin connects
the agent to Padlet's hosted MCP server, so you can read, search, create, and edit padlets by
asking for it in plain language.

No API keys and nothing to run locally — you sign in with your own Padlet account through
your browser the first time the agent connects.

## Installing

The plugin supports **Cursor** and **Grok Build**.

**Grok Build** — install straight from this repository:

```
grok plugin install padlet/agent-plugins --trust
```

**Cursor** — on a Teams or Enterprise plan, an admin can add this repository as a team
marketplace (**Dashboard → Plugins & MCPs → Add Marketplace → Import from Repo**), and
everyone can then install it from **Customize**:

```
https://github.com/padlet/agent-plugins
```

On any plan you can copy the repository into `~/.cursor/plugins/local/padlet` and reload the
window.

You can also skip the plugin and register the MCP server directly with any client that speaks
MCP over streamable HTTP:

```json
{
  "mcpServers": {
    "padlet": { "type": "http", "url": "https://mcp.padlet.com/" }
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
- "Freeze the Q3 retro board so no one can post any more."

## Tools

The plugin connects to Padlet's default tool list, about 20 tools. Most of them find, read, and
edit padlets, posts, sections, and comments. Five action tools (`search_padlet_actions`,
`describe_padlet_action`, and the three `run_padlet_*_action` tools) find and run everything else
Padlet can do: padlet settings and sharing, collaborators, automations, grading, and, if you are
an owner, admin, or teacher in a Padlet for Schools or Teams library, library administration.
The [Padlet MCP server README](https://github.com/padlet/padlet-mcp#tools) lists every tool.

Agent clients limit how many tools a model can see across *all* of your connected MCP servers.
The short list leaves room for the rest of your setup, so there is nothing to turn on or off.

For a connection that can only find and read padlets, skip the plugin and register
`https://mcp.padlet.com/read-only` with your client instead.

## Limitations

- Padlet's Sandbox (whiteboard) format is only partially supported; other board formats are
  fully supported.
- Deleted posts and sections can be restored. Tools that overwrite what someone wrote, delete
  something, or change a whole library in a way that can't be put back are marked destructive,
  so clients that honour MCP tool annotations can ask you to confirm first.

## What this plugin talks to

Declared for review and for anyone auditing the plugin before installing it:

- **Network endpoint:** `https://mcp.padlet.com` — Padlet's hosted MCP server, and the only host
  this plugin contacts.
- **Credentials:** none stored in this repo. Authentication is OAuth 2.1 with PKCE, performed by
  your agent client in the browser; tokens are held by the client, never by these files.
- **Code execution:** none. This repository contains configuration and documentation only — no
  scripts, binaries, skills, install hooks, or lifecycle hooks.
- **Scope:** every action runs as the signed-in user and is bounded by Padlet's own permission
  model. The agent can reach any action that user could take in Padlet, including padlet
  settings and, for library owners, admins, and teachers, library administration.

## Repository layout

One plugin, one MCP registration, one manifest per host:

```
.mcp.json                        MCP server registration (shared by all hosts)
.cursor-plugin/plugin.json       Cursor manifest
.cursor-plugin/marketplace.json  Cursor catalog entry for this repository
.grok-plugin/plugin.json         Grok Build manifest
.grok-plugin/marketplace.json    Grok Build catalog entry for this repository
assets/logo.svg                  Padlet mark, transparent background
assets/logo-white-plate-512.png  Padlet mark on an opaque white plate, 1:1, for host listings
```

## Support

- Padlet help centre: https://padlet.help
- Issues with this plugin: https://github.com/padlet/agent-plugins/issues

## Privacy and terms

Your use of Padlet through this plugin is covered by Padlet's
[Privacy Policy](https://padlet.com/about/privacy) and
[Terms of Service](https://padlet.com/about/terms).

## License

MIT — see [LICENSE](LICENSE). This repository contains plugin configuration only; the MCP
server itself is hosted by Padlet at `https://mcp.padlet.com`.

### Trademarks

Padlet and the Padlet logo are trademarks of Wallwisher, Inc. The MIT license above covers the
configuration and documentation in this repository; it does **not** grant any right to use
Padlet's name or marks. `assets/logo.svg` is excluded from that license and is provided solely
so hosts can display it when listing this plugin.
