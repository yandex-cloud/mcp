# Yandex Cloud Toolkit MCP Server

Lightweight MCP server to interact with Yandex Cloud Compute, VPC, IAM, Object Storage (S3) and Managed YDB.

It helps vibecoders to deploy simple applications in Yandex Cloud.

## Table of Contents

- [Yandex Cloud Toolkit MCP Server](#yandex-cloud-toolkit-mcp-server)
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

- Deploy my service in Yandex Cloud with public IP address
- Create S3 bucket for static pages distribution
- Create YDB database for my server (provided in IDE)
- Describe my security group rules
- Are all my virtual machines running?

## Installation and Usage

### Prerequisites

#### Authorization

- User account authorization

  1. User account must have all roles needed for your tasks (e.g. `editor` or `compute.admin`);

  2. [Install](https://yandex.cloud/en/docs/cli/quickstart) Yandex Cloud CLI;

  3. Get IAM token with `yc iam create-token` CLI command.

      Then valid authorization header will be `Authorization: Bearer <IAM token>`.

      > Note that token has a maximum lifespan of **12 hours**. After expiration, it must be recreated.

- [Service account](https://yandex.cloud/en/docs/iam/concepts/users/service-accounts) authorization

  1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests.

  2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all needed roles (e.g. `compute.editor`) to the service account you created.

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
    <td> Cloud-Id </td>
    <td> YC cloud as default working area. If not specified, tool's input field <code>cloud_id</code> is required. </td>
    <td> Optional </td>
  </tr>
  <tr>
    <td> Folder-Id </td>
    <td> Yandex Cloud folder as default working area. If not specified, tool's input field <code>folder_id</code> is required. </td>
    <td> Optional </td>
  </tr>
</table>

### Configuration

To start working with Yandex Cloud Toolkit MCP Server, you have to update your assistant's configuration (e.g. Cline, Roo Code or Claude Desktop) by adding `yandex-cloud-toolkit` server.

There are two available ways:

1. Directly via streamable http

```json
{
  "mcpServers": {
    "yandex-cloud-toolkit": {
      "type": "streamableHttp",
      "url": "https://toolkit.mcp.cloud.yandex.net/mcp",
      "headers": {
        "Authorization": "Bearer <YC IAM Token>",
        "Cloud-Id": "<YC Cloud ID>",
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
    "yandex-cloud-toolkit": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://toolkit.mcp.cloud.yandex.net/mcp",
        "--header", "Authorization:Bearer <YC IAM Token>",
        "--header", "Cloud-Id:<YC Cloud ID>",
        "--header", "Folder-Id:<YC Folder ID>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

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
3. Many advanced methods are currently excluded to minimize token consumption by the LLM.
