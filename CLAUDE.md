// .devcontainer/devcontainer.json
{
  "name": "Claude + Bitwarden + Playwright MCP Servers Dev Container",
  "image": "mcr.microsoft.com/devcontainers/python:3.12",
  "features": {
    // add Node 22 for both MCP servers
    "ghcr.io/devcontainers/features/node:1": {
      "version": "22"
    }
  },
  // mount your repo at /app so all code lives together
  "workspaceFolder": "/app",
  "workspaceMount": "source=${localWorkspaceFolder},target=/app,type=bind,consistency=cached",

  // use root so VS Code can forward ports and install global tools
  "remoteUser": "root",

  "settings": {
    "terminal.integrated.shell.linux": "/bin/bash",
    "typescript.tsdk": "node_modules/typescript/lib"
  },

  "extensions": [
    // Claude CLI support
    "ms-python.python",
    "ms-python.pylint",
    // JS tooling
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    // Playwright Test for VS Code
    "ms-playwright.playwright"
  ],

  // Bitwarden MCP UI + Playwright MCP SSE ports
  "forwardPorts": [
    3000,
    8931
  ],

  // After container creation:
  // 1. Install Claude CLI, Bitwarden CLI, MCP servers, and Playwright MCP
  // 2. Install & build your local code
  // 3. Log in to Bitwarden CLI non-interactively and persist the session
  "postCreateCommand": "npm install -g @anthropic-ai/claude-code @bitwarden/cli @bitwarden/mcp-server @playwright/mcp && npm install && npm run build && SESSION=$(bw login $BITWARDEN_EMAIL $BITWARDEN_PASSWORD --raw) && echo \"export BW_SESSION=$SESSION\" >> /root/.bashrc",

  // Source the session on each new shell
  "postStartCommand": "source /root/.bashrc",

  "containerEnv": {
    "NODE_ENV": "development",
    // Define these in your host env or in a .env file so VS Code will prompt you
    "BITWARDEN_EMAIL": "${localEnv:BITWARDEN_EMAIL}",
    "BITWARDEN_PASSWORD": "${localEnv:BITWARDEN_PASSWORD}"
  }
}

# Testing Instructions
Keep going until all the test cases in TESTCASES.md have been executed
Provide a report at the end of test execution complete with screenshots and exact test duration per test case
