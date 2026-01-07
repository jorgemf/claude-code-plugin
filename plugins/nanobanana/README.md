# Nanobanana

AI image generation plugin for Claude Code using Google's Gemini API.

## Overview

This plugin integrates the [Nano Banana MCP server](https://github.com/zhongweili/nanobanana-mcp-server) to provide AI-powered image generation capabilities directly within Claude Code conversations.

## Features

- **Text-to-Image Generation**: Generate images from text descriptions
- **Image Editing**: Modify existing images with AI
- **Style Transfer**: Apply artistic styles to images
- **High Quality Output**: Up to 4K resolution with Gemini Pro models

## Setup

### 1. Get a Gemini API Key

1. Go to [Google AI Studio](https://aistudio.google.com/)
2. Create or sign in to your Google account
3. Generate an API key

### 2. Configure Environment Variable

Add your API key to your environment:

```bash
export GEMINI_API_KEY="your-api-key-here"
```

Or add it to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.).

## Usage

Once configured, you can ask Claude to generate images:

```
"Generate an image of a sunset over mountains"

"Create a logo for a tech startup"

"Edit this image to add a rainbow"
```

## MCP Server

This plugin uses `nanobanana-mcp-server` via uvx:

```json
{
  "mcpServers": {
    "nanobanana": {
      "command": "uvx",
      "args": ["nanobanana-mcp-server@latest"],
      "env": {
        "GEMINI_API_KEY": "${GEMINI_API_KEY}"
      }
    }
  }
}
```

## Requirements

- Python/uvx for running the MCP server
- Valid Gemini API key
- Internet connection for API calls

## Available Tools

The MCP server provides these tools:

| Tool | Description |
|------|-------------|
| `generate_image` | Generate images from text prompts |
| `edit_image` | Edit/modify existing images |

## Models

- **Gemini 2.5 Flash**: Fast generation, good quality
- **Gemini Pro**: Higher quality, professional results up to 4K

## Troubleshooting

### API Key Not Found
Ensure `GEMINI_API_KEY` is set in your environment and accessible to Claude Code.

### Rate Limits
Google AI has usage limits. Check your quota in Google AI Studio if you encounter errors.

### Image Not Generated
Some prompts may be filtered by content policies. Try rephrasing your request.

## Resources

- [Nano Banana MCP Server](https://github.com/zhongweili/nanobanana-mcp-server)
- [Google AI Studio](https://aistudio.google.com/)
- [Gemini API Documentation](https://ai.google.dev/docs)

## License

MIT
