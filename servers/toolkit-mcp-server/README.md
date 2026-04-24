# Yandex Cloud Toolkit MCP Server

MCP server to deploy simple applications in Yandex Cloud.

Server interacts with Compute, VPC, IAM, Storage (S3) and Managed YDB.

## Table of Contents

- [Yandex Cloud Toolkit MCP Server](#yandex-cloud-toolkit-mcp-server)
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

- Deploy my service in Yandex Cloud with public IP address
- Create S3 bucket for static pages distribution
- Create YDB database for my server (provided in IDE)
- Describe my security group rules
- Are all my virtual machines running?

## Installation and Usage

### Configuration

To start working with Yandex Cloud Toolkit MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-toolkit` server.

There are two available ways:

#### 1. NPM Client (recommended)

Provides authentication via OAuth (browser-based, default) or Yandex Cloud CLI (`yc`).

> See the [npm package documentation](https://www.npmjs.com/package/@yandex-cloud/mcp) for more details.

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary roles (e.g., `editor` or `compute.admin`).
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
    "yandex-cloud-toolkit": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y", "@yandex-cloud/mcp",
        "-s", "toolkit",
        "-H", "Cloud-Id:<Cloud ID (optional)>",
        "-H", "Folder-Id:<Folder ID (optional)>"
      ]
    }
  }
}
```

#### 2. Streamable HTTP

**Prerequisites:**

- Roles. Account to perform operations with this MCP Server must have the necessary roles (e.g., `editor` or `compute.admin`).
- [IAM token](https://yandex.cloud/en/docs/iam/concepts/authorization/iam-token). You can get it using [Yandex Cloud CLI](https://yandex.cloud/en/docs/cli/quickstart):

  - `yc iam create-token` for user account
  - `yc iam create-token --impersonate-service-account-id <service-account-id>` for [service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts)

  > The IAM token has a maximum lifespan of **12 hours**. After expiration, it must be rotated.

**Configuration:**

```json
{
  "mcpServers": {
    "yandex-cloud-toolkit": {
      "type": "streamableHttp",
      "url": "https://toolkit.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Cloud-Id": "<Cloud ID (optional)>",
        "Folder-Id": "<Folder ID (optional)>"
      }
    }
  }
}
```

### Headers

| Header | Description | Requireness |
| ------------- | ------------- | --------- |
| Cloud-Id | Yandex Cloud cloud as default value for MCP tool's input field `cloud_id` | Optional |
| Folder-Id | Yandex Cloud folder as default value for MCP tool's input field `folder_id` | Optional |
| Authorization | Yandex Cloud IAM Token for Streamable HTTP authorization | Required for Streamable HTTP |

## Tools

Yandex Cloud Toolkit MCP Server currently consists of 42 tools listed below:

<table>
  <tr>
    <th> Yandex Cloud Service </th>
    <th> Tool </th>
    <th> Description </th>
  </tr>

  <tr>
    <td rowspan="18"> Compute </td>
    <td> instance_get </td>
    <td> Get Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instances_list </td>
    <td> List Yandex Cloud compute instances in folder </td>
  </tr>
  <tr>
    <td> instance_create </td>
    <td> Create Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_update </td>
    <td> Update Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_metadata_update </td>
    <td> Update Yandex Cloud compute instance metadata </td>
  </tr>
  <tr>
    <td> instance_network_interface_update </td>
    <td> Update Yandex Cloud compute instance network interface </td>
  </tr>
  <tr>
    <td> instance_delete </td>
    <td> Delete Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_start </td>
    <td> Start Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_stop </td>
    <td> Stop Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_restart </td>
    <td> Restart Yandex Cloud compute instance </td>
  </tr>
  <tr>
    <td> instance_operations_list </td>
    <td> List Yandex Cloud compute instance operations. E.g. creation or deletion </td>
  </tr>
  <tr>
    <td> image_get </td>
    <td> Get Yandex Cloud compute instance image </td>
  </tr>
  <tr>
    <td> image_get_latest_by_family </td>
    <td> Get latest Yandex Cloud compute instance image in folder by its family </td>
  </tr>
  <tr>
    <td> images_list </td>
    <td> List Yandex Cloud compute instance images in folder </td>
  </tr>
  <tr>
    <td> disk_get </td>
    <td> Get Yandex Cloud disk </td>
  </tr>
  <tr>
    <td> disks_list </td>
    <td> List Yandex Cloud disks in folder </td>
  </tr>
  <tr>
    <td> disk_types_list </td>
    <td> List Yandex Cloud disk types </td>
  </tr>
  <tr>
    <td> zones_list </td>
    <td> List Yandex Cloud availability zones </td>
  </tr>

  <tr>
    <td rowspan="4"> VPC </td>
    <td> network_get </td>
    <td> Get Yandex Cloud network </td>
  </tr>
  <tr>
    <td> networks_list </td>
    <td> List Yandex Cloud networks in folder </td>
  </tr>
  <tr>
    <td> network_subnets_list </td>
    <td> List Yandex Cloud subnets in network </td>
  </tr>
  <tr>
    <td> network_security_groups_list </td>
    <td> List Yandex Cloud security groups in network </td>
  </tr>

  <tr>
    <td rowspan="5"> IAM </td>
    <td> roles_list </td>
    <td> List Yandex Cloud roles </td>
  </tr>
  <tr>
    <td> service_account_get </td>
    <td> Get Yandex Cloud service account </td>
  </tr>
  <tr>
    <td> service_accounts_list </td>
    <td> List Yandex Cloud service accounts in folder </td>
  </tr>
  <tr>
    <td> service_account_create </td>
    <td> Create Yandex Cloud service account </td>
  </tr>
  <tr>
    <td> service_account_delete </td>
    <td> Delete Yandex Cloud service account </td>
  </tr>

  <tr>
    <td rowspan="6"> Resource Manager </td>
    <td> clouds_list </td>
    <td> List Yandex Cloud clouds in organization </td>
  </tr>
  <tr>
    <td> folder_get </td>
    <td> Get Yandex Cloud folder </td>
  </tr>
  <tr>
    <td> folders_list </td>
    <td> List Yandex Cloud folders in cloud </td>
  </tr>
  <tr>
    <td> folder_accesses_list </td>
    <td> List access bindings for Yandex Cloud folder </td>
  </tr>
  <tr>
    <td> folder_accesses_update </td>
    <td> Update access bindings for Yandex Cloud folder </td>
  </tr>

  <tr>
    <td rowspan="5"> Managed YDB </td>
    <td> ydb_database_get </td>
    <td> Get Yandex Cloud YDB database </td>
  </tr>
  <tr>
    <td> ydb_databases_list </td>
    <td> List Yandex Cloud YDB databases in folder </td>
  </tr>
  <tr>
    <td> ydb_serverless_database_create </td>
    <td> Create Yandex Cloud YDB serverless database </td>
  </tr>
  <tr>
    <td> ydb_serverless_database_update </td>
    <td> Update Yandex Cloud YDB serverless database </td>
  </tr>
  <tr>
    <td> ydb_database_delete </td>
    <td> Delete Yandex Cloud YDB database </td>
  </tr>

  <tr>
    <td rowspan="5"> Object Storage (S3) </td>
    <td> bucket_get </td>
    <td> Get Yandex Cloud Object Storage bucket </td>
  </tr>
  <tr>
    <td> buckets_list </td>
    <td> List Yandex Cloud Object Storage buckets in folder with basic view </td>
  </tr>
  <tr>
    <td> bucket_create </td>
    <td> Create Yandex Cloud Object Storage bucket </td>
  </tr>
  <tr>
    <td> bucket_update </td>
    <td> Update Yandex Cloud Object Storage bucket </td>
  </tr>
  <tr>
    <td> bucket_delete </td>
    <td> Delete Yandex Cloud Object Storage bucket </td>
  </tr>
</table>

### Current Restrictions

1. Only serverless YDB databases creation and update supported;
2. Access managing is available only for folders;
