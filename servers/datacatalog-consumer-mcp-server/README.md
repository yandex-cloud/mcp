# Yandex Cloud Data Catalog Consumer MCP Server

MCP server for Yandex Cloud Data Catalog Consumer is a centralized repository of organization metadata.

It allows searching various types of metadata: tables, views and queries, as well as getting the dependency graph (lineage) at the table and column level.

## Use Cases

Prompts examples:

- Build an SQL query for YoY sales analytics
- Find all tables with users' payment information
- Discover tables marked as containing sensitive data
- Determine the origin of the data for the customer_transactions table
- Find the right tables to calculate user retention metrics
- Discover where user behavior data is stored on a website
- Determine what data should be used for sales funnel conversion analysis
- Show all dependencies of the transactions table to better understand the impact of schema changes

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all the [required roles](https://yandex.cloud/en/docs/metadata-hub/security/data-catalog-roles) (e.g. `data-catalog.viewer`).

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI.

  3. Get an IAM token for a Yandex account by running the `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > The IAM token lifetime does not exceed 12 hours; however, we recommend requesting a token more often, e.g., every hour.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to work with the MCP server.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all [required roles](https://yandex.cloud/en/docs/metadata-hub/security/data-catalog-roles) (e.g. `data-catalog.viewer`) to the service account you've created.

  3. There are different authorization options, depending on the environment you will use to send your requests:

      1. Local usage:

          1. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

          2. Get a service account's IAM token by running the `yc iam create-token --impersonate-service-account-id <service-account-id>` CLI command.

          Then valid authorization header will be `Authorization: Bearer <IAM token>`.

          > The IAM token lifetime does not exceed 12 hours; however, we recommend requesting a token more often, e.g., every hour.

      2. Yandex Cloud Compute Instance (Virtual Machine):

          Use the [metadata service](https://yandex.cloud/en/docs/security/standard/authentication#service-accounts) by assigning the service account to the Virtual Machine.

### Configuration

To start working with Yandex Cloud Data Catalog MCP Server, you have to update your assistant's configuration (for example, Cline, Roo Code or Claude Desktop) by adding the `yandex-cloud-data-catalog` server.

There are two available ways:

1. Directly via streamable HTTP:

```json
{
  "mcpServers": {
    "yandex-cloud-data-catalog": {
      "type": "streamableHttp",
      "url": "https://datacatalog-consumer.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>"
      }
    }
  }
}
```

2. Using stdio with the `npx mcp-remote` client:

```json
{
  "mcpServers": {
    "yandex-cloud-data-catalog": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://datacatalog-consumer.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud Data Catalog MCP Server currently consists of 4 tools listed below:

<table>
  <tr>
    <th> Tool </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> list_datacatalogs </td>
    <td> Get a list of data catalogs in the specified folder. </td>
  </tr>
  <tr>
    <td> search </td>
    <td> Search for metadata or markup resources in the data catalog. Supports searching by text query across names, descriptions, documentation, and SQL content. Allows filtering by resource type (tables, views, queries, domains, tags, terms, etc.) and various attributes. </td>
  </tr>
  <tr>
    <td> get_assets_by_urns </td>
    <td> Get detailed information about metadata by their URN identifiers. </td>
  </tr>
  <tr>
    <td> get_lineage </td>
    <td> Get the lineage graph between assets. Returns relationships at the asset level and at the column level, showing data flow upstream (sources) and downstream (destinations). </td>
  </tr>
</table>
