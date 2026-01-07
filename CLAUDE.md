# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin repository containing multiple plugins that extend Claude Code functionality. There is no build system, tests, or compiled code - plugins are Markdown files and JSON configurations.

## Plugin Architecture

Each plugin follows the Claude Code plugin structure:

```
plugins/<plugin-name>/
├── .claude-plugin/plugin.json   # Required manifest
├── commands/                     # Slash commands (*.md files)
├── agents/                       # Custom agents (*.md files)
├── hooks/                        # Event handlers (hooks.json + scripts)
└── .mcp.json                     # MCP server configurations
```

### Plugin Manifest

The `plugin.json` defines plugin metadata. The `name` field becomes the command namespace (e.g., `name: "code-toolkit"` → `/code-toolkit:command`).

### Commands vs Agents

- **Commands** (`commands/*.md`): User-invoked via `/plugin-name:command-name`. Include frontmatter with `description` and `allowed-tools`.
- **Agents** (`agents/*.md`): Model-invoked based on task context. Claude decides when to use them based on the `description` and `capabilities` in frontmatter.

### MCP Servers

Configured in `.mcp.json` at plugin root. Use `${CLAUDE_PLUGIN_ROOT}` for portable paths within hooks and scripts.

## Current Plugins

- **code-toolkit**: Git workflow commands + PR review agents + IDE/browser MCP integrations
- **nanobanana**: Gemini API image generation via MCP server
- **ralph-wiggum**: Autonomous iteration loops using Stop hooks

## Testing Plugins Locally

```bash
claude --plugin-dir ./plugins/code-toolkit
```

## Writing Plugin Content

- Frontmatter uses YAML between `---` markers
- `allowed-tools` restricts which tools the command can use (glob patterns supported)
- `description` is critical for agents - Claude uses it to decide when to invoke them
- Use `$ARGUMENTS` in command markdown to capture user input after the command name
