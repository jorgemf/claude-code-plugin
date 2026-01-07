# Claude Code Plugins

A collection of plugins for [Claude Code](https://claude.ai/code) to enhance developer workflows with git automation, code review agents, AI image generation, and autonomous task loops.

## Plugins

### [code-toolkit](./plugins/code-toolkit/)
Developer toolkit with git workflow commands, PR review agents, and IDE/browser integrations.

**Features:**
- 5 slash commands: `branch-new`, `commit-push`, `pr-new`, `code-review`, `pr-review`
- 6 review agents: comment-analyzer, pr-test-analyzer, silent-failure-hunter, type-design-analyzer, code-reviewer, code-simplifier
- MCP servers for JetBrains IDE and Chrome DevTools

### [nanobanana](./plugins/nanobanana/)
AI image generation using Google's Gemini API.

**Features:**
- Text-to-image generation
- Image editing and style transfer
- Support for Gemini 2.5 Flash and Pro models

### [ralph-wiggum](./plugins/ralph-wiggum/)
Autonomous task execution loop for Claude Code.

**Features:**
- Start/stop autonomous loops
- Custom hook integration
- Session-based task management

## Installation

### Using `--plugin-dir` flag

```bash
# Load a single plugin
claude --plugin-dir ./plugins/code-toolkit

# Load multiple plugins
claude --plugin-dir ./plugins/code-toolkit --plugin-dir ./plugins/nanobanana
```

### Installing from this repository

```bash
# Add as marketplace
/plugin marketplace add path/to/this/repo

# Install individual plugins
/plugin install code-toolkit@my-marketplace
/plugin install nanobanana@my-marketplace
```

## Requirements

- [Claude Code](https://claude.ai/code) 1.0.33+
- `gh` CLI for GitHub operations (code-toolkit)
- Gemini API key for image generation (nanobanana)
- JetBrains IDE with MCP plugin (optional, for code-toolkit)

## Project Structure

```
claude-code-plugin/
├── plugins/
│   ├── code-toolkit/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── .mcp.json
│   │   ├── agents/
│   │   ├── commands/
│   │   └── README.md
│   ├── nanobanana/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── .mcp.json
│   │   └── README.md
│   └── ralph-wiggum/
│       ├── .claude-plugin/plugin.json
│       ├── commands/
│       ├── hooks/
│       ├── scripts/
│       └── README.md
├── LICENSE
└── README.md
```

## Creating Your Own Plugin

Each plugin requires a manifest at `.claude-plugin/plugin.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What the plugin does",
  "author": {
    "name": "Your Name"
  }
}
```

Add optional components:
- `commands/` - Slash commands (Markdown files)
- `agents/` - Custom agents for Claude to invoke
- `skills/` - Model-invoked capabilities
- `hooks/` - Event handlers
- `.mcp.json` - MCP server configurations

See the [Claude Code Plugin Documentation](https://docs.anthropic.com/en/docs/claude-code/plugins) for details.

## License

MIT - See [LICENSE](./LICENSE) for details.
