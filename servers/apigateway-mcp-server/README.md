# Yandex Cloud API Gateway MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud API Gateway - create, configure, and manage API gateways with OpenAPI specifications, custom domains, WebSocket connections, and access control.

## Table of Contents

- [Yandex Cloud API Gateway MCP Server (Preview)](#yandex-cloud-api-gateway-mcp-server-preview)
  - [Table of Contents](#table-of-contents)
  - [Use Cases](#use-cases)
  - [Installation and Usage](#installation-and-usage)
    - [Prerequisites](#prerequisites)
      - [Authorization](#authorization)
    - [Headers](#headers)
    - [Configuration](#configuration)
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

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all [roles](https://yandex.cloud/en/docs/api-gateway/security/#roles-list) needed for your tasks (e.g. `editor` or `api-gateway.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [roles](https://yandex.cloud/en/docs/api-gateway/security/#roles-list) needed for your tasks (e.g. `api-gateway.websocketBroadcaster`) to the service account you created.

  3. There are different authorization options, depending on the environment you will call MCP server from:

      1. Local usage

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get service account's IAM token with `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

      2. Yandex Cloud Compute Instance (Virtual Machine)

          Use the [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the VM.

### Headers

<table>
  <tr>
    <th> Header </th>
    <th> Description </th>
    <th> Requireness </th>
  </tr>

  <tr>
    <td> Authorization </td>
    <td> Yandex Cloud IAM Token (see <a href="#authorization">Authorization</a>) </td>
    <td> Required </td>
  </tr>
  <tr>
    <td> Folder-Id </td>
    <td> Yandex Cloud folder as default working area. If not specified, tool's input field <code>folder_id</code> is required. </td>
    <td> Optional </td>
  </tr>
</table>

### Configuration

To start working with Yandex Cloud API Gateway MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-apigateway` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-apigateway": {
      "type": "streamableHttp",
      "url": "https://apigateway.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Folder-Id": "<YC Folder ID>"
      }
    }
  }
}
```

2. Using stdio with `npx mcp-remote` client

```json
{
  "mcpServers": {
    "yandex-cloud-apigateway": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://apigateway.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>",
        "--header", "Folder-Id:<YC Folder ID>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

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
