# Yandex Search MCP Server

Web search using Yandex Search: both generative and classic.

The server uses international search type: `yandex.com` search domain name and `EN` localization.

## Table of Contents

- [Yandex Search MCP Server](#yandex-search-mcp-server)
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

- Search for sites which offer lectures on prompt-engineering
- Search for the Alice AI announcement

## Installation and Usage

### Configuration

To start working with Yandex Search MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-search` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the `search-api.webSearch.user` [role](https://yandex.cloud/en/docs/search-api/security/) in the folder.
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
    "yandex-search": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "search",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the `search-api.webSearch.user` [role](https://yandex.cloud/en/docs/search-api/security/) in the folder.
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-search": {
      "type": "streamableHttp",
      "url": "https://search.mcp.cloud.yandex.net/mcp",
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

Yandex Search MCP Server currently consists of two tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> search </td>
    <td> Searches via Yandex Search API by provided query </td>
  </tr>
  <tr>
    <td> search_generative </td>
    <td> Searches via Yandex Search API using generative search with "chat completion" query format (user's conversation with assistant) </td>
  </tr>
</table>
