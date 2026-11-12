# Hex MCP plugin

Connect Cursor to your [Hex](https://hex.tech) workspace through the Model Context Protocol (MCP).

> **Note:** The Hex MCP server is in beta and requires a Hex Team or Enterprise plan.

## Installation

Install the [Hex plugin](https://cursor.com/marketplace/hex) from the Cursor Marketplace. If your workspace uses a custom Hex URL, configure it as described below before signing in. Connect to Hex in Cursor and sign in to your Hex account through OAuth.

### Configure a custom Hex URL

The default MCP URL is `https://app.hex.tech/mcp`. If your workspace uses a different address, set **Hex MCP URL** before connecting:

1. In the Cursor dashboard, open **Plugins**, find **Hex**, and select **Configure**.
2. Enter your workspace's full HTTPS MCP URL, including `/mcp`.
3. Save your changes, then connect to Hex in Cursor and sign in to your Hex account.

| Workspace | Example MCP URL |
|-----------|-----------------|
| EU multi-tenant | `https://eu.hex.tech/mcp` |
| HIPAA multi-tenant | `https://hc.hex.tech/mcp` |
| Single-tenant | `https://your-company.hex.tech/mcp` |
| Customer-owned domain | `https://hex.example.com/mcp` |

For team installations, a Cursor team Admin configures this value. See [Cursor's plugin variables documentation](https://cursor.com/docs/reference/plugins#variables) for details.

## What this plugin provides

This plugin adds Hex MCP tools and agent skills to Cursor.

### MCP tools

| Tool | Description |
|------|-------------|
| `search_projects` | Find projects in your Hex workspace |
| `create_thread` | Start a new Thread conversation to ask data questions |
| `get_thread` | Retrieve messages and results from a Thread |
| `continue_thread` | Add follow-up questions to an existing Thread |

### Skills

| Skill | Description |
|-------|-------------|
| `hex-business-analytics-question` | Searches existing Hex projects and uses Hex Agent threads when needed to answer business analytics questions. |
| `hex-notebook-authoring` | Guides local-first Hex notebook and project authoring through the Hex CLI. |
| `hex-to-canvas` | Prompts Hex Threads for exact, source-backed KPI values and uses them to build a Cursor canvas with clear citations and caveats. |

## Usage examples

### Search for projects

```
Do we have any projects about customer segmentation?
```

Returns project titles, descriptions, summaries, and direct links.

### Ask data questions

```
What were our top-selling products last quarter?
```

Creates a Thread that analyzes your data and returns insights, charts, and a link to view the full analysis in Hex.

### Follow-up questions

```
Can you break that down by sales channel?
```

Continues the existing Thread conversation.

### Build a canvas from Hex metrics

```
Use Hex to pull the latest activation KPIs from our onboarding dashboard and build a canvas that compares them with this branch's recent product changes.
```

Uses the `hex-to-canvas` skill to request exact values, periods, rate denominators, source tables, and caveats from Hex before creating a Cursor canvas.

## Requirements

- **Hex plan**: Team or Enterprise
- **Workspace role**: Explorer or higher (for `create_thread` and `continue_thread`)
- **Data connection**: A default data connection must be configured in your Hex workspace

## Limitations

- Only text-based prompts are supported (no file uploads)
- Threads typically take several minutes to complete

## Resources

- [Hex MCP server documentation](https://learn.hex.tech/docs/api-integrations/mcp-server)
- [Hex support](mailto:support@hex.tech)
