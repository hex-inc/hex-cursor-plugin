# Hex MCP Plugin

Connect Cursor to your [Hex](https://hex.tech) workspace through the Model Context Protocol (MCP).

> **Note:** Hex MCP Server is currently in beta and requires a Hex Team or Enterprise plan.

## Installation

Install this plugin from the Cursor Marketplace. After installation, you'll be prompted to authenticate with your Hex account via OAuth.

> **Tip:** For single tenant, EU multi-tenant, or HIPAA customers, you may need to configure a custom Hex URL (e.g., `your-company.hex.tech/mcp`, `eu.hex.tech/mcp`, `hc.hex.tech/mcp`).

## What This Plugin Provides

This plugin adds four MCP tools to Cursor:

| Tool | Description |
|------|-------------|
| `search_projects` | Find projects in your Hex workspace |
| `create_thread` | Start a new Thread conversation to ask data questions |
| `get_thread` | Retrieve messages and results from a Thread |
| `continue_thread` | Add follow-up questions to an existing Thread |

## Usage Examples

### Search for Projects

```
Do we have any projects about customer segmentation?
```

Returns project titles, descriptions, summaries, and direct links.

### Ask Data Questions

```
What were our top-selling products last quarter?
```

Creates a Thread that analyzes your data and returns insights, charts, and a link to view the full analysis in Hex.

### Follow-up Questions

```
Can you break that down by sales channel?
```

Continues the existing Thread conversation.

## Requirements

- **Hex Plan**: Team or Enterprise
- **Workspace Role**: Explorer or higher (for `create_thread` and `continue_thread`)
- **Data Connection**: A default data connection must be configured in your Hex workspace

## Limitations

- Only text-based prompts are supported (no file uploads)
- Threads typically take several minutes to complete

## Future Improvements
We are looking to expand our plugin offering, if you have any ideas or feedback, please let us know by emailing support@hex.tech.

## Resources

- [Hex MCP Server Documentation](https://learn.hex.tech/docs/administration/mcp-server)
- [Hex Support](mailto:support@hex.tech)
