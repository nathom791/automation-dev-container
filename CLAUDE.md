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

# Playwright MCP Server Commands

## Interaction Tools
- `browser_click` - Click on elements
  - element: CSS selector or accessibility label
  - ref: Reference identifier
  - doubleClick: Optional boolean for double-click
- `browser_type` - Type text into elements
  - element: Target element selector
  - ref: Reference identifier
  - text: Text to type
  - submit: Optional boolean to submit form
  - slowly: Optional boolean for slow typing
- `browser_drag` - Drag elements
  - startElement: Source element selector
  - startRef: Source reference
  - endElement: Target element selector
  - endRef: Target reference
- `browser_hover` - Hover over elements
  - element: Element selector
  - ref: Reference identifier
- `browser_select_option` - Select dropdown options
  - element: Select element selector
  - ref: Reference identifier
  - values: Array of option values
- `browser_press_key` - Press keyboard keys
  - key: Key to press (e.g., "Enter", "Escape")

## Navigation Tools
- `browser_navigate` - Navigate to URLs
  - url: Target URL
- `browser_navigate_back` - Go back in browser history
- `browser_navigate_forward` - Go forward in browser history

## Resource Tools
- `browser_take_screenshot` - Take screenshots
  - raw: Optional boolean for raw format
  - filename: Optional output filename
  - element: Optional specific element selector
  - ref: Optional element reference
- `browser_pdf_save` - Save page as PDF
  - filename: Optional output filename

## Utility Tools
- `browser_close` - Close browser
- `browser_resize` - Resize browser window
  - width: Window width in pixels
  - height: Window height in pixels

## Tab Management Tools
- `browser_tab_new` - Open new tab
  - url: Optional URL to load
- `browser_tab_select` - Switch to tab
  - index: Tab index number
- `browser_tab_close` - Close tab
  - index: Optional tab index (current if not specified)

## Operating Modes
- **Snapshot Mode** (default): Uses accessibility tree for fast, structured interactions
- **Vision Mode**: Uses screenshots for visual-based interactions
