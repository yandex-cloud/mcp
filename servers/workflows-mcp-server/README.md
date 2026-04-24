# Yandex Cloud Serverless Workflows MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Workflows - create, configure, and manage workflows with YAML specifications, executions, scheduling, and access control.

## Table of Contents

- [Yandex Cloud Serverless Workflows MCP Server (Preview)](#yandex-cloud-serverless-workflows-mcp-server-preview)
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

- List all workflows in my folder
- Create a new workflow with YAML specification
- Start workflow execution with input data
- Get workflow execution status and history
- Stop or terminate running execution
- Update workflow specification
- Configure workflow scheduling with cron expression
- Manage access bindings for workflows

## Installation and Usage

### Configuration

To start working with Yandex Cloud Serverless Workflows MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-workflows` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/serverless-integrations/security/workflows) (e.g., `editor` or `serverless.workflows.admin`).
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
    "yandex-cloud-workflows": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "workflows",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/serverless-integrations/security/workflows) (e.g., `editor` or `serverless.workflows.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-workflows": {
      "type": "streamableHttp",
      "url": "https://workflows.mcp.cloud.yandex.net/mcp",
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

Yandex Cloud Serverless Workflows MCP Server currently consists of 15 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> workflow_get </td>
    <td> Get Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflows_list </td>
    <td> List Yandex Cloud Serverless Workflows in the folder </td>
  </tr>
  <tr>
    <td> workflow_create </td>
    <td> Create Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflow_update </td>
    <td> Update Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflow_delete </td>
    <td> Delete Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflow_operations_list </td>
    <td> List Yandex Cloud Serverless Workflow operations </td>
  </tr>
  <tr>
    <td> workflow_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflow_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Workflow </td>
  </tr>
  <tr>
    <td> workflow_execution_get </td>
    <td> Get Yandex Cloud Serverless Workflow execution </td>
  </tr>
  <tr>
    <td> workflow_execution_list </td>
    <td> List Yandex Cloud Serverless Workflow execution </td>
  </tr>
  <tr>
    <td> workflow_execution_start </td>
    <td> Start Yandex Cloud Serverless Workflow execution </td>
  </tr>
  <tr>
    <td> workflow_execution_stop </td>
    <td> Stop Yandex Cloud Serverless Workflow execution </td>
  </tr>
  <tr>
    <td> workflow_execution_terminate </td>
    <td> Terminate Yandex Cloud Serverless Workflow execution </td>
  </tr>
  <tr>
    <td> workflow_execution_history_get </td>
    <td> Get Yandex Cloud Serverless Workflow execution history </td>
  </tr>
</table>
