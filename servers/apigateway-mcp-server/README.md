# Yandex Cloud API Gateway MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud API Gateway - create, configure, and manage API gateways with OpenAPI specifications, custom domains, WebSocket connections, and access control.

## Table of Contents

- [Yandex Cloud API Gateway MCP Server (Preview)](#yandex-cloud-api-gateway-mcp-server-preview)
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

- List all API gateways in my folder
- Create a new API gateway with OpenAPI specification
- Get details of API gateway xyz
- Update API gateway configuration with new OpenAPI spec
- Add custom domain to my API gateway
- Get OpenAPI specification of my gateway
- Manage WebSocket connections
- Configure access bindings for API gateway

## Installation and Usage

### Configuration

To start working with Yandex Cloud API Gateway MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-apigateway` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/api-gateway/security/#roles-list) (e.g., `editor` or `api-gateway.admin`).
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
    "yandex-cloud-apigateway": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "apigateway",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/api-gateway/security/#roles-list) (e.g., `editor` or `api-gateway.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-apigateway": {
      "type": "streamableHttp",
      "url": "https://apigateway.mcp.cloud.yandex.net/mcp",
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

Yandex Cloud API Gateway MCP Server currently consists of 16 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> api_gateway_get </td>
    <td> Get Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateways_list </td>
    <td> List Yandex Cloud API Gateways in the folder </td>
  </tr>
  <tr>
    <td> api_gateway_create </td>
    <td> Create Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_update </td>
    <td> Update Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_delete </td>
    <td> Delete Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_resume </td>
    <td> Resume Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_stop </td>
    <td> Resume Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_domain_add </td>
    <td> Add domain of Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_domain_remove </td>
    <td> Remove domain of Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_openapi_spec_get </td>
    <td> Get OpenAPI specification of Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_operations_list </td>
    <td> List Yandex Cloud API Gateway operations </td>
  </tr>
  <tr>
    <td> api_gateway_accesses_list </td>
    <td> List access bindings for Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> api_gateway_accesses_update </td>
    <td> Update access bindings for Yandex Cloud API Gateway </td>
  </tr>
  <tr>
    <td> websocket_connection_get </td>
    <td> Get Yandex Cloud API Gateway webscocket connection </td>
  </tr>
  <tr>
    <td> websocket_connection_send </td>
    <td> Send data to Yandex Cloud API Gateway webscocket connection </td>
  </tr>
  <tr>
    <td> websocket_connection_disconnect </td>
    <td> Disconnect from Yandex Cloud API Gateway webscocket connection </td>
  </tr>
</table>
