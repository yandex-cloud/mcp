# Yandex Cloud Data Catalog Consumer MCP Server

MCP server for Yandex Cloud Data Catalog Consumer is a centralized repository of organization metadata.

It allows searching various types of metadata: tables, views and queries, as well as getting the dependency graph (lineage) at the table and column level.

## Table of Contents

- [Yandex Cloud Data Catalog Consumer MCP Server](#yandex-cloud-data-catalog-consumer-mcp-server)
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

- Build an SQL query for YoY sales analytics
- Find all tables with users' payment information
- Discover tables marked as containing sensitive data
- Determine the origin of the data for the customer_transactions table
- Find the right tables to calculate user retention metrics
- Discover where user behavior data is stored on a website
- Determine what data should be used for sales funnel conversion analysis
- Show all dependencies of the transactions table to better understand the impact of schema changes

## Installation and Usage

### Configuration

To start working with Yandex Cloud Data Catalog MCP Server, you have to update your assistant's configuration (for example, Cline, Roo Code or Claude Desktop) by adding the `yandex-cloud-data-catalog` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the [required roles](https://yandex.cloud/en/docs/metadata-hub/security/data-catalog-roles) (e.g., `data-catalog.viewer`).
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
    "yandex-cloud-datacatalog-consumer": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "datacatalog-consumer"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the [required roles](https://yandex.cloud/en/docs/metadata-hub/security/data-catalog-roles) (e.g., `data-catalog.viewer`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

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

### Headers

| Header | Description | Requireness |
| ------------- | ------------- | --------- |
| Authorization | Yandex Cloud IAM Token for Streamable HTTP authorization | Required for Streamable HTTP |

## Tools

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
