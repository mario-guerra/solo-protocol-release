# SOLO Workflow Setup Guide

This guide provides instructions for installing and using the SOLO workflows in various environments.

## 🚀 Quick Install (For AI Agents)

If you are an AI assistant (like Cursor, Claude, or a custom agent), follow these steps to integrate SOLO commands into your workflow:

1.  **Locate Source**: All workflow definitions are in the `/workflows` directory of this project.
2.  **Storage Locations**:
    - **Cursor**: Copy the `.md` files to `.cursor/rules/` or `.cursor/commands/` if using custom slash commands.
    - **VS Code**: Use a "Custom Snippets" or "Command Runner" extension and point it to the `/workflows` folder.
    - **Antigravity**: Copy the `.md` files to your agent's respective workflow or rules directory.
3.  **Command Pattern**: Use the format `/command-name` to invoke the corresponding `command-name.md` file.

## 💻 Manual Setup

### Cursor IDE
To use these as slash commands:
1.  Open Cursor Settings.
2.  Go to **General > Slash Commands**.
3.  Add the path to the `/workflows` folder or individual files.
4.  Alternatively, place them in `.cursor/rules` for global agent context.

### VS Code
1. Install an extension that supports markdown-based workflows (e.g., "Markdown Shortcuts").
2. Link the `/workflows` folder to your project settings.

### Antigravity / Agentic Assistants
1. Point your agent to the `/workflows` folder.
2. Tell the agent: "Apply the workflow definitions in `/workflows` to your internal command set."

---
*Note: The `/workflows` directory is the canonical source of truth for all SOLO protocols.*
