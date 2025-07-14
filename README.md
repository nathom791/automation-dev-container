# claude-in-docker

empty project for a quick setup of Claude in Docker+VSCode

Motivation + bonus step-by-step github authorization setting here: [timsh.org](https://timsh.org/claude-inside-docker/)

# Prerequisites:

- Docker installed
- VSCode installed
- Claude somehow paid for

# Steps

- `git clone https://github.com/tim-sha256/claude-in-docker.git`
- `cd claude-in-docker`
- `code` (or open the folder manually in VSCode)
- a popup in the rear right corner should appear offering to "Reopen in Container" - do it!
- wait for a bit...
- in the automatically opened terminal tab you'll see that you're inside docker
- type `claude` to activate Claude Code

You're all set!

## Bitwarden MCP Server Setup

The container includes the Bitwarden CLI and MCP server for secure password management integration. To use it:

### 1. Authenticate with Bitwarden CLI

After the container is running, authenticate with your Bitwarden account:

```bash
# Set your Bitwarden server (use vault.bitwarden.com for cloud)
bw config server https://vault.bitwarden.com

# Login to your account
bw login

# Unlock your vault (you'll need to do this each session)
bw unlock
```

### 2. Set Session Key

After unlocking, Bitwarden will provide a session key. Export it as an environment variable:

```bash
export BW_SESSION="your_session_key_here"
```

### 3. Verify Setup

Test that everything is working:

```bash
# List your vault items
bw list items

# Sync your vault
bw sync
```

**Note:** You'll need to unlock your vault and set the session key each time you start a new container session. For security, the session key is not persisted between container restarts.

## Available MCP Servers

This container includes the following MCP servers:

- **Bitwarden**: Access your password vault securely
- **Playwright**: Web automation and testing capabilities

Both servers are automatically configured and available when you start Claude Code.
