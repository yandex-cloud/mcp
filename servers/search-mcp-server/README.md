# Yandex Search MCP Server

Web search using Yandex Search: both generative and classic.

The server uses international search type: `yandex.com` search domain name and `EN` localization.

## Use Cases

Prompts examples:

- Search for sites which offer lectures on prompt-engineering
- Search for the Alice AI announcement

## Installation and Usage

### Prerequisites

- Existing [Yandex Cloud folder](https://yandex.cloud/en/docs/resource-manager/operations/folder/create) for [Yandex Search API](https://yandex.cloud/en/docs/search-api/)

- Authorization

    There are two available options:

  1. With [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token)

      1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

      2. Get IAM token with `yc iam create-token` CLI command;

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

  2. With service account [API key](https://yandex.cloud/en/docs/iam/concepts/authorization/api-key)

      1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests to [Yandex Search API](https://yandex.cloud/en/docs/search-api/);

      2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) the `search-api.webSearch.user` role to the service account you created;

      3. [Create](https://yandex.cloud/en/docs/iam/operations/authentication/manage-api-keys#create-api-key) an API key for the service account with `yc.search-api.execute` for its [scope](https://yandex.cloud/en/docs/iam/concepts/authorization/api-key#scoped-api-keys).

      Then valid authorization header will be `Authorization: Api-Key <API key>`.

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
        "Authorization": "Bearer <YC IAM Token>", // Or "Api-Key <YC Api-Key>"
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
        "--header", "Authorization:Bearer <YC IAM Token>", // Or "Authorization:Api-Key <YC Api-Key>"
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
