# Yandex Cloud Serverless Triggers MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Triggers - create, configure, and manage event-driven triggers for functions and containers from various sources like timers, message queues, object storage, IoT, and more.

## Table of Contents

- [Yandex Cloud Serverless Triggers MCP Server (Preview)](#yandex-cloud-serverless-triggers-mcp-server-preview)
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

- List all serverless triggers in my folder
- Create a timer trigger to run my function every hour
- Set up a trigger for Object Storage events
- Create a message queue trigger with batch processing
- Resume paused trigger xyz

## Installation and Usage

### Configuration

To start working with Yandex Cloud Serverless Triggers MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-triggers` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) (e.g., `editor` or `functions.admin`).
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
    "yandex-cloud-triggers": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "triggers",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) (e.g., `editor` or `functions.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-triggers": {
      "type": "streamableHttp",
      "url": "https://triggers.mcp.cloud.yandex.net/mcp",
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

Yandex Cloud Serverless Triggers MCP Server currently consists of 8 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> trigger_get </td>
    <td> Get Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> triggers_list </td>
    <td> List Yandex Cloud Serverless Triggers in the folder </td>
  </tr>
  <tr>
    <td> trigger_create </td>
    <td> Create Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> trigger_update </td>
    <td> Update Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> trigger_delete </td>
    <td> Delete Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> trigger_pause </td>
    <td> Pause Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> trigger_resume </td>
    <td> Resume Yandex Cloud Serverless Trigger </td>
  </tr>
  <tr>
    <td> trigger_operations_list </td>
    <td> List Yandex Cloud Serverless Trigger operations </td>
  </tr>
</table>
