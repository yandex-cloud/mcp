# Yandex Cloud Documentation MCP Server

Real-time free access to official Yandex Cloud documentation using generative search.

## Use Cases

Prompts examples:

- How to install YC CLI?
- How to configure Terraform for Yandex Cloud provider?
- How to start working with YC Managed k8s?
- What is a 'service account' in Yandex Cloud?

## Installation and Usage

### Prerequisites

No authorization credentials needed.

### Configuration

To start working with Yandex Cloud Documentation MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-documentation` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-documentation": {
      "type": "streamableHttp",
      "url": "https://docs.mcp.cloud.yandex.net/mcp"
    }
  }
}
```

2. Using stdio with `npx mcp-remote` client

```json
{
  "mcpServers": {
    "yandex-cloud-documentation": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://docs.mcp.cloud.yandex.net/mcp"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud Documentation MCP Server currently consists of one tool shown below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> documentation_generative_search </td>
    <td> Searches Yandex Cloud documentation with generative search </td>
  </tr>
</table>
