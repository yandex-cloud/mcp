# Yandex Cloud MCP Gateway MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud MCP Gateway - create and configure MCP gateways with custom tools that can invoke functions, containers, HTTP endpoints, workflows, and other MCP servers.

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

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all roles needed for your tasks (e.g. `editor` or `serverless.mcpGateways.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all roles needed for your tasks (e.g. `serverless.mcpGateways.editor`) to the service account you created.

  3. There are different authorization options, depending on the environment you will call MCP server from:

      1. Local usage

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get service account's IAM token with `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

      2. Yandex Cloud Compute Instance (Virtual Machine)

          Use the [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the VM.

### Configuration

To start working with Yandex Cloud MCP Gateway MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-mcpgateway` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-mcpgateway": {
      "type": "streamableHttp",
      "url": "https://mcpgateway.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>"
      }
    }
  }
}
```

2. Using stdio with `npx mcp-remote` client

```json
{
  "mcpServers": {
    "yandex-cloud-mcpgateway": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcpgateway.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud MCP Gateway MCP Server currently consists of 8 tools listed below:

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
    <td> mcp_gateway_access_set </td>
    <td> Set access bindings for Yandex Cloud MCP Gateway </td>
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
