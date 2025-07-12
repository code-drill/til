<!--
.. title: Claude Desktop MCP Configuration Guide
.. slug: claude-desktop-mcp-configuration-guide
.. date: 2025-07-12 23:36:35 UTC+02:00
.. tags: LLM,Claude,MCP,playwright,npx,docker
.. category: LLM 
.. link: 
.. description: 
.. type: text
-->

# Claude Desktop MCP Configuration Guide

This guide covers how to configure Claude Desktop with various MCP (Model Context Protocol) servers to extend Claude's capabilities with filesystem access, web browsing, documentation search, and persistent memory.

## Configuration Setup

### Config File Location

The Claude Desktop configuration file is located at:

**Windows:**
```
C:\Users\USER\AppData\Roaming\Claude\claude_desktop_config.json
```

You can also use the environment variable:
```
%USERPROFILE%\AppData\Roaming\Claude\claude_desktop_config.json
```

### Configuration Methods

You can configure MCP servers using two different approaches:

#### NPX-Based Configuration

This method uses Node.js packages directly:

```json
{
    "mcpServers": {
        "filesystem": {
            "args": [
                "-y",
                "@modelcontextprotocol/server-filesystem",
                "C:\\Users\\homee\\Desktop",
                "D:\\kotlin-workspace"
            ],
            "command": "npx"
        },
        "playwright": {
            "args": [
                "@playwright/mcp@latest"
            ],
            "command": "npx"
        }
    }
}
```

#### Docker-Based Configuration

This method uses Docker containers for isolated execution:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--mount", "type=bind,src=c:\\Users\\homee\\Desktop,dst=/projects/Desktop",
        "--mount", "type=bind,src=d:\\kotlin-workspace\\michal_rutkowski,dst=/projects/michal_rutkowski",
        "mcp/filesystem",
        "/projects"
      ]
    }
  }
}
```

## Available MCP Servers

### Filesystem Access

Provides Claude with access to your local filesystem for reading and writing files.

**Configuration:**
```json
"filesystem": {
    "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\homee\\Desktop",
        "D:\\kotlin-workspace"
    ],
    "command": "npx"
}
```

### Playwright Web Browser Integration

Adds web browsing capabilities to Claude, allowing it to interact with websites, take screenshots, and automate web tasks.

**Configuration:**
```json
"playwright": {
    "args": [
        "@playwright/mcp@latest"
    ],
    "command": "npx"
}
```

### Context7 Documentation Access

Provides up-to-date documentation for popular libraries and frameworks, perfect for coding assistance.

**Configuration:**
```json
"Context7": {
    "args": [
        "-y",
        "@upstash/context7-mcp"
    ],
    "command": "npx"
}
```

**Usage Example:**
```
Create a CRUD API in FastAPI with authentication. use context7
```

### Basic Memory - Local LLM Memory

Gives Claude persistent memory across conversations, allowing it to remember and reference previous information.

**Installation:**
```shell
uv tool install basic-memory
```

**Configuration:**
```json
"basic-memory": {
  "command": "uvx",
  "args": [
    "basic-memory",
    "mcp"
  ]
}
```

**Usage Examples:**
- Write notes: "Create a note about coffee brewing methods"
- Read notes: "What do I know about pour over coffee?"
- Search: "Find information about Ethiopian beans"

## Getting Started

1. Locate your Claude Desktop configuration file
2. Choose your preferred configuration method (NPX or Docker)
3. Add the desired MCP servers to your configuration
4. Restart Claude Desktop to apply the changes
5. Start using the new capabilities in your conversations

These integrations significantly expand Claude's capabilities, making it a powerful assistant for development, research, and productivity tasks.