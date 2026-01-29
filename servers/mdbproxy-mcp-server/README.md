# Yandex Cloud MDB Proxy MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud MDB Proxy - create and configure database proxies for Managed PostgreSQL and ClickHouse clusters with secure access control.

## Table of Contents

- [Yandex Cloud MDB Proxy MCP Server (Preview)](#yandex-cloud-mdb-proxy-mcp-server-preview)
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

- List all MDB proxies in my folder
- Create a new proxy for PostgreSQL cluster
- Set up proxy for ClickHouse database
- Update proxy configuration with new credentials
- Configure proxy target with specific database
- Manage access bindings for MDB proxy
- Get proxy operations history
- Delete MDB proxy

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) needed for your tasks (e.g. `editor` or `functions.mdbProxiesUser`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) needed for your tasks (e.g. `functions.mdbProxiesUser`) to the service account you created.

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

To start working with Yandex Cloud MDB Proxy MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-mdbproxy` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-mdbproxy": {
      "type": "streamableHttp",
      "url": "https://mdbproxy.mcp.cloud.yandex.net/mcp",
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
    "yandex-cloud-mdbproxy": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mdbproxy.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>",
        "--header", "Folder-Id:<YC Folder ID>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

## Tools

Yandex Cloud MDB Proxy MCP Server currently consists of 8 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> mdb_proxy_get </td>
    <td> Get Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxies_list </td>
    <td> List Yandex Cloud MDB Proxies in the folder </td>
  </tr>
  <tr>
    <td> mdb_proxy_create </td>
    <td> Create Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxy_update </td>
    <td> Update Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxy_delete </td>
    <td> Delete Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxy_operations_list </td>
    <td> List Yandex Cloud MDB Proxy operations </td>
  </tr>
  <tr>
    <td> mdb_proxy_access_set </td>
    <td> Set access bindings for Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxy_accesses_list </td>
    <td> List access bindings for Yandex Cloud MDB Proxy </td>
  </tr>
  <tr>
    <td> mdb_proxy_accesses_update </td>
    <td> Update access bindings for Yandex Cloud MDB Proxy </td>
  </tr>
</table>
