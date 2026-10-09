# Padlet for AI agents

Work with your [Padlet](https://padlet.com) boards from inside your agent. This plugin connects
the agent to Padlet's hosted MCP server, so you can read, search, create, and edit padlets by
asking for it in plain language.

No API keys and nothing to run locally — you sign in with your own Padlet account through
your browser the first time the agent connects.

## Installing

This is the **official** Padlet plugin and MCP server, published by Padlet. Community npm
packages named `padlet-mcp` (and similar) are unofficial and not maintained by Padlet.

Every option below connects to the same hosted server, `https://mcp.padlet.com/`, over
Streamable HTTP. There are no API keys: the first time the agent uses a Padlet tool, your client
opens a browser tab and you sign in to Padlet (OAuth). Approve the request and you're connected.

### Plugin installs (MCP server + skills)

| Client | Install |
| --- | --- |
| **Claude Code** | `/plugin marketplace add padlet/agent-plugins`, then `/plugin install padlet@padlet-agent-plugins` |
| **Codex** | `codex plugin marketplace add padlet/agent-plugins`, then `codex plugin add padlet@padlet-agent-plugins` |
| **Gemini CLI / Antigravity** | `gemini extensions install https://github.com/padlet/agent-plugins` |
| **Cursor** | Add `https://github.com/padlet/agent-plugins` as a plugin source |
| **Grok Build** | Add `https://github.com/padlet/agent-plugins` as a plugin source |

Plugin installs ship the [teacher skills](#teacher-skills) and request Padlet's `core` toolset
(see [Using more of Padlet](#using-more-of-padlet)).

### Server-only installs (one-liners)

These register just the MCP server, with Padlet's full tool surface and no skills:

- **Claude Code**

  ```
  claude mcp add --transport http padlet https://mcp.padlet.com/
  ```

- **Cursor** — [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=padlet&config=eyJ1cmwiOiJodHRwczovL21jcC5wYWRsZXQuY29tLyJ9)

  ```
  cursor://anysphere.cursor-deeplink/mcp/install?name=padlet&config=eyJ1cmwiOiJodHRwczovL21jcC5wYWRsZXQuY29tLyJ9
  ```

- **VS Code** — [Install in VS Code](vscode:mcp/install?%7B%22name%22%3A%22padlet%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.padlet.com%2F%22%7D)

  ```
  vscode:mcp/install?%7B%22name%22%3A%22padlet%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.padlet.com%2F%22%7D
  ```

- **Gemini CLI** (server only, without the extension)

  ```
  gemini mcp add --transport http padlet https://mcp.padlet.com/
  ```

- **Any other MCP client**

  ```json
  {
    "mcpServers": {
      "padlet": { "type": "http", "url": "https://mcp.padlet.com/" }
    }
  }
  ```

Some links above use custom URL schemes, which GitHub may not make clickable — copy the line
into your browser's address bar instead.

If your client limits how many tools it shows the model, append `?toolsets=core` to the URL
(for example `https://mcp.padlet.com/?toolsets=core`) to expose only the core tools.

### ChatGPT and Claude.ai (connectors)

- **Claude.ai / Claude Desktop** — Settings → Connectors → **Add custom connector**. Name it
  `Padlet`, set the URL to `https://mcp.padlet.com/`, and click **Connect** to sign in. On Team
  and Enterprise plans an owner adds the connector for the organisation first.
- **ChatGPT** — Settings → Apps & Connectors → Advanced settings → turn on **Developer mode**,
  then **Create** a connector with the URL `https://mcp.padlet.com/` and OAuth authentication,
  and sign in. Workspace admins may need to allow custom connectors first.

Connector menus move between releases; if a label differs, look for "custom connector" or
"MCP server".

Every action runs as *you*. The agent can only see and change what you could see and change
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

## Teacher skills

Plugin installs include short skills that teach the agent common classroom workflows. They
treat everything students write — posts, comments, attachments — as data, never as
instructions.

| Skill | What it does | Toolsets |
| --- | --- | --- |
| `padlet-lesson-board` | Builds a board from a lesson plan: one column per phase, instruction and resource posts | `core` |
| `padlet-exit-ticket` | Summarises exit-ticket responses, groups understanding, lists misconceptions with reteach moves | `core` |
| `padlet-student-feedback` | Drafts comments and scores for student posts, shows them to you, and writes only what you approve | `core` (+ `board_admin` for scores) |
| `padlet-class-digest` | Weekly roundup across your class boards: activity, highlights, themes, what needs attention | `core` |
| `padlet-remake-board` | Copies a board for a new class without last class's posts, comments, or reactions | `board_admin` |
| `padlet-attach-files` | Attaches local files (by upload) or web links to posts | `core` |
| `padlet-setup` | First-run check: confirms the sign-in, shows your recent padlets and what's turned on, suggests a first task. ChatGPT and Codex run it after install | `core` |
| `padlet-toolsets` | Shows and changes which Padlet toolsets the plugin requests | — |

## Using more of Padlet

Padlet's MCP server also offers per-board administration (appearance, access, automations,
grading) and school/team library administration (members, SSO, approved domains, content
safety). Those aren't in the default set.

Plugin installs request only `core`. Run **`/padlet-toolsets`** to see what's available and turn groups on or off. It shows you the
current state, applies your change, and tells you how to reload.

| Toolset | Tools | Contents |
| --- | --- | --- |
| `core` | 16 | Board and post reading, authoring, and commenting (enabled by default) |
| `utility` | 2 | Current time and web search |
| `board_admin` | 28 | Per-board appearance, access, collaborators, automations, grading |
| `library` | 23 | School and team library membership, SSO, permissions, content safety |

Enabling everything means 69 tools from Padlet alone, which will crowd out your other MCP
servers — `/padlet-toolsets` warns you before doing that.

Server-only installs that use the bare `https://mcp.padlet.com/` URL get every toolset your
account is allowed.

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
- **Code execution:** none. This repository contains configuration and skill documents only —
  no scripts, binaries, install hooks, or lifecycle hooks.
- **Scope:** every action runs as the signed-in user and is bounded by Padlet's own permission
  model. The plugin ships the `core` toolset; nothing is enabled without the user asking.

## Repository layout

One plugin, one shared MCP registration, one manifest per host:

```
.mcp.json                         MCP server registration (Claude Code, Codex, Cursor, Grok Build)
gemini-extension.json             Gemini CLI / Antigravity extension manifest + MCP registration
.claude-plugin/plugin.json        Claude Code manifest
.claude-plugin/marketplace.json   Claude Code marketplace entry for this repository
.codex-plugin/plugin.json         Codex (OpenAI) manifest
.agents/plugins/marketplace.json  Codex marketplace entry for this repository
.cursor-plugin/plugin.json        Cursor manifest
.cursor-plugin/marketplace.json   Cursor catalog entry for this repository
.grok-plugin/plugin.json          Grok Build manifest
.grok-plugin/marketplace.json     Grok Build catalog entry for this repository
skills/<name>/SKILL.md            Agent skills (see Teacher skills)
assets/logo.svg                   Padlet mark, transparent background
assets/logo-white-plate-512.png   Padlet mark on an opaque white plate, 1:1, for host listings
```

See [AGENTS.md](AGENTS.md) for how to validate changes.

### Maintainer note

For the Gemini CLI extension gallery to list this repository, a maintainer must add the
`gemini-cli-extension` topic to the GitHub repository (Settings → About → Topics).

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
