# Yandex Cloud Serverless Containers MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Containers - create, deploy, configure, and manage containerized applications with revisions, scaling policies, and access control.

## Table of Contents

- [Yandex Cloud Serverless Containers MCP Server (Preview)](#yandex-cloud-serverless-containers-mcp-server-preview)
  - [Table of Contents](#table-of-contents)
  - [Use Cases](#use-cases)
  - [Installation and Usage](#installation-and-usage)
    - [Headers](#headers)
    - [Configuration](#configuration)
      - [NPM Client (recommended)](#npm-client-recommended)
      - [Streamable HTTP](#streamable-http)
  - [Tools](#tools)

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

### Headers

| Header | Description | Requireness |
| ------------- | ------------- | --------- |
| Folder-Id | Yandex Cloud folder as default value for MCP tool's input field `folder_id` | Optional |
| Authorization | Yandex Cloud IAM Token for Streamable HTTP authorization | Required for Streamable HTTP |

### Configuration

To start working with Yandex Cloud Serverless Containers MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-containers` server.

There are two available ways:

#### NPM Client (recommended)

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/serverless-containers/security/#roles-list) (e.g., `editor` or `serverless-containers.admin`).
- Node.js 18.0.0 or higher
- [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart) (`yc`) installed with configured user profile

> See the [package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-containers": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "containers",
        "-p", "<CLI profile (optional)>",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/serverless-containers/security/#roles-list) (e.g., `editor` or `serverless-containers.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-containers": {
      "type": "streamableHttp",
      "url": "https://containers.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Folder-Id": "<Folder ID (optional)>"
      }
    }
  }
}
```

## Tools

Yandex Cloud Serverless Containers MCP Server currently consists of 12 tools listed below:

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
    <td> container_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Container </td>
  </tr>
  <tr>
    <td> container_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Container </td>
  </tr>
</table>
