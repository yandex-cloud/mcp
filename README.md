# Yandex Cloud MCP Servers

Specialized MCP servers for interacting with the Yandex Cloud using the MCP protocol.

## Table of Contents

- [Yandex Cloud MCP Servers](#yandex-cloud-mcp-servers)
  - [Table of Contents](#table-of-contents)
  - [About Model Context Protocol (MCP)](#about-model-context-protocol-mcp)
  - [YC MCP Servers Concept](#yc-mcp-servers-concept)
  - [Installing MCP Servers](#installing-mcp-servers)
    - [Available MCP Servers](#available-mcp-servers)
    - [Configuration](#configuration)
    - [Authorization](#authorization)
  - [Current Restrictions](#current-restrictions)
    - [Manual Installation](#manual-installation)
    - [Manual IAM token Retrieval](#manual-iam-token-retrieval)
  - [License](#license)

## About Model Context Protocol (MCP)

> MCP (Model Context Protocol) is an open-source standard for connecting AI applications to external systems.
>
> Using MCP, AI applications like Claude or ChatGPT can connect to data sources (e.g. local files, databases), tools (e.g. search engines, calculators) and workflows (e.g. specialized prompts) — enabling them to access key information and perform tasks.
>
> &mdash; [Model Context Protocol documentation (Anthropic)](https://modelcontextprotocol.io/docs/getting-started/intro)

MCP solves a fundamental challenge in AI development: connecting AI assistants to the systems and data they need to be useful. Without MCP, developers must build custom integrations for each AI platform and data source combination. This creates duplicated work and maintenance burden.

MCP provides a universal standard that works across different AI applications. This dramatically reduces development time and makes AI integrations more reliable and maintainable.

## YC MCP Servers Concept

As the adoption of AI continues to grow across all areas of development, we sincerely believe that managing cloud infrastructure with MCP servers is part of the future of cloud management.

MCP servers of cloud providers also help to open the door to the world of cloud technologies for non-tech people, e.g. product manager who wants to validate their new idea but doesn't have sufficient skills - it's never been easier to build and deploy the "proof-of-concept" of any service than by using AI + MCP.

Using provided MCP servers, you can manage Yandex Cloud infrastructure either for "VibeDevOps" and complex AI-automation scenarios, which extremely simplifies working with the cloud infrastructure.

## Installing MCP Servers

### Available MCP Servers

| Server Name | Description | Install |
|-------------|-------------|---------|
| [🛠️ Toolkit MCP Server](./servers/toolkit-mcp-server/README.md)    | Lightweight MCP server to deploy simple applications in Yandex Cloud with Compute, VPC, IAM, Storage (S3) and Managed YDB | [Install](./servers/toolkit-mcp-server/README.md#prerequisites) |
| [📚 Documentation MCP Server](./servers/documentation-mcp-server/README.md) | Real-time free access to official Yandex Cloud documentation using generative search | [Install](./servers/documentation-mcp-server/README.md#prerequisites) |
| [🔍 Yandex Search MCP Server](./servers/search-mcp-server/README.md) | Web search using Yandex Search: both generative and classic | [Install](./servers/search-mcp-server/README.md#prerequisites) |
| [🚀 Functions MCP Server (Preview)](./servers/functions-mcp-server/README.md) | Manage Yandex Cloud Serverless Functions - create, deploy, configure functions and their versions, tags and scaling policies | [Install](./servers/functions-mcp-server/README.md#prerequisites) |
| [📦 Serverless Containers MCP Server (Preview)](./servers/containers-mcp-server/README.md) | Manage Yandex Cloud Serverless Containers - deploy containerized applications with revisions, and scaling policies | [Install](./servers/containers-mcp-server/README.md#prerequisites) |
| [🔌 Triggers MCP Server (Preview)](./servers/triggers-mcp-server/README.md) | Manage event-driven triggers for functions and containers from various sources like timers, message queues, object storage, and IoT | [Install](./servers/triggers-mcp-server/README.md#prerequisites) |
| [⚡ Workflows MCP Server (Preview)](./servers/workflows-mcp-server/README.md) | Create and manage serverless workflows with YAML specifications, executions, and scheduling | [Install](./servers/workflows-mcp-server/README.md#prerequisites) |
| [🌐 API Gateway MCP Server (Preview)](./servers/apigateway-mcp-server/README.md) | Manage API gateways with OpenAPI specifications, custom domains, and WebSocket connections | [Install](./servers/apigateway-mcp-server/README.md#prerequisites) |
| [🔗 MCP Gateway MCP Server (Preview)](./servers/mcpgateway-mcp-server/README.md) | Configure MCP gateways with custom tools that invoke functions, containers, HTTP endpoints, and workflows | [Install](./servers/mcpgateway-mcp-server/README.md#prerequisites) |
| [🗄️ MDB Proxy MCP Server (Preview)](./servers/mdbproxy-mcp-server/README.md) | Manage database proxies for Managed PostgreSQL and ClickHouse clusters | [Install](./servers/mdbproxy-mcp-server/README.md#prerequisites) |
| [🕵️‍♂️ Data Catalog Consumer MCP Server](./servers/datacatalog-consumer-mcp-server/README.md) | Searching tables, views, queries and viewing dependency graphs in a centralized organization metadata repository | [Install](./servers/datacatalog-consumer-mcp-server/README.md#prerequisites) |

### Configuration

To connect MCP servers to your assistant, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding chosen server.

There are two available ways:

#### 1. Directly via streamable http

<details>
<summary>Server configuration template</summary>

```json
{
  "mcpServers": {
    "<server-name>": {
      "type": "streamableHttp",
      "url": "<server-url>",
      "headers": {
        // Server-specific headers, e.g. "Authorization: Bearer <YC IAM token>"
      }
    }
  }
}
```

</details>

#### 2. Using stdio with `npx mcp-remote` client

<details>
<summary>Server configuration template</summary>

```json
{
  "mcpServers": {
    "<server-name>": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "<server-url>",
        "--header" // Server-specific header, e.g. "Authorization:Bearer <YC IAM Token>"
      ]
    }
  }
}
```

</details>

> Specific configurations for each server are located in particular MCP servers documentation, e.g. [this one for Toolkit MCP Server](./servers/toolkit-mcp-server/README.md#configuration).

### Authorization

Most of MCP servers need `Authorization` header to authorize user or system account.

Currently available authorization methods:

1. [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token) with `Authorization: Bearer <IAM token>` header in MCP-server configuration.

2. [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) on the Compute Instance with assigned service account.

## Current Restrictions

### Manual Installation

Temporarily, you have to manually update your MCP configuration in your IDE to install an MCP server.

After we publish our MCP servers in well-known MCP marketplaces (e.g. VS Code MCP Marketplace), you will install an MCP server just in one click.

### Manual IAM token Retrieval

Most of MCP servers need Yandex Cloud IAM token for authorization purposes.

Temporarily, you have to manually retrieve your YC IAM token and specify it in `Authorization` header.

You can get the token using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart) with:

- `yc iam create-token` command for user account

- `yc iam create-token --impersonate-service-account-id <service-account-id>` command for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

> The issue is that IAM token has a maximum lifespan of **12 hours**. After expiration, the token must be manually recreated.

In the near future, we'll implement authorization for MCP servers using [OAuth 2.1](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization).

## License

This project is licensed under the Apache-2.0 License.
