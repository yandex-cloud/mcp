# Yandex Cloud Documentation MCP Server

Real-time free access to official Yandex Cloud documentation using generative search.

## Table of Contents

- [Yandex Cloud Documentation MCP Server](#yandex-cloud-documentation-mcp-server)
  - [Table of Contents](#table-of-contents)
  - [Use Cases](#use-cases)
  - [Installation and Usage](#installation-and-usage)
    - [Configuration](#configuration)
      - [Streamable HTTP](#streamable-http)
      - [NPM Client](#npm-client)
  - [Tools](#tools)

## Use Cases

Prompts examples:

- How to install YC CLI?
- How to configure Terraform for Yandex Cloud provider?
- How to start working with YC Managed k8s?
- What is a 'service account' in Yandex Cloud?

## Installation and Usage

### Configuration

To start working with Yandex Cloud Documentation MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-documentation` server.

There are two available ways:

#### Streamable HTTP

**Configuration:**

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

#### NPM Client

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Node.js 18.0.0 or higher

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-documentation": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "docs",
        "--no-auth"
      ]
    }
  }
}
```

## Tools

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
