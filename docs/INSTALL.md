# BetterCallClaude Installation Guide

**Version 3.1.0** -- Swiss Legal Intelligence Plugin for Claude Code and Cowork Desktop

This guide covers installation, configuration, and verification for both platforms.

---

## Contents

1. [Before You Begin](#1-before-you-begin)
2. [Claude Code CLI Installation](#2-claude-code-cli-installation)
3. [Cowork Desktop Installation](#3-cowork-desktop-installation)
4. [Team Setup](#4-team-setup)
5. [Manual / Developer Installation](#5-manual--developer-installation)
6. [MCP Server Reference](#6-mcp-server-reference)
7. [Troubleshooting](#7-troubleshooting)
8. [Upgrading and Uninstalling](#8-upgrading-and-uninstalling)

---

## 1. Before You Begin

### Prerequisites

| Requirement | Details |
|-------------|---------|
| **Claude Code CLI** or **Cowork Desktop** | Latest version of either platform |
| **Node.js** | Version 18 or later (required for MCP servers) |

No Python installation, API keys, or external accounts are required.

### Which Platform Are You Using?

- **Claude Code CLI**: A command-line tool you run in your terminal (macOS, Linux, Windows). If you installed Claude Code with `npm`, `brew`, or one of the Anthropic install scripts, you are using the CLI. Go to [Part 2](#2-claude-code-cli-installation).

- **Cowork Desktop**: A desktop application with a graphical interface that includes Claude Code in a sandboxed virtual machine. If you downloaded Cowork from Anthropic's website and it runs as a standalone app with a sidebar, you are using Cowork. Go to [Part 3](#3-cowork-desktop-installation).

- **Developer / contributor**: If you want to build from source or run the plugin from a local clone. Go to [Part 5](#5-manual--developer-installation).

---

## 2. Claude Code CLI Installation

### 2.1 Install the Plugin

Open your terminal and run:

```
claude plugin marketplace add fedec65/bettercallclaude
claude plugin install bettercallclaude@bettercallclaude-marketplace
```

### 2.2 Verify the Installation

1. Restart Claude Code (quit and reopen).
2. Run the version command:

   ```
   /bettercallclaude:version
   ```

   You should see a status report listing 18 commands, 19 agents, 10 skills, and up to 6 MCP servers.

3. Run the setup command to check MCP server connectivity:

   ```
   /bettercallclaude:setup
   ```

   This probes 5 of the 6 MCP servers (the ollama server is local-only and not probed). Each server is reported as connected or not configured.

4. Test a command:

   ```
   /bettercallclaude:help
   ```

### 2.3 Windows Notes

Claude Code on Windows requires [Git for Windows](https://git-scm.com/downloads/win). Install Git first, then install Claude Code using one of these methods:

**PowerShell:**

```powershell
irm https://claude.ai/install.ps1 | iex
```

**CMD:**

```batch
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

**WinGet:**

```powershell
winget install Anthropic.ClaudeCode
```

After Claude Code is installed, install the plugin using the same two commands from Section 2.1:

```
claude plugin marketplace add fedec65/bettercallclaude
claude plugin install bettercallclaude@bettercallclaude-marketplace
```

You can launch `claude` from PowerShell, CMD, or Git Bash. Administrator privileges are not required.

**WSL users**: Both WSL 1 and WSL 2 are supported. Inside your WSL terminal, install Claude Code with `curl -fsSL https://claude.ai/install.sh | bash`, then install the plugin as above.

**Git Bash not found?** If Claude Code cannot locate your Git Bash installation, add this to your `settings.json`:

```json
{ "env": { "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe" } }
```

### 2.4 Linux Notes

Install Claude Code using the official install script:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Then install the plugin with the same two commands from Section 2.1. No platform-specific adjustments are needed.

---

## 3. Cowork Desktop Installation

### 3.1 Install the Plugin

Open Cowork's built-in terminal (click **Terminal** in the bottom bar) and run:

```
claude plugin marketplace add fedec65/bettercallclaude
claude plugin install bettercallclaude@bettercallclaude-marketplace
```

**Why the terminal?** The Cowork GUI "Add marketplace from GitHub" dialog has a known networking issue that prevents it from reaching GitHub from within the sandboxed VM ([#26951](https://github.com/anthropics/claude-code/issues/26951), [#28125](https://github.com/anthropics/claude-code/issues/28125), [#28853](https://github.com/anthropics/claude-code/issues/28853)). The terminal method works reliably.

### 3.2 The VM Sandbox Problem

Cowork Desktop runs inside a sandboxed virtual machine. This sandbox prevents MCP servers from making outbound network requests to external APIs. Five of the six MCP servers require internet access to reach Swiss legal databases.

| Server | Needs Internet | Works Inside Cowork VM | Purpose |
|--------|:-:|:-:|---------|
| entscheidsuche | Yes | No | Swiss court decision search |
| bge-search | Yes | No | Federal Supreme Court decisions |
| legal-citations | Partial | Partial | Citation formatting and verification |
| fedlex-sparql | Yes | No | Federal legislation database |
| onlinekommentar | Yes | No | Legal commentary access |
| ollama | No | Yes | Privacy classification (offline regex) |

The ollama server works entirely offline using built-in regex patterns for privacy classification. It does not require Ollama to be installed and does not make any network calls.

To get full MCP server access in Cowork, the servers must be installed at the **Claude Desktop host level** (outside the VM). The setup command handles this automatically when possible.

### 3.3 Configure MCP Servers

Run the setup command inside Cowork:

```
/bettercallclaude:setup
```

The setup command follows this sequence:

1. **Probes each server** to check current connectivity (5 servers tested).
2. **Displays a status table** showing which servers are connected and which are missing.
3. **Detects the environment** by checking for the `mcp__Control_your_Mac__osascript` tool.
4. **Installs servers automatically** using the best available method:

   - **If osascript is available** (Cowork with "Control your Mac" enabled): The command uses AppleScript to copy server files to a stable location on the host Mac (`~/.claude/bettercallclaude-servers/`) and writes the server configuration into Claude Desktop's `claude_desktop_config.json`. A backup of your existing config is created automatically. After installation, restart Claude Desktop and re-run `/bettercallclaude:setup` to verify.

   - **If osascript is not available**: The command attempts direct file system access to write the Claude Desktop config. If this succeeds, restart Claude Desktop and re-run `/bettercallclaude:setup`.

   - **If both methods fail**, the command provides three manual fallback options:
     - **MCPB bundles**: Download `.mcpb` files from the [latest release](https://github.com/fedec65/BetterCallClaude_Marketplace/releases/latest) and double-click each one to register it with Claude Desktop.
     - **Install script**: Clone the repository on your host machine and run `bash scripts/install-claude-desktop.sh`.
     - **Manual config**: Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `~/.config/Claude/claude_desktop_config.json` (Linux) directly. See the CONNECTORS.md file in the plugin for the full JSON.

### 3.4 Verify the Cowork Installation

After configuring MCP servers and restarting Claude Desktop:

1. Open a new Cowork session.
2. Run `/bettercallclaude:setup` and confirm all 5 servers show as connected.
3. Run `/bettercallclaude:version` to confirm the full component list.
4. Test a research command:

   ```
   /bettercallclaude:research Art. 97 OR contractual liability
   ```

### 3.5 Working Without MCP Servers

If MCP server configuration is not possible or not needed, BetterCallClaude still works in **reduced mode**:

- All 18 commands, 19 agents, and 10 skills remain fully functional.
- Legal analysis, strategy, drafting, translation, and adversarial analysis work using Claude's built-in Swiss law knowledge.
- The privacy classification tool (ollama server) works inside the VM.

What reduced mode cannot do:

- Search live court decision databases (entscheidsuche, bge-search).
- Verify citation existence against live databases.
- Query current federal legislation via SPARQL (fedlex-sparql).
- Access legal commentaries (onlinekommentar).

For many use cases (strategy development, document drafting, legal translation, adversarial analysis), reduced mode is fully sufficient.

---

## 4. Team Setup

To ensure every team member who clones your project repository gets prompted to install BetterCallClaude automatically, add the following to `.claude/settings.json` in your project root:

```json
{
  "extraKnownMarketplaces": {
    "bettercallclaude-marketplace": {
      "source": {
        "source": "github",
        "repo": "fedec65/bettercallclaude"
      }
    }
  }
}
```

Commit this file to your repository. When a team member opens the project with Claude Code or Cowork, they will be prompted to install the plugin.

Each team member still needs to:

1. Accept the plugin installation prompt (or run the two `claude plugin` commands manually).
2. Run `/bettercallclaude:setup` individually to configure MCP servers for their own environment.

MCP server configuration is per-machine and cannot be shared through the repository.

---

## 5. Manual / Developer Installation

### 5.1 Run from a Local Clone

```bash
git clone https://github.com/fedec65/bettercallclaude.git
cd bettercallclaude
claude --plugin-dir bettercallclaude/
```

The `bettercallclaude/` subdirectory is the plugin root. It contains all agents, commands, skills, hooks, MCP server bundles, and the `.mcp.json` configuration.

### 5.2 Build MCP Servers from Source

The `mcp-servers-src/` directory contains TypeScript source for all six MCP servers. Pre-compiled bundles are included in `bettercallclaude/mcp-servers/*/dist/`, so building from source is only necessary if you are modifying server code.

```bash
# Install dependencies
npm install

# Compile TypeScript and bundle into bettercallclaude/mcp-servers/*/dist/
npm run build:bundle

# Run tests
npm test

# Create distributable plugin zip
npm run package

# Create .mcpb bundles for Claude Desktop
npm run build:mcpb
```

### 5.3 Repository Structure

```
bettercallclaude/              Plugin root (single source of truth)
  .claude-plugin/              Plugin manifest
  .mcp.json                    MCP server configuration
  agents/                      19 agent definitions (markdown)
  commands/                    18 slash commands (markdown)
  skills/                      10 auto-activated skills (markdown)
  hooks/                       Privacy detection hook
  mcp-servers/                 Pre-compiled MCP server bundles
mcp-servers-src/               TypeScript source for MCP servers
  shared/                      Shared infrastructure (database, HTTP, NLP)
  entscheidsuche/              Swiss court decision search
  bge-search/                  Federal Supreme Court search
  legal-citations/             Citation verification and formatting
  fedlex-sparql/               Federal legislation via SPARQL
  onlinekommentar/             Legal commentaries
  ollama/                      Privacy classification (offline)
  integration-tests/           Cross-server integration tests
scripts/                       Build and installation scripts
docs/                          Documentation
```

---

## 6. MCP Server Reference

BetterCallClaude includes six pre-compiled MCP servers. All servers are self-contained Node.js applications with no external dependencies beyond `@modelcontextprotocol/sdk`. No API keys or external accounts are required.

### Server Details

| Server | Description | External API | Network Required |
|--------|-------------|--------------|:---:|
| **entscheidsuche** | Searches Swiss court decisions across federal and cantonal courts via entscheidsuche.ch. Supports keyword search, language filtering, and court-specific queries. | entscheidsuche.ch | Yes |
| **bge-search** | Searches Federal Supreme Court (BGE/ATF/DTF) decisions. Supports keyword search, article reference filtering, date ranges, and section filtering. | bger.ch | Yes |
| **legal-citations** | Validates citation format and existence. Converts citations between DE/FR/IT/EN formats (BGE/ATF/DTF). | Partial (format validation is local; existence checks require network) | Partial |
| **fedlex-sparql** | Queries Swiss federal legislation via the Fedlex SPARQL endpoint. Retrieves statutes by SR number, searches legislation, finds related acts, gets article text. Has built-in retry logic (3 attempts, 2-second delay). | fedlex.data.admin.ch | Yes |
| **onlinekommentar** | Searches Swiss legal commentaries (Kommentare). Finds scholarly analysis by article reference, keyword, or legislative act. | onlinekommentar.ch | Yes |
| **ollama** | Privacy classification using offline regex patterns. Detects privileged (PRIVILEGED) and confidential (CONFIDENTIAL) content in German, French, and Italian. Does not require Ollama to be installed. | None | No |

### How Servers Register

Server registration depends on the platform:

- **Claude Code CLI**: Servers auto-register from the plugin's `.mcp.json` file. The file uses `${CLAUDE_PLUGIN_ROOT}` for portable paths. No manual configuration is needed.

- **Cowork Desktop**: The `/bettercallclaude:setup` command detects the environment and installs servers to the host-level Claude Desktop configuration. See [Section 3.3](#33-configure-mcp-servers).

- **Claude Desktop (standalone)**: Servers must be registered in `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `~/.config/Claude/claude_desktop_config.json` (Linux). The setup command, MCPB bundles, or the install script handle this. See the CONNECTORS.md file for the full JSON configuration.

---

## 7. Troubleshooting

### Plugin Not Loading

1. Confirm the plugin is installed:

   ```
   claude plugin list
   ```

   You should see `bettercallclaude` in the output.

2. If not listed, re-run the installation commands:

   ```
   claude plugin marketplace add fedec65/bettercallclaude
   claude plugin install bettercallclaude@bettercallclaude-marketplace
   ```

3. Restart Claude Code or Cowork after installation.

### Commands Not Recognized

All BetterCallClaude commands use the `/bettercallclaude:` prefix. For example:

```
/bettercallclaude:help
/bettercallclaude:research Art. 97 OR
/bettercallclaude:version
```

If commands are not recognized after installation, restart Claude Code or Cowork.

### MCP Servers Not Connecting

1. **Check Node.js version**: Run `node --version` in your terminal. Version 18 or later is required.

2. **Cowork VM sandbox**: If you are using Cowork Desktop, MCP servers that require internet access cannot connect from inside the VM. Run `/bettercallclaude:setup` to install servers at the host level. See [Section 3.2](#32-the-vm-sandbox-problem).

3. **ECONNREFUSED errors**: The server process started but cannot reach the external API. This typically indicates a network or firewall issue. Verify that your machine can reach the relevant domain (e.g., `curl https://entscheidsuche.ch`).

4. **Run setup again**: After any configuration change, run `/bettercallclaude:setup` to verify the current state.

### Cowork GUI Install Fails

The "Add marketplace from GitHub" dialog in Cowork has a known networking bug ([#26951](https://github.com/anthropics/claude-code/issues/26951), [#28125](https://github.com/anthropics/claude-code/issues/28125), [#28853](https://github.com/anthropics/claude-code/issues/28853)). Use the terminal method instead:

1. Open Cowork's built-in terminal (click **Terminal** in the bottom bar).
2. Run the two `claude plugin` commands from [Section 3.1](#31-install-the-plugin).

### SPARQL / Fedlex Timeout

The fedlex-sparql server queries `fedlex.data.admin.ch`, which is an external government service. Timeouts or HTTP 5xx errors indicate the Fedlex service is temporarily unavailable. The server has built-in retry logic (3 attempts with a 2-second delay). If the problem persists, try again later. This is not a plugin issue.

### Windows-Specific Issues

- **Git Bash path**: If Claude Code cannot find Git Bash, set the path in `settings.json`:

  ```json
  { "env": { "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe" } }
  ```

- **Execution policy**: If PowerShell blocks the install script, you may need to run `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` first. This is a one-time Windows configuration.

- **WSL**: Both WSL 1 and WSL 2 are supported. Install Claude Code inside WSL using the bash install script, then install the plugin as normal.

---

## 8. Upgrading and Uninstalling

### Upgrading

To update to the latest version:

```
claude plugin update
```

After updating, run `/bettercallclaude:version` to confirm the new version number.

If you configured MCP servers for Cowork Desktop, re-run `/bettercallclaude:setup` after upgrading to ensure server binaries are up to date.

### Uninstalling

To remove the plugin:

```
claude plugin uninstall bettercallclaude
```

If you installed MCP servers at the Claude Desktop host level (Cowork users), you may also want to:

1. Remove the server files: `rm -rf ~/.claude/bettercallclaude-servers/`
2. Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `~/.config/Claude/claude_desktop_config.json` (Linux) and remove the five `bettercallclaude-*` entries from the `mcpServers` object.

---

## Further Resources

- [Command Reference](command-reference.md) -- Full list of all commands with usage examples.
- [CONNECTORS.md](../CONNECTORS.md) -- Detailed MCP server API documentation.
- [README](../README.md) -- Project overview, component list, and quick start.
- [GitHub Issues](https://github.com/fedec65/bettercallclaude/issues) -- Report bugs or request features.

---

**BetterCallClaude v3.1.0** -- Swiss Legal Intelligence Plugin

All outputs produced by this plugin require professional lawyer review and validation before use. This tool assists legal professionals but does not replace professional judgment or the duty of care owed to clients.
