# Yandex Cloud MCP Gateway MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud MCP Gateway - create and configure MCP gateways with custom tools that can invoke functions, containers, HTTP endpoints, workflows, and other MCP servers.

## Table of Contents

- [Yandex Cloud MCP Gateway MCP Server (Preview)](#yandex-cloud-mcp-gateway-mcp-server-preview)
  - [Table of Contents](#table-of-contents)
  - [Use Cases](#use-cases)
  - [Installation and Usage](#installation-and-usage)
    - [Configuration](#configuration)
      - [NPM Client (recommended)](#1-npm-client-recommended)
      - [Streamable HTTP](#2-streamable-http)
    - [Headers](#headers)
  - [Tools](#tools)

## Use Cases

Prompts examples:

- List all MCP gateways in my folder
- Create a new MCP gateway with custom tools
- Configure tool to call serverless function
- Set up tool to invoke HTTP endpoint
- Update MCP gateway with new tool definitions
- Configure gateway to be publicly accessible
- Manage access bindings for MCP gateway
- Get MCP gateway operations history

## Installation and Usage

### Configuration

To start working with Yandex Cloud MCP Gateway MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-mcpgateway` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary roles (e.g., `editor` or `serverless.mcpGateways.admin`).
- Node.js 18.0.0 or higher
- (Optional) [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart) (`yc`) - required only when using CLI authentication

**Authentication Options:**

- **OAuth (default, recommended)**: Run the package without authentication flags. Browser-based authentication is used automatically.
- **OAuth with explicit account selection**: Use `-S <user or service account ID>` or `-u <email>`
- **CLI**: Use `-p <profile>` to authenticate via Yandex Cloud CLI (requires CLI installation)

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-mcpgateway": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "mcpgateway",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary roles (e.g., `editor` or `serverless.mcpGateways.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-mcpgateway": {
      "type": "streamableHttp",
      "url": "https://mcpgateway.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Folder-Id": "<Folder ID (optional)>"
      }
    }
  }
}
```

### Headers

| Header | Description | Requireness |
| ------------- | ------------- | --------- |
| Folder-Id | Yandex Cloud folder as default value for MCP tool's input field `folder_id` | Optional |
| Authorization | Yandex Cloud IAM Token for Streamable HTTP authorization | Required for Streamable HTTP |

## Tools

Yandex Cloud MCP Gateway MCP Server currently consists of 7 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> mcp_gateway_get </td>
    <td> Get Yandex Cloud MCP Gateway </td>
  </tr>
  <tr>
    <td> mcp_gateways_list </td>
    <td> List Yandex Cloud MCP Gateway in the folder </td>
  </tr>
  <tr>
    <td> mcp_gateway_create </td>
    <td> Create Yandex Cloud MCP Gateway </td>
  </tr>
  <tr>
    <td> mcp_gateway_update </td>
    <td> Update Yandex Cloud MCP Gateway </td>
  </tr>
  <tr>
    <td> mcp_gateway_delete </td>
    <td> Delete Yandex Cloud MCP Gateway </td>
  </tr>
  <tr>
    <td> mcp_gateway_operations_list </td>
    <td> List Yandex Cloud MCP Gateway operations </td>
  </tr>
  <tr>
    <td> mcp_gateway_accesses_list </td>
    <td> List access bindings for Yandex Cloud MCP Gateway </td>
  </tr>
  <tr>
    <td> mcp_gateway_accesses_update </td>
    <td> Update access bindings for Yandex MCP Gateway </td>
  </tr>
</table>
