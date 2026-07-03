# MyAider Claude Code Plugins

This repository is a Claude Code plugin marketplace for MyAider-focused workflows.

MyAider is a skillful MCP hub: instead of relying on runtime tool introspection, it delivers tool-ready skills that already include instructions and tool details. In practice, this helps reduce token overhead and makes skill execution more predictable.

## What is included

Marketplace metadata is defined in `.claude-plugin/marketplace.json` with marketplace name:

- `myaider-plugins`

Currently available plugin:

- `myaider-skill-importer` (v1.0.0)
  - Purpose: import and upgrade skills from your MyAider MCP into Claude Code skills
  - Source: `./myaider-skill-importer`

## Install via marketplace

Claude Code marketplace flow is:

1. Add a marketplace
2. Install specific plugins from that marketplace

This follows Claude Code's plugin docs: https://code.claude.com/docs/en/discover-plugins

### Option A: Add from local path (for development)

From inside Claude Code:

```shell
/plugin marketplace add ./claude-code-plugins
/plugin install myaider-skill-importer@myaider-plugins
```

### Option B: Add from GitHub (for shared use)

Currently github repo is `hurungang/claude-code-plugins`:

```shell
/plugin marketplace add hurungang/claude-code-plugins
/plugin install myaider-skill-importer@myaider-plugins
```

### Optional: CLI equivalent

```bash
claude plugin install myaider-skill-importer@myaider-plugins --scope user
```

You can also use `--scope project` (shared in `.claude/settings.json`) or `--scope local` (local-only).

## Quick start: import skills from MyAider MCP

After installing the plugin, ask Claude:

```text
Import my MyAider skills
```

or

```text
Upgrade my MyAider skills to the latest version
```

The `myaider-skill-importer` workflow will:

1. Detect your configured MyAider MCP server by finding `get_myaider_skills`
2. Fetch available skills from MyAider
3. Let you choose all skills or selected skills
4. Create or upgrade local Claude skills with embedded tool schemas and descriptions

## Prerequisites

Before importing skills, ensure your MyAider MCP server is configured in Claude Code.

If not configured yet, start here:

- https://www.myaider.ai/mcp

## Manage plugins and marketplaces

Useful Claude Code commands:

```shell
/plugin
/plugin marketplace list
/plugin marketplace update myaider-plugins
/plugin uninstall myaider-skill-importer@myaider-plugins
/reload-plugins
```

## Security note

Plugins and marketplaces are highly trusted and can execute code with your user privileges. Only add marketplaces and plugins from sources you trust.

## License

MIT
