# BetterCallClaude — Development Setup Guide

> Version: v3.1.0 | [GitHub](https://github.com/fedec65/bettercallclaude)

## Project Overview

BetterCallClaude is a Claude Code plugin for Swiss legal intelligence. It consists of:
- **Plugin definitions** (`bettercallclaude/`): Markdown-based agent, command, and skill specifications
- **MCP server source** (`mcp-servers-src/`): TypeScript implementations
- **MCP server builds** (`mcp-servers/`): Pre-compiled JavaScript bundles (checked into git)

## Prerequisites

- [Claude Code](https://claude.ai/code) (latest version)
- Node.js 18+ and npm (for MCP server development only)
- TypeScript (for MCP server development only)
- Git

## Project Structure

```
BetterCallClaude/
├── bettercallclaude/           # Plugin root (installed by Claude Code)
│   ├── agents/                 # 18 specialized legal agent definitions (.md)
│   ├── commands/               # 17 slash command definitions (.md)
│   ├── skills/                 # 10 skill definitions (subdirectories with SKILL.md)
│   ├── .mcp.json              # MCP server configuration
│   └── README.md              # Plugin README
├── mcp-servers-src/            # TypeScript MCP server source code
│   ├── bge-search/            # BGE court decision search
│   ├── entscheidsuche/        # Entscheidsuche.ch integration
│   ├── fedlex-sparql/         # Federal law SPARQL queries
│   ├── legal-citations/       # Citation parsing and formatting
│   └── onlinekommentar/       # Legal commentary search
├── mcp-servers/                # Pre-compiled MCP server bundles
│   ├── bge-search/dist/
│   ├── entscheidsuche/dist/
│   ├── fedlex-sparql/dist/
│   ├── legal-citations/dist/
│   └── onlinekommentar/dist/
├── docs/                       # Documentation
└── README.md                   # Repository README
```

## Development Workflow

### Plugin Development (Agents, Commands, Skills)

Plugin definitions are plain markdown files with YAML frontmatter. No build step required.

1. Edit markdown files in `bettercallclaude/agents/`, `commands/`, or `skills/`
2. Test by using the commands in Claude Code directly
3. Changes take effect immediately on next Claude Code invocation

### MCP Server Development

MCP servers are TypeScript projects built with npm.

```bash
# Navigate to a server source directory
cd mcp-servers-src/<server-name>

# Install dependencies
npm install

# Build
npm run build

# The compiled output goes to bettercallclaude/mcp-servers/<server-name>/dist/
```

### Common Tasks

| Task | Command |
|------|---------|
| Build an MCP server | `cd mcp-servers-src/<name> && npm run build` |
| Add a new agent | Create `bettercallclaude/agents/<name>.md` |
| Add a new command | Create `bettercallclaude/commands/<name>.md` |
| Add a new skill | Create `bettercallclaude/skills/<name>/SKILL.md` |
| Test a command | Use `/legal-<name>` in Claude Code |

## Notes

- MCP server builds (`mcp-servers/*/dist/`) are checked into git for distribution — end users don't need to build
- Plugin definitions use YAML frontmatter for metadata (name, description, tools) and markdown body for content
- The `shared/` directory in `mcp-servers-src/` contains common utilities (database, types)
