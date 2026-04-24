# Yandex Cloud MCP Servers

Specialized MCP servers for interacting with Yandex Cloud using the MCP protocol.

## Table of Contents

- [Yandex Cloud MCP Servers](#yandex-cloud-mcp-servers)
  - [Table of Contents](#table-of-contents)
  - [About Model Context Protocol (MCP)](#about-model-context-protocol-mcp)
  - [YC MCP Servers Concept](#yc-mcp-servers-concept)
  - [Installing MCP Servers](#installing-mcp-servers)
    - [Available MCP Servers](#available-mcp-servers)
      - [Deployment](#deployment)
      - [Search & Knowledge](#search--knowledge)
      - [Serverless](#serverless)
      - [Data Platform](#data-platform)
    - [Configuration](#configuration)
      - [NPM Client (Stdio)](#1-npm-client-stdio)
      - [Streamable HTTP](#2-streamable-http)
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

#### Deployment

| Server Name | Description | Install |
| ------------- | ------------- | --------- |
| [🛠️ Toolkit MCP Server](./servers/toolkit-mcp-server/README.md) | MCP server to deploy simple applications in Yandex Cloud with Compute, VPC, IAM, Storage (S3) and Managed YDB | [Install](./servers/toolkit-mcp-server/README.md#configuration) |

#### Search & Knowledge

| Server Name | Description | Install |
| ------------- | ------------- | --------- |
| [📚 Documentation MCP Server](./servers/documentation-mcp-server/README.md) | Real-time free access to official Yandex Cloud documentation using generative search | [Install](./servers/documentation-mcp-server/README.md#configuration) |
| [🔍 Yandex Search MCP Server](./servers/search-mcp-server/README.md) | Web search using Yandex Search: both generative and classic | [Install](./servers/search-mcp-server/README.md#configuration) |

#### Serverless

| Server Name | Description | Install |
| ------------- | ------------- | --------- |
| [🚀 Functions MCP Server (Preview)](./servers/functions-mcp-server/README.md) | Manage Yandex Cloud Serverless Functions - create, deploy, configure functions and their versions, tags and scaling policies | [Install](./servers/functions-mcp-server/README.md#configuration) |
| [📦 Serverless Containers MCP Server](./servers/containers-mcp-server/README.md) | Manage Yandex Cloud Serverless Containers - deploy containerized applications with revisions, scaling policies, and registry management | [Install](./servers/containers-mcp-server/README.md#configuration) |
| [🔌 Triggers MCP Server (Preview)](./servers/triggers-mcp-server/README.md) | Manage event-driven triggers for functions and containers from various sources like timers, message queues, object storage, and IoT | [Install](./servers/triggers-mcp-server/README.md#configuration) |
| [⚡ Workflows MCP Server (Preview)](./servers/workflows-mcp-server/README.md) | Create and manage serverless workflows with YAML specifications, executions, and scheduling | [Install](./servers/workflows-mcp-server/README.md#configuration) |
| [🌐 API Gateway MCP Server (Preview)](./servers/apigateway-mcp-server/README.md) | Manage API gateways with OpenAPI specifications, custom domains, and WebSocket connections | [Install](./servers/apigateway-mcp-server/README.md#configuration) |
| [🔗 MCP Gateway MCP Server (Preview)](./servers/mcpgateway-mcp-server/README.md) | Configure MCP gateways with custom tools that invoke functions, containers, HTTP endpoints, and workflows | [Install](./servers/mcpgateway-mcp-server/README.md#configuration) |

#### Data Platform

| Server Name | Description | Install |
| ------------- | ------------- | --------- |
| [🕵️‍♂️ Data Catalog Consumer MCP Server](./servers/datacatalog-consumer-mcp-server/README.md) | Searching tables, views, queries and viewing dependency graphs in a centralized organization metadata repository | [Install](./servers/datacatalog-consumer-mcp-server/README.md#configuration) |

### Configuration

To connect your assistant with MCP servers, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding chosen server.

Most MCP servers need to be authorized in Yandex Cloud. There are several ways to configure and authorize:

#### 1. NPM Client (Stdio)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Server-specific Yandex Cloud [roles](https://yandex.cloud/en/docs/iam/concepts/access-control/roles).
- Node.js 18.0.0 or higher
- (Optional) [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart) (`yc`) - required only when using CLI authentication

**Authentication Options:**

- **OAuth (default, recommended)**: Run the package without authentication flags. Browser-based authentication is used automatically.
- **OAuth with explicit account selection**: Use `-S <user or service account ID>` or `-u <email>`
- **CLI**: Use `-p <profile>` to authenticate via Yandex Cloud CLI (requires CLI installation)

**Configuration example:**

```json
{
  "mcpServers": {
    "yandex-cloud-toolkit": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "toolkit"
      ]
    }
  }
}
```

> Here `toolkit` stands for the server name, e.g. `search`, `functions`, `docs`. You can find the particular server's name in its npm client configuration section, e.g. [this one](./servers/search-mcp-server/README.md#npm-client-recommended) for Yandex Search MCP Server.

#### 2. Streamable HTTP

**Prerequisites:**

- Server-specific Yandex Cloud [roles](https://yandex.cloud/en/docs/iam/concepts/access-control/roles).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration example:**

```json
{
  "mcpServers": {
    "yandex-cloud-toolkit": {
      "type": "streamableHttp",
      "url": "https://toolkit.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>"
      }
    }
  }
}
```

> You can find the particular server's url in its streamable http configuration section, e.g. [this one](./servers/search-mcp-server/README.md#streamable-http) for Yandex Search MCP Server.

## License

This project is licensed under the Apache-2.0 License.
