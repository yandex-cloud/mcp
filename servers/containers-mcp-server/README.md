# Yandex Cloud Serverless Containers MCP Server

MCP server for managing Yandex Cloud Serverless Containers - create, deploy, configure, and manage containerized applications with revisions, scaling policies, access control and container registry management.

## Table of Contents

- [Yandex Cloud Serverless Containers MCP Server)](#yandex-cloud-serverless-containers-mcp-server)
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

- List all serverless containers in my folder
- Create a new container called 'web-app'
- Deploy a new revision with Docker image from Container Registry
- Get details of container revision xyz
- Rollback container to previous revision
- Configure scaling policy for my container
- Update container configuration and labels
- Manage access bindings for container

## Installation and Usage

### Configuration

To start working with Yandex Cloud Serverless Containers MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-containers` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/serverless-containers/security/#roles-list) (e.g., `editor` or `serverless-containers.admin`).
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
    "yandex-cloud-containers": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "containers",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

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

### Headers

| Header | Description | Requireness |
| ------------- | ------------- | --------- |
| Folder-Id | Yandex Cloud folder as default value for MCP tool's input field `folder_id` | Optional |
| Authorization | Yandex Cloud IAM Token for Streamable HTTP authorization | Required for Streamable HTTP |

## Tools

Yandex Cloud Serverless Containers MCP Server currently consists of 24 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> docker_images_list </td>
    <td> 
      List Yandex Cloud Docker images in the folder, container registry or repository.
      To upload a Docker image, use `docker push` in Command Line Interface (CLI).
    </td>
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
    <td> Update Yandex Cloud Serverless Container </td>
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
    <td> List Yandex Cloud Serverless Container revisions: either for a specific container or all in the folder </td>
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
  <tr>
    <td> docker_image_repositories_list </td>
    <td> List Yandex Cloud Docker Container Registry repositories in the folder or registry </td>
  </tr>
  <tr>
    <td> docker_image_repository_upsert </td>
    <td> Upsert Yandex Cloud Docker Container Registry repository </td>
  </tr>
  <tr>
    <td> docker_image_repository_delete </td>
    <td> Delete Yandex Cloud Docker Container Registry repository </td>
  </tr>
  <tr>
    <td> docker_image_repository_accesses_list </td>
    <td> List access bindings for Yandex Cloud Container Registry repository </td>
  </tr>
  <tr>
    <td> docker_image_repository_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Container Registry repository </td>
  </tr>
  <tr>
    <td> docker_image_registries_list </td>
    <td> List Yandex Cloud Docker Container Registries in the folder </td>
  </tr>
  <tr>
    <td> docker_image_registry_create </td>
    <td> Create Yandex Cloud Docker Container Registry </td>
  </tr>
  <tr>
    <td> docker_image_registry_update </td>
    <td> Update Yandex Cloud Docker Container Registry </td>
  </tr>
  <tr>
    <td> docker_image_registry_delete </td>
    <td> Delete Yandex Cloud Docker Container Registry </td>
  </tr>
  <tr>
    <td> docker_image_registry_accesses_list </td>
    <td> List access bindings for Yandex Cloud Container Registry </td>
  </tr>
  <tr>
    <td> docker_image_registry_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Container Registry </td>
  </tr>
</table>
