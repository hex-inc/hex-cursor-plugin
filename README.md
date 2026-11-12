# Hex MCP plugin

Connect Cursor to your [Hex](https://hex.tech) workspace to find existing analyses, ask questions about your data, and build notebooks and data apps.

## Installation

1. Install the [Hex plugin](https://cursor.com/marketplace/hex) from the Cursor Marketplace.
2. If your workspace uses a custom Hex URL, [configure the endpoint](#configure-a-custom-hex-url) before connecting.
3. Connect to Hex in Cursor and sign in to your Hex account through OAuth. If you belong to multiple workspaces, select the one you want to use.

### Configure a custom Hex URL

The default MCP URL is `https://app.hex.tech/mcp`. If your workspace uses a different address:

1. In the Cursor dashboard, open **Plugins**, find **Hex**, and select **Configure**.
2. Set **Hex MCP URL** to your workspace's full HTTPS MCP URL, including `/mcp`.
3. Save your changes, then connect to Hex in Cursor.

| Workspace | Example MCP URL |
|-----------|-----------------|
| EU multi-tenant | `https://eu.hex.tech/mcp` |
| HIPAA multi-tenant | `https://hc.hex.tech/mcp` |
| Single-tenant | `https://your-company.hex.tech/mcp` |
| Customer-owned domain | `https://hex.example.com/mcp` |

For team installations, a Cursor team Admin configures this value. See [Cursor's plugin variables documentation](https://cursor.com/docs/reference/plugins#variables) for details.

## Use Hex in Cursor

Start a new chat and ask Cursor to use Hex. For example:

- "Find our customer retention dashboard in Hex."
- "Use Hex to analyze sales by region for last quarter."
- "Add a monthly revenue chart to this Hex notebook: `<project URL>`."
- "Build a Cursor canvas with the activation KPIs from our Hex onboarding dashboard."

The Hex MCP server supplies Cursor with tools and instructions for using them. Available actions depend on your workspace's enabled features and your permissions. See the [Hex MCP server documentation](https://learn.hex.tech/docs/api-integrations/mcp-server) for details.

## Hex to canvas

The plugin includes the [Hex to canvas skill](skills/hex-to-canvas/SKILL.md) to build Cursor canvases from metrics in Hex.

## Access and permissions

Your Hex workspace role, project permissions, and data connection access apply when you use Hex from Cursor. Your workspace's settings also determine which features are available.

## Resources

- [Hex MCP server documentation](https://learn.hex.tech/docs/api-integrations/mcp-server)
- [Hex support](mailto:support@hex.tech)
