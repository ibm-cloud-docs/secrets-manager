---

copyright:
  years: 2022
lastupdated: "2022-06-09"

keywords: Secrets Manager Vault, Vault APIs, HashiCorp, Vault, Vault wrapper, use Vault with Secrets Manager, KV, key-value, KV APIs

subcollection: secrets-manager

---

{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:tip: .tip}
{:preview: .preview}
{:deprecated: .deprecated}
{:beta: .beta}
{:term: .term}
{:shortdesc: .shortdesc}
{:script: data-hd-video='script'}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:help: data-hd-content-type='help'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}
{:curl: .ph data-hd-programlang='curl'}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:go: .ph data-hd-programlang='go'}
{:unity: .ph data-hd-programlang='unity'}
{:release-note: data-hd-content-type='release-note'}

# Managing key-value secrets with Vault API
{: #vault-manage-kv}

With {{site.data.keyword.secrets-manager_full}}, you can manage multiple versions per key and access the history and metadata of your [key-value secret](/docs/secrets-manager?topic=secrets-manager-key-value&interface=ui) by using the HashiCorp Vault HTTP API. 
{: shortdesc}

## Overview
{: #overview-kv}

If you already use the Vault API, you can use its API format and guidelines to interact with {{site.data.keyword.secrets-manager_short}}. {{site.data.keyword.secrets-manager_short}} supports KV version 2 only. The endpoints for the key-value secrets engine that are defined in the [Vault documentation](https://www.vaultproject.io/api-docs/secret/kv/kv-v2#kv-secrets-engine-version-2-api){: external} are compatible with the CLI and other applicable tools. 

For more information about authentication and the custom version of open source HashiCorp Vault that {{site.data.keyword.secrets-manager_short}} uses, see [Vault API](/docs/secrets-manager?topic=secrets-manager-vault-api).

To use the standard REST API for Secrets Manager, check out the [{{site.data.keyword.secrets-manager_short}} API reference](/apidocs/secrets-manager){: external}.


### Difference between KV and {{site.data.keyword.secrets-manager_short}}
{: #differences-kv}

The key-value secrets engine that {{site.data.keyword.secrets-manager_short}} uses differs slightly from Vault's KV secrets engine. 

* You cannot fully customize the path. The path to a key-value secret must be: 
    * `{secret_group_id}/{secret_name}` for secrets that are located in a custom secret group. 
    * `/{secret_name}` for secrets that are in the default group.
* The methods that are used on Vault to [configure the key-value engine](https://www.vaultproject.io/api-docs/secret/kv/kv-v2#configure-the-kv-engine){: external} and [read the configuration](https://www.vaultproject.io/api-docs/secret/kv/kv-v2#read-kv-engine-configuration){: external} are not supported. 

## Create or update a key-value secret
{: #update-kv-secret}

Create a version of a key-value secret. 

### Example request
{: #update-kv-request}

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/data/{secret_name}' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
            "payload": {
                "key7":"value7"
            }
    }'
```
{: codeblock}


| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the {{site.data.keyword.secrets-manager_short}} instance. |
| `region` | The region in which the {{site.data.keyword.secrets-manager_short}} instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
| `payload` | **Required.** The secret data in JSON format to assign to the secret. The maximum file size is 512 KB. |
{: caption="Table 1. Create or update a key-value secret request parameters" caption-side="top"}


### Example response
{: #update-kv-response}

A request to update a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "9000000d4-f0000-4c000-000000-800000000f",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_time": "2022-02-09T23:41:58.888138788Z",
        "deletion_time": "",
        "destroyed": false,
        "version": 2
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

## Read a version of a key-value secret
{: #kv-version}

Get a version of a key-value secret. A successful request returns the secret data that is associated with the specified version of your secret, along with other metadata. 

### Example request
{: kv-version-request}

```sh
curl -X GET 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/data/{secret_name}?version={version}' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' 
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `version` | The versions that you want to read. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. |
{: caption="Table 2. Read a version of a key-value secret request parameters" caption-side="top"}


### Example response
{: #kv-version-response}

A request to get a version of a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "400000-60000-8e000-0ad0-00000bc0000caebe",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "data": {
            "key": "value"
        },
        "metadata": {
            "created_time": "2022-01-13T21:31:49.893962888Z",
            "deletion_time": "",
            "destroyed": false,
            "version": 1
        }
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}


## Delete the latest version of a key-value secret
{: #kv-version-delete}

Delete the latest version of a key-value secret. After you delete the version, you cannot retrieve it by using `list` or `get` API calls. However, it is a soft delete and the underlying data is not removed. You can undo the deletion by calling the [undelete](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#kv-version-restore) API endpoint.

### Example request
{: #kv-version-delete-request}

```sh
curl -X DELETE 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/data/{secret_name}' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' 
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
{: caption="Table 3. Delete the latest version of a key-value secret request parameters" caption-side="top"}

### Example response
{: #kv-version-delete-response}

A request to delete the latest version of a key-value secret in the `default` secret group returns a blank response with a 204 status code to confirm that the latest version was deleted.

## Delete specified versions of a key-value secret
{: #kv-version-delete-multiple}

Delete the specified versions of a key-value secret. After you delete the versions, you can't retrieve them by using `list` or `get` API calls. However, it is a soft delete and the underlying data is not removed. You can undo the deletion by calling the [undelete](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#kv-version-restore) API endpoint.

### Example request
{: #kv-version-delete-multiple-request}

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/delete/test-kv' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -d '{
            "versions": [1, 2]
            }'
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
| `versions` | The specified versions that are to be deleted. |
{: caption="Table 4. Delete specified versions of a key-value secret request parameters" caption-side="top"}


### Example response
{: #kv-version-delete-multiple-response}

A request to get delete specified versions of a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "43abde16-6a33-971f-1690-469eccc00d91",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": null,
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}


## Undelete a key-value secret
{: #kv-version-restore}

Restore a previously deleted version of a key-value secret. 

### Example request
{: #kv-version-restore-request}

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/undelete/{secret_name}' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
            "versions": [
                1, 2
                ]
            }
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
| `versions` | The versions of the key-value secret you want to delete. | 
{: caption="Table 5. Undelete a version of a key-value secret request parameters" caption-side="top"}

### Example response
{: #kv-version-restore-response}

A request to restore a version of a key-value secret in the `default` secret group returns a blank response with a 204 status code to confirm that the specified versions were restored.


## Destroy versions of a secret
{: #kv-destroy}

Destroy specified versions of a key-value secret permanently. To soft delete versions of a secret instead, use the [delete specified versions](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#kv-version-delete-multiple) API endpoint.

### Example request
{: #kv-destroy-request}

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.test.appdomain.cloud/v1/ibmcloud/kv/destroy/{secret_name}' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
            "versions": [1, 3]
            }'
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
| `versions` | The versions of the key-value secret you want to permanently destroy. | 
{: caption="Table 6. Destroy versions of a key-value secret request parameters" caption-side="top"}

### Example response
{: #kv-destroy-response}

A request to permanently destroy versions of a key-value secret in the `default` secret group returns a blank response with a 204 status code to confirm that the secret's versions were destroyed.



## Read metadata of key-value secret
{: #kv-metadata}

Get a key-value secret's metadata by specifying the ID of the version. 

### Example request
{: #kv-metadata-request}

```sh
curl -X GET 'https://{instance_id}.{region}.secrets-manager.test.appdomain.cloud/v1/ibmcloud/kv/metadata/{secret_name}' \
    -H 'Accept: application/json'
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
{: caption="Table 8. Read the metadata of a key-value secret request parameters" caption-side="top"}

### Example response
{: #kv-metadata-response}

A request to get the metadata of a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "400000-60000-8e000-0ad0-00000bc0000caebe",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "cas_required": false,
        "created_time": "2022-01-13T21:31:49.893962888Z",
        "current_version": 3,
        "delete_version_after": "3h25m19s",
        "max_versions": 5,
        "oldest_version": 0,
        "updated_time": "2022-02-09T23:54:16.313286558Z",
        "versions": {
            "1": {
                "created_time": "2022-01-13T21:31:49.893962888Z",
                "deletion_time": "",
                "destroyed": false
            },
            "2": {
                "created_time": "2022-02-09T23:41:58.888138788Z",
                "deletion_time": "",
                "destroyed": false
            },
            "3": {
                "created_time": "2022-02-09T23:54:16.313286558Z",
                "deletion_time": "",
                "destroyed": false
            }
        }
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}


## Delete the metadata and all versions of a key-value secret
{: #kv-delete-metadata}

Delete the metadata and all version data of a specified key-value secret permanently. All version history is removed when you use this API endpoint.

### Example request
{: #kv-delete-metadata-request}

```sh
curl -X DELETE 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/metadata/{secret_name}' \
    -H 'Accept: application/json'
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `secret_name` | The name of the key-value secret. | 
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
{: caption="Table 9. Delete the metadata of a key-value secret request parameters" caption-side="top"}

### Example response
{: #kv-delete-metadata-response}

A request to delete the metadata and all versions of a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "62b1e2c2-801a-6592-0526-edb38896a546",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": null,
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}


## List key names of a key-value secret
{: #list-kv}

Get a list of key names of a key-value secret. Do not encode sensitive information in key names. The values of the keys are not accessible by using this command.

### Example request
{: #list-kv-request}

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.test.appdomain.cloud/v1/ibmcloud/kv/metadata/?list=true" \ 
    -H 'Accept: application/json'\
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

| Request parameter | Description |
| ------------- | ------------------------- |
| `instance_id` | The ID of the Secrets Manager instance. |
| `region` | The region in which the Secrets Manager instance was created. |
| `Vault-Token` | The authentication token that is retrieved from Vault. | 
{: caption="Table 10. List the key names of a key-value secret request parameters" caption-side="top"}


### Example response
{: #list-kv-response}

A request to list the key names of a key-value secret in the `default` secret group returns the following response: 

```json
{
    "request_id": "a21993df-a4b7-21f1-95a9-c1af7be87d1b",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "keys": [
            "secret1",
            "secret2"
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

