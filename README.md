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
