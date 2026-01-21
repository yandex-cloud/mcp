# Yandex Cloud Serverless Containers MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Containers - create, deploy, configure, and manage containerized applications with revisions, scaling policies, and access control.

## Use Cases

Prompts examples:

- List all serverless containers in my folder
- Create a new container called 'web-app'
- Deploy a new revision with Docker image from Container Registry
- Get details of container revision xyz
- Rollback container to previous revision
- Configure scaling policy for my container
- Update container configuration and labels
- Manage access bindings for container

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all [roles](https://yandex.cloud/en/docs/serverless-containers/security/#roles-list) needed for your tasks (e.g. `editor` or `serverless-containers.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [roles](https://yandex.cloud/en/docs/serverless-containers/security/#roles-list) needed for your tasks (e.g. `serverless-containers.editor`) to the service account you created.

  3. There are different authorization options, depending on the environment you will call MCP server from:

      1. Local usage

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get service account's IAM token with `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

      2. Yandex Cloud Compute Instance (Virtual Machine)

          Use the [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the VM.

### Configuration

To start working with Yandex Cloud Serverless Containers MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-containers` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-containers": {
      "type": "streamableHttp",
      "url": "https://containers.mcp.cloud.yandex.net/mcp",
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
    "yandex-cloud-containers": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://containers.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud Serverless Containers MCP Server currently consists of 13 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> container_get </td>
    <td> Get Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> containers_list </td>
    <td> List Yandex Cloud Serverless Containers in the folder </td>
  </tr>
  <tr>
    <td> container_create </td>
    <td> Create Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_update </td>
    <td> Upate Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_delete </td>
    <td> Delete Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_revision_deploy </td>
    <td> Deploy revision of Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_rollback </td>
    <td> Rollback Yandex Cloud Serverless Container to particular revision </td>
  </tr>
  <tr>
    <td> container_revision_get </td>
    <td> Get Yandex Cloud Serverless Container revision </td>
  </tr>
  <tr>
    <td> container_revisions_list </td>
    <td> List Yandex Cloud Serverless Container revisions </td>
  </tr>
  <tr>
    <td> container_operations_list </td>
    <td> List Yandex Cloud Serverless Container operations </td>
  </tr>
  <tr>
    <td> container_access_set </td>
    <td> Set access bindings for Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Container </td>
  </tr>
</table>
