---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-22"

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

# Vault API
{: #vault-api}

If you're already using the HashiCorp Vault HTTP API, you can use its API format and guidelines to interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

To use the standard REST API for {{site.data.keyword.secrets-manager_short}}, check out the [{{site.data.keyword.secrets-manager_short}} API reference](/apidocs/secrets-manager){: external}.

## Overview
{: #vault-api-overview}

{{site.data.keyword.secrets-manager_short}} uses a custom version of open source HashiCorp Vault. This custom version adds the {{site.data.keyword.cloud_notm}} IAM Auth method and a set of secrets engines to support operations in {{site.data.keyword.secrets-manager_short}} for various secret types.

All operations follow the REST API standards that are available for the Vault HTTP APIs. For more information about how to authenticate and use the Vault HTTP APIs, check out the [Vault documentation](https://www.vaultproject.io/api-docs/index){: external}.

Plug-ins and other components that are offered by the open source Vault community might not be accessible by {{site.data.keyword.secrets-manager_short}}. For more information, see the [FAQs](/docs/secrets-manager?topic=secrets-manager-faqs#faq-differences-vault).
{: important}

### Endpoint URLs
{: #vault-api-base-url}

To access {{site.data.keyword.secrets-manager_short}} by using the Vault APIs, use the dedicated endpoint URL that is unique to your {{site.data.keyword.secrets-manager_short}} service instance.

The following table lists the endpoint URLs by region that can be used to interact with the Vault APIs.

| Region        | Endpoint URL                                                     |
| ------------- | ---------------------------------------------------------------- |
| Dallas        | `https://{instance_id}.us-south.secrets-manager.appdomain.cloud` |
| Washington DC | `https://{instance_id}.us-east.secrets-manager.appdomain.cloud`  |
| London        | `https://{instance_id}.eu-gb.secrets-manager.appdomain.cloud`    |
| Frankfurt     | `https://{instance_id}.eu-de.secrets-manager.appdomain.cloud`    |
| Sydney        | `https://{instance_id}.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_id}.jp-tok.secrets-manager.appdomain.cloud`   |
{: caption="Table 1. Public endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #public-endpoints-vault}
{: tab-title="Public endpoints"}
{: tab-group="vault-endpoint-urls"}
{: class="simple-tab-table"}

| Region        | Endpoint URL                                                             |
| ------------- | ------------------------------------------------------------------------ |
| Dallas        | `https://{instance_id}.private.us-south.secrets-manager.appdomain.cloud` |
| Washington DC | `https://{instance_id}.private.us-east.secrets-manager.appdomain.cloud`  |
| London        | `https://{instance_id}.private.eu-gb.secrets-manager.appdomain.cloud`    |
| Frankfurt     | `https://{instance_id}.private.eu-de.secrets-manager.appdomain.cloud`    |
| Sydney        | `https://{instance_id}.private.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_id}.private.jp-tok.secrets-manager.appdomain.cloud`   |
{: caption="Table 1. Private endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #private-endpoints-vault}
{: tab-title="Private endpoints"}
{: tab-group="vault-endpoint-urls"}
{: class="simple-tab-table"}

You can find your unique endpoint URL in the **Endpoints** page of the {{site.data.keyword.secrets-manager_short}} UI, or by retrieving it by HTTP request. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls).
{: tip}

### Common headers
{: #vault-api-headers}

This section describes the headers that are common to all requests.

| Header          | Description                                                                         |
| --------------- | ----------------------------------------------------------------------------------- |
| `X-Vault-Token` | **Required.** A valid Vault token with sufficient permissions to perform the operation. |
| `Content-Type`  | **Required.** `application/json`                                                        |
{: caption="Table 2. Common headers" caption-side="top"}

### Timestamps
{: #vault-api-timestamps}

The timestamps in all requests and responses, such as creation and expiration dates, are formatted according to [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339){: external}. For example: `1985-04-12T23:20:50.52Z`

### Field names
{: #vault-field-names}

This API follows the Vault HTTP API guidelines. All field names are formatted in snake case (`snake_case`).

## Login
{: #vault-api-login}

### Log in to Vault
{: #vault-login}

Logs in to Vault by using an {{site.data.keyword.cloud_notm}} IAM token and obtains a Vault token with mapped policies.

#### Parameters
{: #vault-login-params}

| Name    | Description                                    |
| ------- | ---------------------------------------------- |
| `token` | **Required.** Your {{site.data.keyword.cloud_notm}} IAM access token. |
{: caption="Table 3. Login request parameters" caption-side="top"}


#### Example request
{: #vault-login-request}

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/login" \
-H 'Accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
  "token": "{IAM_token}"
}'
```
{: codeblock}

#### Example response
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

### Configure a login token
{: #vault-configure-login-token}

Configures the duration or time-to-live (TTL) and lifespan (MaxTTL) of a Vault login token.

Use a duration string such as `300s` or `2h45m`. Valid time units are `s`, `m`, and `h`. The {{site.data.keyword.cloud_notm}} auth plug-in sets the default login token duration (TTL) to 1 hour, and the default lifespan (MaxTTL) to 24 hours.

#### Parameters
{: #vault-configure-login-token-params}

| Name            | Description                                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------- |
| `token_max_ttl` | The maximum lifetime of the login token. Default is `24h`. This value can't exceed the Vault `MaxLeaseTTL` value. |
| `token_ttl`     | The initial time-to-live (TTL) of the login token to generate. Default is `1h`.                                   |
{: caption="Table 3. Configure login token request parameters" caption-side="top"}


#### Example request
{: #vault-configure-login-token-request}

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/login" \
-H 'Accept: application/json' \
-H 'X-Vault-Token: {Vault_token}' \
-H 'Content-Type: application/json' \
-d '{
  "token_ttl": "30m",
  "token_max_ttl": "2h"
}'
```
{: codeblock}

#### Example response
{: #vault-configure-login-token-response}

This operation returns `HTTP 204 No Content`.

### Get the configuration of a login token
{: #vault-read-token-config}

Retrieves the login configuration of a Vault token.

#### Example request
{: #vault-read-token-config-request}

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/login" \
-H 'Accept: application/json' \
-H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
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

## Secret groups
{: #vault-api-secret-groups}

### Create a secret group
{: #vault-create-secret-group}

Creates a secret group.

#### Parameters
{: #vault-create-secret-group-params}

| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret group. |
| `description` | An extended description of the secret group.                                        |
{: caption="Table 4. Create secret group request parameters" caption-side="top"}


#### Example request
{: #vault-create-secret-group-request}

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-secret-group",
    "description": "Extended description for my secret group."
  }'
```
{: codeblock}

#### Example response
{: #vault-create-secret-group-response}

```json
{
    "request_id": "f0e47267-940e-1a59-8742-e4e77401b06b",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "creation_date": "2020-12-15T22:08:46Z",
        "description": "Extended description for my secret group.",
        "id": "2bcaa289-5d38-aa57-910d-970e418ab1b3",
        "last_update_date": "2020-12-15T22:08:46Z",
        "name": "test-secret-group",
        "type": "application/vnd.ibm.secrets-manager.secret.group+json"
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

#### Example request
{: #vault-list-secret-groups-request}

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-list-secret-groups-response}

```json
{
    "request_id": "7ecc32f2-b78b-9290-015c-24803a1e87c9",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "groups": [
            {
                "creation_date": "2020-12-14T14:48:55Z",
                "description": "Read and write to Cloud Object storage buckets.",
                "id": "714e070d-8122-6270-198c-fef9166729e3",
                "last_update_date": "2020-12-14T14:48:55Z",
                "name": "cloud-object-storage-writers",
                "type": "application/vnd.ibm.secrets-manager.secret.group+json"
            },
            {
                "creation_date": "2020-12-15T22:08:46Z",
                "description": "Extended description for my secret group.",
                "id": "2bcaa289-5d38-aa57-910d-970e418ab1b3",
                "last_update_date": "2020-12-15T22:08:46Z",
                "name": "test-secret-group",
                "type": "application/vnd.ibm.secrets-manager.secret.group+json"
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

| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret group. |
| `description` | An extended description of the secret group.                                        |
{: caption="Table 5. Update secret group request parameters" caption-side="top"}


#### Example request
{: #vault-update-secret-group-request}

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "updated-secret-group-name",
    "description": "Updated description for my secret group"
  }'
```
{: codeblock}

#### Example response
{: #vault-update-secret-group-response}

```json
{
    "request_id": "b02c5035-9da1-85fe-b7c7-3db2c77ddbb6",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "creation_date": "2020-12-15T22:08:46Z",
        "description": "Updated description for my secret group.",
        "id": "2bcaa289-5d38-aa57-910d-970e418ab1b3",
        "last_update_date": "2020-12-15T22:16:32Z",
        "name": "updated-secret-group-name",
        "type": "application/vnd.ibm.secrets-manager.secret.group+json"
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

#### Example request
{: #vault-get-secret-group-request}

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-get-secret-group-response}

```json
{
    "request_id": "0d127ae6-8359-bc36-af53-3a56be4c3e24",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "creation_date": "2020-12-15T22:08:46Z",
        "description": "Updated description for my secret group.",
        "id": "2bcaa289-5d38-aa57-910d-970e418ab1b3",
        "last_update_date": "2020-12-15T22:18:44Z",
        "name": "updated-secret-group-name",
        "type": "application/vnd.ibm.secrets-manager.secret.group+json"
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


#### Example request
{: #vault-delete-secret-group-request}

```sh
curl -X DELETE "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-delete-secret-group-response}

```json
{
    "request_id": "37065859-3238-f671-941f-d43ac340ad99",
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

## Secrets
{: #vault-api-secrets}

### Create a secret
{: #vault-create-secret}

Creates or imports a secret by using the {{site.data.keyword.secrets-manager_short}} secrets engines. For more information about secret types that you can manage in the service, see [Working with secrets of different types](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

#### Parameters
{: #vault-create-secret-params}

| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `payload`     | The secret data to assign to an `arbitrary` secret. |
| `username`    | The username to assign to a `username_password` secret.|
| `password`    | The password to assign to a `username_password` secret. |
| `expiration_date` | The expiration date that you want to assign to the secret. This option is supported for the `arbitrary` and `username_password` secret types. The date format follows [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339).|
| `certificate` | The certificate data to assign to an `imported_cert` secret. |
| `private_key` | The matching private key to assign to an `imported_cert` secret. |
| `intermediate` | The intermediate certificate data to assign to an `import_cert` secret.|
{: caption="Table 6. Create secret request parameters" caption-side="top"}



#### Example requests
{: #vault-create-secret-request}

Create an arbitrary secret in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-arbitrary-secret",
    "description": "Extended description for my secret."
    "payload": "secret-data",
    "labels": [
      "dev",
      "us-south"
    ],
    "expiration_date": "2030-04-01T09:30:00Z"
  }'
```
{: codeblock}

Create an arbitrary secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-arbitrary-secret-in-group",
    "description": "Extended description for my secret.",
    "payload": "secret-data",
    "labels": [
      "dev",
      "us-south"
    ],
    "expiration_date": "2030-04-01T09:30:00Z"
  }'
```
{: codeblock}

Create IAM credentials in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/roles/{secret_name}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-iam-credentials",
    "description": "Extended description for my secret.",
    "access_groups": [
        "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
    ],
    "labels": [
        "dev",
        "us-south"
    ],
    "ttl": "30m"
  }'
```
{: codeblock}

Create IAM credentials in an existing group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/roles/groups/{group_id}/{secret_name}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-iam-credentials-in-group",
    "description": "Extended description for my secret.",
    "access_groups": [
        "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
    ],
    "labels": [
        "dev",
        "us-south"
    ],
    "ttl": "30m"
  }'
```
{: codeblock}

Create user credentials in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-username-password",
    "description": "Extended description for my secret.",
    "username": "user123",
    "password": "cloudy-rainy-coffee-book",
    "expiration_date": "2020-12-31T00:00:00Z",
    "labels": [
      "dev",
      "us-south"
    ]
  }'
```
{: codeblock}

Create user credentials in an existing secret group:

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-username-password-in-group",
    "description": "Extended description for my secret.",
    "username": "user123",
    "password": "cloudy-rainy-coffee-book",
    "expiration_date": "2020-12-31T00:00:00Z",
    "labels": [
      "dev",
      "us-south"
    ]
  }'
```
{: codeblock}

Import a TLS certificate and assign it to the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/imported_cert/secrets" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-imported-certificate",
    "description": "Extended description for my secret."
    "certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)",
    "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)",
    "intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...(redacted)",
    "labels": [
      "dev",
      "us-south"
    ]
  }'
```
{: codeblock}

Import a TLS certificate and assign it to an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/imported_cert/secrets/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-imported-certificate-in-group",
    "description": "Extended description for my secret."
    "certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)",
    "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)",
    "intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...(redacted)",
    "labels": [
      "dev",
      "us-south"
    ]
  }'
```
{: codeblock}

#### Example responses
{: #vault-create-secret-response}

A request to create an arbitrary secret in the `default` secret group returns the following response:

```json
{
  "request_id": "8c047529-de3a-a79d-7c2f-c382a8e75312",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-15T22:34:53Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:a6972127-35ad-b36f-aac8-0223f0475cb6",
    "description": "Extended description for my secret.",
    "expiration_date": "2030-04-01T09:30:00Z",
    "id": "a6972127-35ad-b36f-aac8-0223f0475cb6",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-15T22:34:53Z",
    "name": "test-arbitrary-secret-in-group",
    "secret_data": {
      "payload": "secret-data"
    },
    "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
    "secret_type": "arbitrary",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
          "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
          "creation_date": "2020-12-15T22:34:53Z",
          "id": "a7f55e6f-b068-977b-062e-4de644633982"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to create an arbitrary secret in an existing secret group returns the following response:

```json
{
  "request_id": "8c047529-de3a-a79d-7c2f-c382a8e75312",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-15T22:34:53Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:a6972127-35ad-b36f-aac8-0223f0475cb6",
    "description": "Extended description for my secret.",
    "expiration_date": "2030-04-01T09:30:00Z",
    "id": "a6972127-35ad-b36f-aac8-0223f0475cb6",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-15T22:34:53Z",
    "name": "test-arbitrary-secret-in-group",
    "secret_data": {
      "payload": "secret-data"
    },
    "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
    "secret_type": "arbitrary",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-15T22:34:53Z",
        "id": "a7f55e6f-b068-977b-062e-4de644633982"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to create IAM credentials in the `default` secret group returns the following response:

```json
{
    "request_id": "3bef24c5-5ab9-72f4-8a1a-dd35a6e7aa15",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "access_groups": [
            "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
        ],
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-16T21:34:51Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:8bc5eae8-e5b7-9599-de8e-525d1c3e2723",
        "description": "Extended description for my secret.",
        "id": "8bc5eae8-e5b7-9599-de8e-525d1c3e2723",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2020-12-16T21:34:51Z",
        "name": "test-iam-credentials",
        "secret_type": "iam_credentials",
        "state": 1,
        "state_description": "Active",
        "ttl": 1800
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to create IAM credentials in an existing secret group returns the following response:

```json
{
  "request_id": "2278a441-6dbe-5ee8-4a4b-3b5b1e814231",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "access_groups": [
      "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
    ],
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-16T21:57:13Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:99425779-0707-4877-81CB-ca11e28b6ef1",
    "description": "Extended description for my secret.",
    "id": "99425779-0707-4877-81CB-ca11e28b6ef1",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-16T21:57:13Z",
    "name": "test-iam-credentials-in-group",
    "secret_group_id": "714e070d-8122-6270-198c-fef9166729e3",
    "secret_type": "iam_credentials",
    "state": 1,
    "state_description": "Active",
    "ttl": 1800
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to create user credentials in the `default` secret group returns the following response:

```json
{
  "request_id": "96fc9603-5aff-5daa-f25c-efc3599b374b",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-15T22:43:36Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:2bd4c8fc-c1e4-f9d7-8026-6c04610f051f",
    "description": "Extended description for my secret.",
    "expiration_date": "2020-12-31T00:00:00Z",
    "id": "2bd4c8fc-c1e4-f9d7-8026-6c04610f051f",
    "labels": [
        "dev",
        "us-south"
    ],
    "last_update_date": "2020-12-15T22:43:36Z",
    "name": "test-username-password",
    "secret_data": {
      "password": "cloudy-rainy-coffee-book",
      "username": "user123"
    },
    "secret_type": "username_password",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
        "auto_rotated": false,
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-15T22:43:36Z",
        "id": "ae4b3afd-5e63-5951-790b-f1892e8c5267"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to create user credentials in an existing secret group returns the following response:

```json
{
  "request_id": "4ccc9dd5-af3a-6865-293f-3f704d2866e1",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-15T22:46:41Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
    "description": "Extended description for my secret.",
    "expiration_date": "2020-12-31T00:00:00Z",
    "id": "be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-15T22:46:41Z",
    "name": "test-username-password-in-group",
    "secret_data": {
      "password": "cloudy-rainy-coffee-book",
      "username": "user123"
    },
    "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
    "secret_type": "username_password",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
          "auto_rotated": false,
          "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
          "creation_date": "2020-12-15T22:46:41Z",
          "id": "a09c7a3c-13a5-7a17-fadc-e7850496d27a"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to import a certificate to the `default` secret group returns the following response:

```json
{
    "request_id": "811b893b-55c7-b6bb-4e55-26a7a8362164",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "algorithm": "RSA",
        "common_name": "example.com",
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2021-06-03T20:50:11Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "description": "Extended description for my secret.",
        "expiration_date": "2021-06-04T15:25:44Z",
        "id": "be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "intermediate_included": false,
        "issuer": "US Texas Austin Example Corp. Example Org example.com",
        "key_algorithm": "SHA256-RSA",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2021-06-03T20:50:11Z",
        "name": "test-imported-certificate-in-group",
        "private_key_included": true,
        "secret_type": "imported_cert",
        "serial_number": "fc:22:29:7e:57:25:8a:05",
        "state": 1,
        "state_description": "Active",
        "validity": {
            "not_after": "2021-06-04T15:25:44Z",
            "not_before": "2021-06-03T15:25:44Z"
        },
        "versions": [
            {
                "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
                "creation_date": "2021-06-03T20:50:11.278296706Z",
                "expiration_date": "2021-06-04T15:25:44Z",
                "id": "be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
                "serial_number": "fc:22:29:7e:57:25:8a:05",
                "validity": {
                    "not_after": "2021-06-04T15:25:44Z",
                    "not_before": "2021-06-03T15:25:44Z"
                }
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to import a certificate to an existing secret group returns the following response:

```json
{
    "request_id": "811b893b-55c7-b6bb-4e55-26a7a8362164",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "algorithm": "RSA",
        "common_name": "example.com",
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2021-06-03T20:50:11Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "description": "Extended description for my secret.",
        "expiration_date": "2021-06-04T15:25:44Z",
        "id": "be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "intermediate_included": false,
        "issuer": "US Texas Austin Example Corp. Example Org example.com",
        "key_algorithm": "SHA256-RSA",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2021-06-03T20:50:11Z",
        "name": "test-imported-certificate-in-group",
        "private_key_included": true,
        "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
        "secret_type": "imported_cert",
        "serial_number": "fc:22:29:7e:57:25:8a:05",
        "state": 1,
        "state_description": "Active",
        "validity": {
            "not_after": "2021-06-04T15:25:44Z",
            "not_before": "2021-06-03T15:25:44Z"
        },
        "versions": [
            {
                "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
                "creation_date": "2021-06-03T20:50:11.278296706Z",
                "expiration_date": "2021-06-04T15:25:44Z",
                "id": "e4f44e8b-abe0-9267-88da-199e754f974a",
                "serial_number": "fc:22:29:7e:57:25:8a:05",
                "validity": {
                    "not_after": "2021-06-04T15:25:44Z",
                    "not_before": "2021-06-03T15:25:44Z"
                }
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}



### Get a secret
{: #vault-get-secret}

Get the value of a secret.

#### Example requests
{: #vault-get-secret-request}

Get an arbitrary secret in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get an arbitrary secret in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get IAM credentials in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/creds/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get IAM credentials in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/creds/groups/{group_id}/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get user credentials in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get user credentials in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/{secret_id}/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get an imported certificate in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/imported_cert/secrets/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get an imported certificate in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/imported_cert/secrets/{secret_id}/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example responses
{: #vault-get-secret-response}

A request to retrieve an arbitrary secret in the `default` secret group returns the following response:

```json
{
  "request_id": "463e84e8-3a0c-1061-1a6e-6ce1434c7ba2",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-16T20:54:52Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:582a8f65-9a2b-a072-4fc3-e69ff3462c23",
    "description": "Extended description for my secret.",
    "expiration_date": "2030-04-01T09:30:00Z",
    "id": "582a8f65-9a2b-a072-4fc3-e69ff3462c23",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-16T20:54:52Z",
    "name": "test-arbitrary-secret",
    "secret_data": {
      "payload": "secret-data"
    },
    "secret_type": "arbitrary",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-16T20:54:52Z",
        "id": "03d9ddb3-aa1d-d929-40c8-04027213ef08"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to retrieve an arbitrary secret in an existing secret group returns the following response:

```json
{
  "request_id": "791340bd-5664-c1e3-e779-d1391494f55d",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-15T22:34:53Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:a6972127-35ad-b36f-aac8-0223f0475cb6",
    "description": "Extended description for my secret.",
    "expiration_date": "2030-04-01T09:30:00Z",
    "id": "a6972127-35ad-b36f-aac8-0223f0475cb6",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-15T22:34:53Z",
    "name": "test-arbitrary-secret-in-group",
    "secret_data": {
      "payload": "secret-data"
    },
    "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
    "secret_type": "arbitrary",
    "state": 1,
    "state_description": "Active",
    "versions": [
      {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-15T22:34:53Z",
        "id": "a7f55e6f-b068-977b-062e-4de644633982"
      }
    ]
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to generate IAM credentials in the `default` secret group returns the following response:

```json
{
  "request_id": "c9716624-669f-2ef4-5560-a5d4e6618826",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "access_groups": [
      "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
    ],
    "api_key": "U40hERZ0h-0C0cnka2bEuL2yK5Yyz2MoHC8FKeYfcV7Z",
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-16T21:55:31Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:d7a2b83f-997c-4914-857a-86bfcdbf0873",
    "description": "Extended description for my secret.",
    "id": "d7a2b83f-997c-4914-857a-86bfcdbf0873",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-16T22:05:16Z",
    "name": "test-iam-credentials",
    "secret_type": "iam_credentials",
    "service_id": "ServiceId-43c79ec9-7f02-481d-92f1-e60363483298",
    "state": 1,
    "state_description": "Active",
    "ttl": 1800
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to generate IAM credentials in an existing secret group returns the following response:

```json
{
  "request_id": "201eaa80-d5f1-2697-66dd-481d94a52685",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "access_groups": [
      "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
    ],
    "api_key": "CFQY6wWPI3C3wKx6XLC9p0c3eiejr5WXd7lpRGiKr40a",
    "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
    "creation_date": "2020-12-16T21:57:13Z",
    "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:99425779-0707-4877-81CB-ca11e28b6ef1",
    "description": "Extended description for my secret.",
    "id": "99425779-0707-4877-81CB-ca11e28b6ef1",
    "labels": [
      "dev",
      "us-south"
    ],
    "last_update_date": "2020-12-16T22:07:20Z",
    "name": "test-iam-credentials-in-group",
    "secret_group_id": "714e070d-8122-6270-198c-fef9166729e3",
    "secret_type": "iam_credentials",
    "service_id": "ServiceId-d1a99978-2108-4eec-9dae-bdf5691e7136",
    "state": 1,
    "state_description": "Active",
    "ttl": 1800
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

A request to retrieve an imported certificate in the `default` secret group returns the following response:

```json
{
    "request_id": "811b893b-55c7-b6bb-4e55-26a7a8362164",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "algorithm": "RSA",
        "common_name": "example.com",
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2021-06-03T20:50:11Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "description": "Extended description for my secret.",
        "expiration_date": "2021-06-04T15:25:44Z",
        "id": "be4a0846-4cb5-3bfa-bab5-10a44dfc3e85",
        "intermediate_included": true,
        "issuer": "US Texas Austin Example Corp. Example Org example.com",
        "key_algorithm": "SHA256-RSA",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2021-06-03T20:50:11Z",
        "name": "test-imported-certificate",
        "private_key_included": true,
        "secret_data": {
            "certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)",
            "intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...(redacted)",
            "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)"
        },
        "secret_type": "imported_cert",
        "serial_number": "fc:22:29:7e:57:25:8a:05",
        "state": 1,
        "state_description": "Active",
        "validity": {
            "not_after": "2021-06-04T15:25:44Z",
            "not_before": "2021-06-03T15:25:44Z"
        },
        "versions": [
            {
                "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
                "creation_date": "2021-06-03T20:50:11.278296706Z",
                "expiration_date": "2021-06-04T15:25:44Z",
                "id": "e4f44e8b-abe0-9267-88da-199e754f974a",
                "serial_number": "fc:22:29:7e:57:25:8a:05",
                "validity": {
                    "not_after": "2021-06-04T15:25:44Z",
                    "not_before": "2021-06-03T15:25:44Z"
                }
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

Retrieve a list of secrets that are available in a {{site.data.keyword.secrets-manager_short}} secrets engine.

#### Example requests
{: #vault-list-secrets-request}

List arbitrary secrets.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

List arbitrary secrets in an existing secret group:

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example responses
{: #vault-list-secrets-response}

A request to list all arbitrary secrets returns the following response:

```json
{
  "request_id": "d8eb84fd-c0bd-08ae-c3ad-cff87606953c",
  "lease_id": "",
  "renewable": false,
  "lease_duration": 0,
  "data": {
    "secrets": [
      {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-15T22:34:53Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:a6972127-35ad-b36f-aac8-0223f0475cb6",
        "description": "Extended description for my secret.",
        "expiration_date": "2030-04-01T09:30:00Z",
        "id": "a6972127-35ad-b36f-aac8-0223f0475cb6",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2020-12-15T22:34:53Z",
        "name": "test-arbitrary-secret-in-group",
        "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
        "secret_type": "arbitrary",
        "state": 1,
        "state_description": "Active"
      },
      {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2020-12-15T22:41:14Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "description": "Extended description for my secret.",
        "expiration_date": "2030-04-01T09:30:00Z",
        "id": "ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2020-12-15T22:41:14Z",
        "name": "another-arbitrary-secret-in-group",
        "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
        "secret_type": "arbitrary",
        "state": 1,
        "state_description": "Active"
      }
    ],
    "secrets_total": 2
  },
  "wrap_info": null,
  "warnings": null,
  "auth": null
}
```
{: screen}

### Get secret metadata
{: #vault-get-secret-metadata}

Retrieve the metadata of a secret, such as it's name, description. To retrieve the actual value of a secret, use [Get a secret](#vault-get-secret).

#### Example requests
{: #vault-get-secret-metadata-request}

Get metadata for an `arbitrary` secret in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}/metadata" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get metadata for an `arbitrary` secret in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}/{secret_id}/metadata" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example responses
{: #vault-get-secret-metadata-response}

A request to retrieve the metadata of an `arbitrary` secret in the `default` secret group returns the following response:

```json
{
    "request_id": "372645c0-9d97-5f6b-0755-99145eacdb93",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2021-06-04T02:55:40Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "description": "Extended description for my secret.",
        "expiration_date": "2030-04-01T09:30:00Z",
        "id": "ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2021-06-04T02:55:40Z",
        "name": "test-arbitrary-secret",
        "secret_type": "arbitrary",
        "state": 1,
        "state_description": "Active"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Update secret metadata
{: #vault-update-secret-metadata}

Update the metadata of a secret, such as it's name, description, or expiration date. To rotate the actual value of a secret, use [Rotate a secret](#vault-rotate-secret).

#### Parameters
{: #vault-update-secret-params}

| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | The updated name to assign to the secret.                                           |
| `description` | The updated description to assign to the secret.                                    |
| `expiration_date` | The updated expiration date to assign to the secret. This option is supported for the `arbitrary` and `username_password` secret types. The date format follows [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339).|
{: caption="Table 7. Update secret metadata request parameters" caption-side="top"}


#### Example requests
{: #vault-update-secret-metadata-request}

Update the name of an `arbitrary` secret in the `default` secret group.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}/metadata" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "name": "updated-arbitrary-secret-name"
  }'
```
{: codeblock}

Update the expiration date of an `arbitrary` secret in an existing secret group.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}/{secret_id}/metadata" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "expiration_date": "2030-05-01T09:30:00Z"
  }'
```
{: codeblock}

#### Example responses
{: #vault-update-secret-metadata-response}

```json
{
    "request_id": "372645c0-9d97-5f6b-0755-99145eacdb93",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
        "creation_date": "2021-06-04T02:55:40Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:secret:ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "description": "Updated description for my secret.",
        "expiration_date": "2030-04-01T09:30:00Z",
        "id": "ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2021-06-05T02:55:40Z",
        "name": "updated-arbitrary-secret",
        "secret_type": "arbitrary",
        "state": 1,
        "state_description": "Active"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Rotate a secret
{: #vault-rotate-secret}

Create a new version of a secret. The secret retains its identifying information, such as its name and ID. To set an automatic rotation policy for a secret, see [Set secret policies](#vault-set-secret-policies).

#### Parameters
{: #vault-rotate-secret-params}

| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `payload`     | The new secret data to assign to an `arbitrary` secret. |
| `username`    | The new password to assign to a `username_password` secret.|
| `password`    | The password to assign to a `username_password` secret. |
| `certificate` | The new certificate to assign to an `imported_cert` secret. |
| `private_key` | The new private key to assign to an `imported_cert` secret. |
| `intermediate` | The new intermediate certificate data to assign to an `import_cert` secret.|
{: caption="Table 8. Rotate secret request parameters" caption-side="top"}


#### Example requests
{: #vault-rotate-secret-request}

Rotate an `arbitrary` secret in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}/rotate" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "payload": "new-secret-data"
  }'
```
{: codeblock}

Rotate an `arbitrary` secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}/{secret_id}/rotate" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "payload": "new-secret-data"
  }'
```
{: codeblock}

Rotate a `username_password` secret in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/{secret_id}/rotate" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "password": "new-password"
  }'
```
{: codeblock}

Rotate an `imported_cert` secret in the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/imported_cert/secrets/{secret_id}/rotate" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}' \
  -d '{
    "certificate": "new-certificate",
    "private_key": "new-private-key",
    "intermediate": "new-intermediate-certificate"
  }'
```
{: codeblock}

#### Example responses
{: #vault-rotate-secret-response}

### Delete a secret
{: #vault-delete-secret}

Deletes a secret from a {{site.data.keyword.secrets-manager_short}} secrets engine.

#### Example requests
{: #vault-delete-secret-request}

Delete an arbitrary secret in the `default` secret group.

```sh
curl -X DELETE "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Delete an arbitrary secret in an existing secret group.

```sh
curl -X DELETE "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/groups/{group_id}/{secret_id}" \
  -H 'Accept: application/json' \
  -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-delete-secret-response}

```json
{
    "request_id": "e48436e3-23d3-ab4a-7642-535cab8935a8",
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

## Policies
{: #vault-api-secret-policies}

### Set secret policies
{: #vault-set-secret-policies}

Creates or updates an [automatic rotation policy](/docs/secrets-manager?topic=secrets-manager-rotate-secrets#auto-rotate-secret) for a secret. Supported secret types include: `username_password`

#### Parameters
{: #vault-set-secret-policies-params}


| Name          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `interval`     | The length of the secret rotation time interval. |
| `unit`    | The units for the secret rotation time interval. Allowable values include: day, month|
{: caption="Table 9. Set secret policy request parameters" caption-side="top"}


#### Example request
{: #vault-set-secret-policies-request}

Set a rotation policy on an `username_password` secret in the `default` secret group.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/secrets/{secret_id}/policies" \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  --data-raw '{
      "policies": [
          {
              "rotation": {
                  "interval": 10,
                  "unit": "day"
              },
              "type": "application/vnd.ibm.secrets-manager.secret.policy+json"
          }
      ]
  }'
```
{: codeblock}

Set a rotation policy on a `username_password` secret in an existing secret group.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/groups/{group_id}/{secret_id}/policies" \
  -H 'X-Vault-Token: {Vault-Token}' \
  -H 'Content-Type: application/json' \
  -d '{
      "policies": [
          {
              "rotation": {
                  "interval": 10,
                  "unit": "day"
              },
              "type": "application/vnd.ibm.secrets-manager.secret.policy+json"
          }
      ]
  }'
```
{: codeblock}

#### Example response
{: #vault-set-secret-policies-response}

```json
{
    "request_id": "89698bdc-d787-4a74-eb2f-53e055ddc7f3",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "policies": [
            {
                "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
                "creation_date": "2021-06-21T14:30:17Z",
                "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:policy:ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
                "id": "ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
                "last_update_date": "2021-06-21T14:33:41Z",
                "rotation": {
                    "interval": 10,
                    "unit": "day"
                },
                "type": "application/vnd.ibm.secrets-manager.secret.policy+json",
                "updated_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{:screen}

### List secret policies
{: #vault-list-secret-policies}

Retrieves a list of policies that are associated with a secret.

#### Example request
{: #vault-list-secret-policies-request}

List the policies for an `username_password` secret in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/{secret_id}/policies" \
  -H 'X-Vault-Token: {Vault-Token}' 
```
{: codeblock}

List the policies for a `username_password` secret in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/secrets/groups/{group_id}/{secret_id}/policies" \
  -H 'X-Vault-Token: {Vault-Token}' 
```
{: codeblock}

#### Example response
{: #vault-list-secret-policies-response}

```json
{
    "request_id": "89698bdc-d787-4a74-eb2f-53e055ddc7f3",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "policies": [
            {
                "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
                "creation_date": "2021-06-21T14:30:17Z",
                "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0:policy:ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
                "id": "ea1907c8-8c8e-6b83-3c20-05f2015b80d8",
                "last_update_date": "2021-06-21T14:33:41Z",
                "rotation": {
                    "interval": 10,
                    "unit": "day"
                },
                "type": "application/vnd.ibm.secrets-manager.secret.policy+json",
                "updated_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0"
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{:screen}

