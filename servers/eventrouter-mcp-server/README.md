# Yandex Cloud Event Router MCP Server (Preview)

> The server is running in preview mode, some features may be unstable

MCP server for managing Yandex Cloud Serverless Event Router - create and configure event buses, connectors, rules for event-driven architectures with filtering, routing, and access control.

## Use Cases

Prompts examples:

- List all event buses in my folder
- Create a new event bus for my application
- Set up a connector to receive events from external source
- Create routing rule with filtering conditions
- Put event to the bus
- Enable or disable routing rule
- Configure connector to start/stop event processing
- Manage access bindings for buses and rules

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all [roles](https://yandex.cloud/en/docs/serverless-integrations/security/eventrouter) needed for your tasks (e.g. `editor` or `serverless.eventrouter.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [roles](https://yandex.cloud/en/docs/serverless-integrations/security/eventrouter) needed for your tasks (e.g. `serverless.eventrouter.editor`) to the service account you created.

  3. There are different authorization options, depending on the environment you will call MCP server from:

      1. Local usage

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get service account's IAM token with `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

      2. Yandex Cloud Compute Instance (Virtual Machine)

          Use the [Metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the VM.

### Configuration

To start working with Yandex Cloud Event Router MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-eventrouter` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-eventrouter": {
      "type": "streamableHttp",
      "url": "https://eventrouter.mcp.cloud.yandex.net/mcp",
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
    "yandex-cloud-eventrouter": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://eventrouter.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud Event Router MCP Server currently consists of 33 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td> bus_get </td>
    <td> Get Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> buses_list </td>
    <td> List Yandex Cloud Serverless Event Router buses in the folder </td>
  </tr>
  <tr>
    <td> bus_create </td>
    <td> Create Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> bus_update </td>
    <td> Update Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> bus_delete </td>
    <td> Delete Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> bus_operations_list </td>
    <td> List Yandex Cloud Serverless Event Router bus operations </td>
  </tr>
  <tr>
    <td> bus_access_set </td>
    <td> Set access bindings for Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> bus_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> bus_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Event Router bus </td>
  </tr>
  <tr>
    <td> connector_get </td>
    <td> Get Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connectors_list </td>
    <td> List Yandex Cloud Serverless Event Router connectors in the folder or the bus </td>
  </tr>
  <tr>
    <td> connector_create </td>
    <td> Create Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_update </td>
    <td> Update Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_delete </td>
    <td> Delete Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_start </td>
    <td> Start Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_stop </td>
    <td> Stop Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_operations_list </td>
    <td> List Yandex Cloud Serverless Event Router connector operations </td>
  </tr>
  <tr>
    <td> connector_access_set </td>
    <td> Set access bindings for Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> connector_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Event Router connector </td>
  </tr>
  <tr>
    <td> event_put </td>
    <td> Put Yandex Cloud Serverless Event Router event </td>
  </tr>
  <tr>
    <td> event_send </td>
    <td> Send Yandex Cloud Serverless Event Router event </td>
  </tr>
  <tr>
    <td> rule_get </td>
    <td> Get Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rules_list </td>
    <td> List Yandex Cloud Serverless Event Router rules in the folder or the bus </td>
  </tr>
  <tr>
    <td> rule_create </td>
    <td> Create Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_update </td>
    <td> Update Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_delete </td>
    <td> Delete Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_enable </td>
    <td> Enable Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_disable </td>
    <td> Disable Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_operations_list </td>
    <td> List Yandex Cloud Serverless Event Router rule operations </td>
  </tr>
  <tr>
    <td> rule_access_set </td>
    <td> Set access bindings for Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_accesses_list </td>
    <td> List access bindings for Yandex Cloud Serverless Event Router rule </td>
  </tr>
  <tr>
    <td> rule_accesses_update </td>
    <td> Update access bindings for Yandex Cloud Serverless Event Router rule </td>
  </tr>
</table>
