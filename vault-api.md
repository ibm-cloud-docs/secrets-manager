---

copyright:
  years: 2020
lastupdated: "2020-12-09"

keywords: Secrets Manager Vault, Vault APIs, HashiCorp, Vault, Vault wrapper, use Vault with Secrets Manager

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
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}
{:api: .ph data-hd-interface='api'} 
{:cli: .ph data-hd-interface='cli'} 
{:ui: .ph data-hd-interface='ui'}




# Using the Vault APIs
{: #vault-api}


If you're already using the HashiCorp Vault HTTP API, you can use its API format and guidelines to interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

To use the standard REST API for {{site.data.keyword.secrets-manager_short}}, check out the [{{site.data.keyword.secrets-manager_short}} API reference](/apidocs/secrets-manager){: external}.

## Introduction
{: #vault-api-intro}

{{site.data.keyword.secrets-manager_short}} uses a custom version of open source HashiCorp Vault. This custom version adds the {{site.data.keyword.cloud_notm}} IAM `auth` method and a set of secret engines to support operations in {{site.data.keyword.secrets-manager_short}} for various secret types.

All operations follow the REST API standards that are available for the Vault HTTP APIs. For more information about how to authenticate and use the Vault HTTP APIs, check out the [Vault documentation](https://www.vaultproject.io/api-docs/index){: external}.

Plug-ins and other components that are offered by the open source Vault community are not accessible by {{site.data.keyword.secrets-manager_short}}.
{: note}

### Endpoint URLs
{: #vault-api-base-url}

To access {{site.data.keyword.secrets-manager_short}} by using the Vault APIs, use the dedicated endpoint URL that is unique to your {{site.data.keyword.secrets-manager_short}} service instance. 

The following table lists the endpoint URLs by region that can be used to interact with the Vault APIs.

| Region | Endpoint URL |
| ---- | ---- |
| Dallas | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud` |
| Sydney | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud` |
{: caption="Table 1. Vault endpoint URLs" caption-side="top"}

You can find your unique endpoint URL in the **Endpoints** section of the {{site.data.keyword.secrets-manager_short}} UI, or by retrieving it by HTTP request. For more information, see [Retrieving your service endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#retrieve-service-endpoints).
{: tip}

### Common headers
{: #vault-api-headers}

This section describes the headers that are common to all requests.

| Header        | Description           | Required  |
| ------------- |-------------| -----|
| `X-Vault-Token`      | A valid Vault token with sufficient permissions to perform the operation. | Yes |
| `Content-Type`        | `application/json` | Yes |
{: caption="Table 2. Common headers" caption-side="top"}

### Timestamps
{: #vault-api-timestamps}

The timestamps in all requests and responses, such as creation and expiration dates, are formatted according to [RFC 3339](https://tools.ietf.org/html/rfc3339). For example: `1985-04-12T23:20:50.52Z`

### Field names
{: #vault-field-names}

This API follows the Vault HTTP API guidelines. All field names are formatted in snake case (`snake_case`).

## Methods
{: #vault-api-methods}

### Configure a login token
{: #vault-configure-login-token}

Configures the duration or time-to-live (TTL) and lifespan (MaxTTL) of a Vault login token. 

Use a duration string such as `300s` or `2h45m`. Valid time units are `s`, `m`, and `h`. The {{site.data.keyword.cloud_notm}} auth plug-in sets the default login token duration (TTL) to 1 hour, and the default lifespan (MaxTTL) to 24 hours.

#### Parameters
{: #vault-configure-login-token-params}

<dl>
    <dt><code>token_max_ttl</code></dt>
    <dd>The maximum lifetime of the login token. Default is `24h`. This value can't exceed the Vault <code>MaxLeaseTTL</code> value.</dd>
    <dt><code>token_ttl</code></dt> 
    <dd>The initial time-to-live (TTL) of the login token to generate. Default is `1h`.</dd>
</dl>

#### Sample request
{: #vault-configure-login-token-request}

```sh
curl --location --request PUT 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/login' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault_token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "token_ttl": "30m",
    "token_max_ttl": "2h"
}'
```
{: pre}

#### Sample response
{: #vault-configure-login-token-response}

This operation returns `HTTP 204 No Content`.

### Read the configuration of a login token
{: #vault-read-token-config}

Reads the login configuration of a Vault token.

#### Sample request
{: #vault-read-token-config-request}

```sh
curl --location --request GET 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/login' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}'
```
{: pre}

#### Sample response
{: #vault-read-token-config-response}

```json
{
    "request_id": "41bc89dc-c950-113f-aa8f-a025646d2975",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "login": {
            "token_max_ttl": "2h0m0s",
            "token_ttl": "30m0s"
        }
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Log in to Vault
{: #vault-login}

Logs in to Vault by using an {{site.data.keyword.cloud_notm}} IAM token and obtains a Vault token with mapped policies.

#### Parameters
{: #vault-login-params}

<dl>
    <dt><code>iam_token</code></dt>
    <dd>Your {{site.data.keyword.cloud_notm}} IAM access token.</dd>
</dl>

#### Sample request
{: #vault-login-request}

```sh
curl --location --request PUT 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/login' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data-raw '{
    "token": "{iam_token}"
}'
```
{: pre}

#### Sample response
{: #vault-login-response}

```json
{
    "request_id": "d9a41bfe-b8ba-8709-f1be-6dbdbc305e07",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": null,
    "wrap_info": null,
    "warnings": null,
    "auth": {
        "client_token": "s.w6vmYTRuEJdzEvVFVYjIEAYG",
        "accessor": "5m6VpELSK42N3sq0yTEuVhn5",
        "policies": [
            "default",
            "instance-reader"
        ],
        "token_policies": [
            "default",
            "instance-reader"
        ],
        "metadata": {
            "bss_acc": "791f5fb10986423e97aa8512f18b7e65",
            "grant_type": "urn:ibm:params:oauth:grant-type:apikey",
            "name": "secrets-manager-test-reader",
            "resource": "crn:v1:bluemix:public:secrets-manager:us-south:a/791f5fb10986423e97aa8512f18b7e65:e415e570-f073-423a-abdc-55de9b58f54e::",
            "user": "iam-ServiceId-b7ebcf90-c7a9-495b-8ce8-bbf33cb95ca0"
        },
        "lease_duration": 3600,
        "renewable": true,
        "entity_id": "336f5725-b98d-e0c6-921a-6041e2d3157d",
        "token_type": "service",
        "orphan": true
    }
}
```
{: screen}

### Create a secret group
{: #vault-create-secret-group}

Creates a secret group.

#### Parameters
{: #vault-create-secret-group-params}

<dl>
    <dt><code>name</code></dt>
    <dd>The human-readable alias that you want to assign to the secret group.</dd>
    <dt><code>description</code></dt> 
    <dd>(Optional) An extended description of the secret group.</dd>
</dl>

#### Sample request
{: #vault-create-secret-group-request}

```sh
curl --location --request PUT 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "my-secret-group",
    "description": "my new group"
}'
```
{: pre}

#### Sample response
{: #vault-create-secret-group-response}

```json
{
    "request_id": "256b98f4-2fa2-1c96-efbc-c058562c1d1b",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-03T09:27:01Z",
        "description": "my new group",
        "id": "b61632aa-ef40-68a2-ddca-de2da2af40a7",
        "name": "my-secret-group",
        "type": "application/secret.group+json",
        "updated_at": ""
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### List secret groups
{: #vault-list-secret-groups}

Lists the secret groups that are available in your {{site.data.keyword.secrets-manager_short}} service instance.

#### Sample request
{: #vault-list-secret-groups-request}

```sh
curl --location --request GET 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}'
```
{: pre}

#### Sample response
{: #vault-create-list-groups-response}

```json
{
    "request_id": "5bce2444-9b41-5940-8da7-a8a48ebd5b23",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "groups": [
            {
                "created_at": "2020-08-03T09:27:01Z",
                "description": "my new group",
                "id": "b61632aa-ef40-68a2-ddca-de2da2af40a7",
                "name": "my-secret-group",
                "type": "application/secret.group+json",
                "updated_at": ""
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Update a secret group
{: #vault-update-secret-group}

Updates the details of an existing secret group.

#### Parameters
{: #vault-update-secret-group-params}

<dl>
    <dt><code>id</code></dt>
    <dd>The ID of the secret group.</dd>
    <dt><code>name</code></dt>
    <dd>The human-readable alias that you want to assign to the secret group.</dd>
    <dt><code>description</code></dt> 
    <dd>(Optional) An extended description of the secret group.</dd>
</dl>

#### Sample request
{: #vault-update-secret-group-request}

```sh
curl --location --request POST 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/{id}' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "my-new-secret-group",
    "description": "my newer group"
}'
```
{: pre}

#### Sample response
{: #vault-update-secret-group-response}

```json
{
    "request_id": "a35b62a8-764e-8309-10cd-157d9fa412dc",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-03T09:27:01Z",
        "description": "my newer group",
        "id": "b61632aa-ef40-68a2-ddca-de2da2af40a7",
        "name": "my-new-secret-group",
        "type": "application/secret.group+json",
        "updated_at": "2020-08-03T10:37:29Z"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Get a secret group
{: #vault-get-secret-group}

Retrieves a secret group and its details.

#### Parameters
{: #vault-get-secret-group-params}

<dl>
    <dt><code>id</code></dt>
    <dd>The ID of the secret group.</dd>
</dl>

#### Sample request
{: #vault-get-secret-group-request}

```sh
curl --location --request GET 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/{id}' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}
```
{: pre}

#### Sample response
{: #vault-get-secret-group-response}

```json
{
    "request_id": "cd9d4cf9-4bb2-1e4d-69ce-3dbd9b43becc",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-03T09:27:01Z",
        "description": "my newer group",
        "id": "b61632aa-ef40-68a2-ddca-de2da2af40a7",
        "name": "my-new-secret-group",
        "type": "application/secret.group+json",
        "updated_at": "2020-08-03T10:37:29Z"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Delete a secret group
{: #vault-delete-secret-group}

Deletes a secret group. 

#### Parameters
{: #vault-delete-secret-group-params}

<dl>
    <dt><code>id</code></dt>
    <dd>The ID of the secret group.</dd>
</dl>

#### Sample request
{: #vault-delete-secret-group-request}

```sh
curl --location --request DELETE 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/9894c07f-89aa-920f-447e-82c32b2200ae' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}'
```

#### Sample response
{: #vault-delete-secret-group-response}

```json
{
    "request_id": "9d90136f-9eeb-eb93-ee3c-a729c708fa14",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-04T08:05:45Z",
        "description": "my new group",
        "id": "9894c07f-89aa-920f-447e-82c32b2200ae",
        "name": "my-secret-group",
        "type": "application/secret.group+json",
        "updated_at": ""
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Create a secret
{: #vault-create-secret}

Creates a secret by using the {{site.data.keyword.secrets-manager_short}} secret engines.

#### Sample requests and responses
{: #vault-create-secret-request-response}

Create a secret in the `default` secret group:

```sh
curl --location --request PUT 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "dave",
    "payload": "my arbitrary secret"
}'
```
{: pre}

```json
{
    "request_id": "a0627556-112d-7733-2000-87effeed7ed8",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-04T10:05:41Z",
        "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:49d8d88b-d2ee-4861-83f0-662172b550b5",
        "description": "",
        "expiration_date": "",
        "id": "49d8d88b-d2ee-4861-83f0-662172b550b5",
        "name": "dave",
        "payload": "my arbitrary secret",
        "state": 1,
        "stateDescription": "Active",
        "tags": [],
        "type": "ARBITRARY",
        "updated_at": "2020-08-04T10:05:41Z",
        "versions": [
            {
                "id": "601d6a83-56a6-c2d9-b0dc-d5bea4f29d9f",
                "updated_at": "2020-08-04T10:05:41.293237Z"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

Create a secret in an existing secret group:

```sh
curl --location --request PUT 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/c481f146-aa9f-5b9b-55b7-f9fd326027cd' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "dave",
    "payload": "my arbitrary secret"
}'
```
{: pre}

```json
{
    "request_id": "a0627556-112d-7733-2000-87effeed7ed8",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_at": "2020-08-04T10:05:41Z",
        "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:49d8d88b-d2ee-4861-83f0-662172b550b5",
        "description": "",
        "expiration_date": "",
        "group_id": "c481f146-aa9f-5b9b-55b7-f9fd326027cd",
        "id": "49d8d88b-d2ee-4861-83f0-662172b550b5",
        "name": "dave",
        "payload": "my arbitrary secret",
        "state": 1,
        "stateDescription": "Active",
        "tags": [],
        "type": "ARBITRARY",
        "updated_at": "2020-08-04T10:05:41Z",
        "versions": [
            {
                "id": "601d6a83-56a6-c2d9-b0dc-d5bea4f29d9f",
                "updated_at": "2020-08-04T10:05:41.293237Z"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### List secrets
{: #vault-list-secrets}

Retrieve a list of secrets that are available in a {{site.data.keyword.secrets-manager_short}} Vault secret engine.

#### Sample requests and responses
{: #vault-list-secrets-request-response}

List all secrets:

```sh
curl --location --request GET 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}'
```
{: pre}

```json
{
    "request_id": "b3dc1219-c296-6936-5433-4384053f740f",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secrets": [
            {
                "created_at": "2020-08-04T10:05:41Z",
                "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:49d8d88b-d2ee-4861-83f0-662172b550b5",
                "description": "",
                "expiration_date": "",
                "id": "49d8d88b-d2ee-4861-83f0-662172b550b5",
                "name": "dave",
                "state": 1,
                "stateDescription": "Active",
                "tags": [],
                "type": "ARBITRARY",
                "updated_at": "2020-08-04T10:05:41Z"
            },
            {
                "created_at": "2020-08-04T10:11:59Z",
                "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:4ca0715e-c999-34ad-f393-a5f8843ba16a",
                "description": "",
                "expiration_date": "",
                "id": "4ca0715e-c999-34ad-f393-a5f8843ba16a",
                "name": "dave",
                "state": 1,
                "stateDescription": "Active",
                "tags": [],
                "type": "ARBITRARY",
                "updated_at": "2020-08-04T10:11:59Z"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

List secrets in a secret group:

```sh
curl --location --request GET 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/c481f146-aa9f-5b9b-55b7-f9fd326027cd' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' 
```
{: pre}

```json
{
    "request_id": "b3dc1219-c296-6936-5433-4384053f740f",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secrets": [
            {
                "created_at": "2020-08-04T10:05:41Z",
                "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:49d8d88b-d2ee-4861-83f0-662172b550b5",
                "description": "",
                "expiration_date": "",
                "id": "49d8d88b-d2ee-4861-83f0-662172b550b5",
                "name": "dave",
                "state": 1,
                "stateDescription": "Active",
                "tags": [],
                "type": "ARBITRARY",
                "updated_at": "2020-08-04T10:05:41Z"
            },
            {
                "created_at": "2020-08-04T10:11:59Z",
                "crn": "crn:v1:bluemix:public:secret-manager:us-south:a/a2b41867c8294f612fd4c7fd10d39586:1ec90496-358e-4b51-b8b5-ecdcd44fb7dc:secret:arbitrary:4ca0715e-c999-34ad-f393-a5f8843ba16a",
                "description": "",
                "expiration_date": "",
                "id": "4ca0715e-c999-34ad-f393-a5f8843ba16a",
                "name": "dave",
                "state": 1,
                "stateDescription": "Active",
                "tags": [],
                "type": "ARBITRARY",
                "updated_at": "2020-08-04T10:11:59Z"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Delete a secret
{: #vault-delete-secret}

Deletes a secret from a {{site.data.keyword.secrets-manager_short}} Vault secret engine.

#### Sample requests and responses
{: #vault-delete-secret-request-response}

Delete a secret in the `default` secret group:

```sh
curl --location --request DELETE 'https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}' \
--header 'Accept: application/json' \
--header 'X-Vault-Token: {Vault-Token}' 
```
{: pre}

