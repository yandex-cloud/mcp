# Yandex Cloud Functions MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Functions - create, deploy, configure, and manage serverless functions and their versions, tags, scaling policies, and network connectivity.

## Table of Contents

- [Yandex Cloud Functions MCP Server (Preview)](#yandex-cloud-functions-mcp-server-preview)
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

- List all serverless functions in my folder
- Create a new function called 'data-processor' in folder xyz
- Get details of function version abc123
- Create a new version of my function with Go 1.23 runtime
- Set tag 'production' to function version xyz
- Configure scaling policy for my function to have 2 provisioned instances

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) needed for your tasks (e.g. `editor` or `functions.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [roles](https://yandex.cloud/en/docs/functions/security/#roles-list) needed for your tasks (e.g. `functions.editor`) to the service account you created.

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

To start working with Yandex Cloud Functions MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-functions` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-functions": {
      "type": "streamableHttp",
      "url": "https://functions.mcp.cloud.yandex.net/mcp",
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
    "yandex-cloud-functions": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://functions.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>",
        "--header", "Folder-Id:<YC Folder ID>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

## Tools

Yandex Cloud Functions MCP Server currently consists of 23 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> function_get </td>
    <td> Get Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> functions_list </td>
    <td> List Yandex Cloud Serverless Functions in the folder </td>
  </tr>
  <tr>
    <td> function_create </td>
    <td> Create Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> function_update </td>
    <td> Update Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> function_delete </td>
    <td> Delete Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> runtimes_list </td>
    <td> List Yandex Cloud Serverless Function runtimes </td>
  </tr>
  <tr>
    <td> function_version_get </td>
    <td> Get Yandex Cloud Serverless Function version </td>
  </tr>
  <tr>
    <td> function_version_get_by_tag </td>
    <td> Get Yandex Cloud Serverless Function version by tag </td>
  </tr>
  <tr>
    <td> function_versions_list </td>
    <td> List Yandex Cloud Serverless Function versions </td>
  </tr>
  <tr>
    <td> function_version_create </td>
    <td> Create Yandex Cloud Serverless Function version </td>
  </tr>
  <tr>
    <td> function_version_delete </td>
    <td> Delete Yandex Cloud Serverless Function version. Old untagged function versions are deleted automatically </td>
  </tr>
  <tr>
    <td> function_tags_list </td>
    <td> List Yandex Cloud Serverless Function tag history </td>
  </tr>
  <tr>
    <td> function_tag_set </td>
    <td> Set tag to Yandex Cloud Serverless Function version </td>
  </tr>
  <tr>
    <td> function_tag_remove </td>
    <td> Remove tag of Yandex Cloud Serverless Function version </td>
  </tr>
  <tr>
    <td> function_scaling_policies_list </td>
    <td> List Yandex Cloud Serverless Function scaling policies </td>
  </tr>
  <tr>
    <td> function_scaling_policy_set </td>
    <td> Set Yandex Cloud Serverless Function scaling policy </td>
  </tr>
  <tr>
    <td> function_scaling_policy_remove </td>
    <td> Remove Yandex Cloud Serverless Function scaling policy </td>
  </tr>
  <tr>
    <td> function_operations_list </td>
    <td> List Yandex Cloud Serverless Function operations </td>
  </tr>
  <tr>
    <td> function_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> function_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Function </td>
  </tr>
  <tr>
    <td> network_connected_resources_list </td>
    <td> List Yandex Cloud Serverless resources connected to any network from the specified scope </td>
  </tr>
  <tr>
    <td> network_get </td>
    <td> Get Yandex Cloud VPC network connected to Serverless resources </td>
  </tr>
  <tr>
    <td> networks_list </td>
    <td> List Yandex Cloud VPC networks connected to Serverless resources </td>
  </tr>
</table>
