# Yandex Cloud Toolkit MCP Server

Lightweight MCP server to interact with Yandex Cloud Compute, VPC, IAM, Object Storage (S3) and Managed YDB.

It helps vibecoders to deploy simple applications in Yandex Cloud.

## Use Cases

Prompts examples:

- Deploy my service in Yandex Cloud with public IP address
- Create S3 bucket for static pages distribution
- Create YDB database for my server (provided in IDE)
- Describe my security group rules
- Are all my virtual machines running?

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

      1. [Create](https://yandex.cloud/en/docs/iam/operations/sa/create) a service account you will use to send requests;

      2. [Assign](https://yandex.cloud/en/docs/iam/operations/sa/assign-role-for-sa#binding-role-resource) all needed roles (e.g. `compute.editor`) to the service account you created;

      3. [Create](https://yandex.cloud/en/docs/iam/operations/authentication/manage-api-keys#create-api-key) an API key for the service account with all needed [scopes](https://yandex.cloud/en/docs/iam/concepts/authorization/api-key#scoped-api-keys).

      Then valid authorization header will be `Authorization: Api-Key <API key>`.

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
        "Authorization": "Bearer <YC IAM Token>" // Or "Api-Key <YC Api-Key>"
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
        "--header", "Authorization:Bearer <YC IAM Token>" // Or "Authorization:Api-Key <YC Api-Key>"
      ]
    }
  }
}
```

For the second option you also need `npx` to be installed.

### Tools

Yandex Cloud Toolkit MCP Server currently consists of 43 tools listed below:

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
    <td> Update Yandex Cloud compute instance's metadata </td>
  </tr>
  <tr>
    <td> instance_network_interface_update </td>
    <td> Update Yandex Cloud compute instance's network interface </td>
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
    <td> List Yandex Cloud compute instance's operations. E.g. creation or deletion. </td>
  </tr>
  <tr>
    <td> image_get </td>
    <td> Get Yandex Cloud compute instance image </td>
  </tr>
  <tr>
    <td> image_get_latest_by_family </td>
    <td> Get latest Yandex Cloud compute instance image in folder by it's family </td>
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
    <td> folder_access_set </td>
    <td> Set access bindings for Yandex Cloud folder </td>
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
