# Yandex Search MCP Server

Web search using Yandex Search: both generative and classic.

The server uses international search type: `yandex.com` search domain name and `EN` localization.

## Use Cases

Prompts examples:

- Search for sites which offer lectures on prompt-engineering
- Search for the Alice AI announcement

## Installation and Usage

### Prerequisites

1. Existing [Yandex Cloud folder](https://yandex.cloud/en/docs/resource-manager/operations/folder/create) for [Yandex Search API](https://yandex.cloud/en/docs/search-api/)

2. Authorization

- User account authorization

    1. User account must have the `search-api.webSearch.user` role in the folder;

    2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

    3. Get IAM token with `yc iam create-token` CLI command.

    Then valid authorization header will be `Authorization: Bearer <IAM token>`.

    > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests to [Yandex Search API](https://yandex.cloud/en/docs/search-api/).

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) the `search-api.webSearch.user` role to the service account you created.

  3. There are different authorization options, depending on the environment you will call MCP server from:

      1. Local usage

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get service account's IAM token with `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

      2. Yandex Cloud Compute Instance (Virtual Machine)

          Use the [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the VM.

### Configuration

To start working with Yandex Search MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-search` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-search": {
      "type": "streamableHttp",
      "url": "https://search.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Folder-Id": "<YC Folder ID>" // You can set `Folder-Id` header to not provide `folderId` argument in each request
      }
    }
  }
}
```

2. Using stdio with `npx mcp-remote` client

```json
{
  "mcpServers": {
    "yandex-search": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://search.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>",
        "--header", "Folder-Id:<YC Folder ID>" // You can set `Folder-Id` header to not provide `folderId` argument in each request
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

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
    <td> Searches via Yandex Search API using generative search with "chat completion" query format (user to assistant dialogue) </td>
  </tr>
</table>
