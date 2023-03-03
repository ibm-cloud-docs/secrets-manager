---

copyright:
  years: 2020, 2023
lastupdated: "2023-03-01"

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
{:terraform: .ph data-hd-interface="terraform"}
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

# Vault API
{: #vault-api}

If you're already using the HashiCorp Vault HTTP API, you can use its API format and guidelines to interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

To use the standard REST API for {{site.data.keyword.secrets-manager_short}}, check out the [{{site.data.keyword.secrets-manager_short}} API reference](/apidocs/secrets-manager){: external}.

## Overview
{: #vault-api-overview}

{{site.data.keyword.secrets-manager_short}} uses a custom version of open source HashiCorp Vault. This custom version adds the {{site.data.keyword.cloud_notm}} IAM Auth method and a set of secrets engines to support operations in {{site.data.keyword.secrets-manager_short}} for various secret types.

All operations follow the REST API standards that are available for the Vault HTTP APIs. For more information about how to authenticate and use the Vault HTTP APIs, check out the [Vault documentation](https://www.vaultproject.io/api-docs/index){: external}.

{{site.data.keyword.secrets-manager_short}} limits Vault access to only specific paths that help you to work with secrets and log in to your instance. All other paths return an HTTP `403 Forbidden` response status code. Plug-ins and other components that are offered by the open source Vault community might not be accessible by {{site.data.keyword.secrets-manager_short}}. For more information, see the [FAQs](/docs/secrets-manager?topic=secrets-manager-faqs#faq-differences-vault).
{: important}

### Endpoint URLs
{: #vault-api-base-url}

To access {{site.data.keyword.secrets-manager_short}} by using the Vault APIs, use the dedicated endpoint URL that is unique to your {{site.data.keyword.secrets-manager_short}} service instance.

The following table lists the endpoint URLs by region that can be used to interact with the Vault APIs.

| Region        | Endpoint URL                                                     |
| ------------- | ---------------------------------------------------------------- |
| Dallas        | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt     | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud`    |
| London        | `https://{instance_ID}.eu-gb.secrets-manager.appdomain.cloud`    |
| Osaka         | `https://{instance_ID}.jp-osa.secrets-manager.appdomain.cloud`   |
| Sao Paulo     | `https://{instance_ID}.br-sao.secrets-manager.appdomain.cloud`   |
| Sydney        | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_ID}.jp-tok.secrets-manager.appdomain.cloud`   |
| Toronto       | `https://{instance_ID}.ca-tor.secrets-manager.appdomain.cloud`   |
| Washington DC | `https://{instance_ID}.us-east.secrets-manager.appdomain.cloud`  |
{: caption="Table 1. Public endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #public-endpoints-vault}
{: tab-title="Public endpoints"}
{: tab-group="vault-endpoint-urls"}
{: class="simple-tab-table"}

| Region        | Endpoint URL                                                             |
| ------------- | ------------------------------------------------------------------------ |
| Dallas        | `https://{instance_ID}.private.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt     | `https://{instance_ID}.private.eu-de.secrets-manager.appdomain.cloud`    |
| London        | `https://{instance_ID}.private.eu-gb.secrets-manager.appdomain.cloud`    |
| Osaka         | `https://{instance_ID}.private.jp-osa.secrets-manager.appdomain.cloud`   |
| Sao Paulo     | `https://{instance_ID}.private.br-sao.secrets-manager.appdomain.cloud`   |
| Sydney        | `https://{instance_ID}.private.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_ID}.private.jp-tok.secrets-manager.appdomain.cloud`   |
| Toronto       | `https://{instance_ID}.private.ca-tor.secrets-manager.appdomain.cloud`   |
| Washington DC | `https://{instance_ID}.private.us-east.secrets-manager.appdomain.cloud`  |
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

| Request parameters      | Description                                    |
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


| Request parameters             | Description                                                                                                       |
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


| Request parameters          | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret group. |
| `description` | An extended description of the secret group.                                        |
{: caption="Table 4. Create secret group request parameters" caption-side="top"}


#### Example request
{: #vault-create-secret-group-request}

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/auth/ibmcloud/manage/groups" \
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

| Request parameters            | Description                                                                         |
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

Creates or imports a secret by using the {{site.data.keyword.secrets-manager_short}} secrets engines. You can add one of the following [secret types](/docs/secrets-manager?topic=secrets-manager-what-is-secret):

- Arbitrary secrets (`arbitrary`)
- IAM credentials (`iam_credentials`)
- Key-value secrets (`kv`)
- User credentials (`user_credentials`)
- Imported certificates (`import_cert`)
- Private certificates (`private_cert`)
- Public certificates (`public_cert`)


| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `payload`     | **Required.** The secret data to assign to the secret. |
| `expiration_date` | The expiration date that you want to assign to the secret. The date format follows [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339).|
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - Arbitrary secrets" caption-side="top"}
{: #vault-create-secret-params-arbitrary}
{: tab-title="Arbitrary secrets"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `access_groups[]`    | **Required.** The access groups that define the capabilities of the service ID and API key that are generated for an `iam_credentials` secret. |
| `ttl`    |  **Required.** The time-to-live (TTL) or lease duration to assign to generated credentials. The value can be either an integer that specifies the number of seconds, or the string representation of a duration, such as `120m` or `24h`. |
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - IAM credentials" caption-side="top"}
{: #vault-create-secret-params-iam-creds}
{: tab-title="IAM credentials"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `payload` | **Required.** The secret data in JSON format to assign to the secret. The maximum file size is 512 KB. |
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - Key-value secrets" caption-side="top"}
{: #vault-create-secret-params-kv}
{: tab-title="Key-value secrets"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}


| Request parameters           | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `username`    | **Required.** The username to assign to the secret.|
| `password`    | The password to assign to the secret. |
| `expiration_date` | The expiration date that you want to assign to the secret. The date format follows [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339).|
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - User credentials" caption-side="top"}
{: #vault-create-secret-params-user-creds}
{: tab-title="User credentials"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `certificate` | **Required.** The certificate data to assign to an `imported_cert` secret. |
| `private_key` | The matching private key to assign to an `imported_cert` secret. |
| `intermediate` | The intermediate certificate data to assign to an `import_cert` secret.|
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - Imported certificates" caption-side="top"}
{: #vault-create-secret-params-imported-certs}
{: tab-title="Imported certificates"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                          |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret.       |
| `description` | An extended description of the secret.                                              |
| `certificate_template` | **Required.** The name of the certificate template.                        |
| `common_name` | The fully qualified domain name or host domain name for the certificate.            |
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
| `alt_names` | The Subject Alternative Names to define for the certificate, in a comma-delimited list.      |                        
| `ip_sans` | The IP Subject Alternative Names to define for the certificate, in a comma-delimited list.     |                        
| `uri_sans` | The URI Subject Alternative Names to define for the certificate, in a comma-delimited list.   |                        
| `other_sans` | The custom Object Identifier (OID) or UTF8-string Subject Alternative Names to define for the certificate.  \n  \n The alternative names must match the values that are specified in the `allowed_other_san`s field in the associated certificate template. The format is the same as OpenSSL: `<oid>:<type>:<value>` where the current valid type is `UTF8`.|
| `ttl` | The time-to-live (TTL) to assign to a private certificate.  \n  \n The value can be supplied as a string representation of a duration in hours, for example '12h'. The value can't exceed the `max_ttl` that is defined in the associated certificate template. |   
| `format` | The format of the returned data. Allowable values are: `pem`, `pem_bundle`. Default: `pem` |
| `auto_rotate` | Determines whether Secrets Manager rotates your certificate automatically. For private certificates, the certificate is rotated according to the time interval specified in the `interval` and `unit` fields.  |
| `interval` | Used together with the `unit` field to specify the rotation interval. The minimum interval is one day, and the maximum interval is 3 years (1095 days). Required in case `auto_rotate` is set to `true`.  |
| `unit` | The time unit of the rotation interval. Allowable values are: `day`, `month` |            
{: caption="Table 6. Create secret request parameters - Private certificates" caption-side="top"}
{: #vault-create-secret-params-private-certs}
{: tab-title="Private certificates"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | **Required.** The human-readable alias that you want to assign to the secret. |
| `description` | An extended description of the secret.                                      |
| `ca` | **Required.** The name of the certificate authority configuration. |
| `dns` |  **Required.** The name of the DNS provider configuration.|
| `common_name` | **Required.** The fully qualified domain name or host domain name for the certificate.|
| `alt_names[]` | The alternative names to define for the certificate. |
| `bundle_certs` | Determines whether your issued certificate is bundled with intermediate certificates.  \n  \n Set to `false` for the certificate file to contain only the issued certificate. Default: `true`. |
| `key_algorithm` | The identifier for the cryptographic algorithm to be used to generate the public key that is associated with the certificate.  \n  \n Allowable values: `RSA2048`, `RSA4096`, `EC256` ,`EC384` |
| `auto_rotate` | Determines whether Secrets Manager rotates your certificate automatically.  \n  \n If set to `true`, the service reorders your certificate 31 days before it expires. Default: `false`|
| `rotate_keys` | Determines whether Secrets Manager rotates the private key for your certificate automatically. If set to `true`, the service generates and stores a new private key for your rotated certificate. Default: `false` |
| `labels[]` | Labels that you can use to filter for secrets in your instance. Up to 30 labels can be added. |
{: caption="Table 6. Create secret request parameters - Public certificates" caption-side="top"}
{: #vault-create-secret-params-public-certs}
{: tab-title="Public certificates"}
{: tab-group="vault-create-secret-params"}
{: class="simple-tab-table"}

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

Create a key-value secret in the `default` secret group. [Learn more](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#overview-kv).

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets' \
-H 'Accept: application/json' \
-H 'X-Vault-Token: {Vault-Token}' \
-H 'Content-Type: application/json' \
-d '{
    "name": "test-kv-secret",
    "description": "Extended description for my secret.",
    "payload": {
        "key1": "value1"
    },
    "labels": [
        "dev",
        "us-south"
    ]
}'
```
{: codeblock}

Create a key-value secret in an existing secret group. 

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/groups/{group_id}' \
-H 'Accept: application/json/groups/{group_id}' \
-H 'X-Vault-Token: {Vault-Token}' \
-H 'Content-Type: application/json' \
-d '{
    "name": "test-kv-secret",
    "description": "Extended description for my secret.",
    "payload": {
        "key1": "value1"
    },
    "labels": [
        "dev",
        "us-south"
    ]
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

Import an SSL/TLS certificate and assign it to the `default` secret group.

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

Import an SSL/TLS certificate and assign it to an existing secret group.

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

Order a public SSL/TLS certificate and assign it to the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/secrets" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "test-public-certificate-in-group",
        "description": "Extended description for my secret.",
        "ca": "my-configured-certificate-authority",
        "dns": "my-configured-dns-provider",
        "common_name": "example.com",
        "alt_names": [
            "www.example.com"
        ],
        "labels": [
            "dev",
            "us-south"
        ],
        "bundle_certs": false, 
        "key_algorithm": "RSA2048",
        "rotation": {
            "auto_rotate": false,
            "rotate_keys": false
        }
    }'
```
{: codeblock}

Order a public SSL/TLS certificate and assign it to an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/secrets/groups/{group_id}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "test-public-certificate-in-group",
        "description": "Extended description for my secret.",
        "ca": "my-configured-certificate-authority",
        "dns": "my-configured-dns-provider",
        "common_name": "example.com",
        "alt_names": [
            "www.example.com"
        ],
        "labels": [
            "dev",
            "us-south"
        ],
        "bundle_certs": false, 
        "key_algorithm": "RSA2048",
        "rotation": {
            "auto_rotate": false,
            "rotate_keys": false
        }
    }'
```
{: codeblock}

Create a private SSL/TLS certificate and assign it to the `default` secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/secrets" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "test-private-certificate",
        "description": "Extended description for my secret.",
        "certificate_template": "my-configured-certificate-template",
        "common_name": "example.com",
        "alt_names": [
            "www.example.com"
        ],
        "labels": [
            "dev",
            "us-south"
        ],
        "rotation": {
            "auto_rotate": true,
            "interval": 90,
            "unit": day
        }
    }'
```
{: codeblock}

Create a private SSL/TLS certificate and assign it to an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/secrets/groups/{group_id}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "test-private-certificate",
        "description": "Extended description for my secret.",
        "certificate_template": "my-configured-certificate-template",
        "common_name": "example.com",
        "alt_names": [
            "www.example.com"
        ],
        "labels": [
            "dev",
            "us-south"
        ],
        "rotation": {
            "auto_rotate": true,
            "interval": 90,
            "unit": day
        }
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

A request to create key-value secrets in the `default` secret group returns the following response:

```json
{
    "request_id": "6e0000-60c0-d0ef-bc00-000c0a000b00",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-9ca407-f38d-000e-0b02-ed6b41",
        "creation_date": "2022-01-25T19:22:59Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/0000000be376647f5f961f5:50004-5f59-4164-8bfc-5000cf66:secret:43f000f-4085-000c-c028-6ff00004dbd",
        "description": "Extended description for my secret.",
        "downloaded": false,
        "id": "40000df-4000-300c-c01028-6ff20000dbd",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2022-01-25T19:22:59Z",
        "name": "test-kv-secret",
        "secret_data": {
            "payload": {
                "key1": "value1"
            }
        },
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "iam-ServiceId-9ca407-f38d-000e-0b02-ed6b41",
                "creation_date": "2022-01-25T19:22:59Z",
                "downloaded": false,
                "id": "40000df-4000-300c-c01028-6ff20000dbd",
                "payload_available": true
            }
        ],
        "versions_total": 1
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to create key-value secrets in an existing secret group returns the following response:

```json
{
    "request_id": "a0766ef6-5bfe-d92d-4894-6d3f40126b25",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-9ca24407-f38d-479e-8b02-ed6b4e1b0d31",
        "creation_date": "2022-01-27T17:59:20Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4b85ea6bbea644a6be376647f5f961f5:5f1a3554-5f59-4164-8bfc-5eef0a20cf66:secret:21764466-5a9d-a9df-fc71-5b8ee4ecbb99",
        "description": "Extended description for my secret.",
        "downloaded": false,
        "id": "21764466-5a9d-a9df-fc71-5b8ee4ecbb99",
        "labels": [
            "dev",
            "us-south"
        ],
        "last_update_date": "2022-01-27T17:59:20Z",
        "name": "test-kv-secret6",
        "secret_data": {
            "payload": {
                "key6": "value6"
            }
        },
        "secret_group_id": "aded5ffd-da17-c923-eb21-600569c5d1c2",
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "iam-ServiceId-9ca24407-f38d-479e-8b02-ed6b4e1b0d31",
                "creation_date": "2022-01-27T17:59:20Z",
                "downloaded": false,
                "id": "96d5b7dd-d8fb-5afc-9c05-6cfdaff8af9e",
                "payload_available": true
            }
        ],
        "versions_total": 1
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

Get key-value secrets in the `default` secret group. [Learn more](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#overview-kv).

```sh
curl -L -X GET 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/{secret_id}' \
    -H 'Accept: application/json'\
    -H 'X-Vault-Token: {Vault-Token}' 
```
{: codeblock}

Get key-value secrets in an existing secret group.

```sh
curl -X GET 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/groups/{group_id}/{secret_id}' \
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
    "api_key": "U40hERZ0h-0C0cnka2bEuL2y...(redacted)",
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
    "api_key": "CFQY6wWPI3C3wKx6XLC9p0c3e...(redacted)",
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

A request to retrieve a key-value secret in the `default` secret group returns the following response:

```json
{
    "request_id": "1e0000-7100-cb00b-d00a-b350000f5a",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-9c00000-00d-000e-8000-ed6b40000",
        "creation_date": "2022-01-25T19:22:04Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4b85000004a6be3700000f5:5f1000-5f00-4000-8bfc-5e0000f66:secret:0000ea8e-7d00-69ce-c000a-0a00000b3ee",
        "description": "Extended description for my secret.",
        "downloaded": true,
        "id": "00002ea8e-7lk90-00ce-c200a-00004b3ee",
        "labels": [],
        "last_update_date": "2022-01-25T19:22:04Z",
        "name": "test-kv-secret",
        "secret_data": {
            "payload": {
                "key1": "value1"
            }
        },
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "iam-ServiceId-000000-f000d-479e-8b02-ed600000",
                "creation_date": "2022-01-25T19:22:04Z",
                "downloaded": true,
                "id": "bf00007-800dc-0006-14d9-a7c720000bh",
                "payload_available": true
            }
        ],
        "versions_total": 1
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to retrieve a key-value secret in an existing secret group returns the following response:

```json
{
    "request_id": "a0000c-e00-000ef-d000e8-a68e60000",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "Id-000000",
        "creation_date": "2022-01-26T20:11:29Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4b0000a6bbe:5f1a3554-5f59-4164-8bfc-5e0000000cf66:secret:e006e8bc-f497-dc93-4102-9d0000001",
        "description": "Extended description for my secret.",
        "downloaded": true,
        "id": "e00000c-f0000-d0003-00002-9d9cf2000001",
        "labels": [],
        "last_update_date": "2022-01-26T20:11:29Z",
        "name": "test-kv-secret-from-group",
        "secret_data": {
            "payload": {
                "key5": "value5"
            }
        },
        "secret_group_id": "0000ffd-da17-c0000-eb0000-600000002",
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "Id-0000000",
                "creation_date": "2022-01-26T20:11:29Z",
                "downloaded": true,
                "id": "5c000000-000c3-00003-de0000-c0d200000",
                "payload_available": true
            }
        ],
        "versions_total": 1
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

Retrieve the metadata of a secret, such as its name, description. To retrieve the actual value of a secret, use [Get a secret](#vault-get-secret).

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

Get metadata for a `kv` secret in the `default` secret group. [Learn more](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv).

```sh
curl -X GET 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/{secret_id}/metadata' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Get metadata for a `kv` secret in an existing secret group. 

```sh
curl -X GET 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/groups/{group_id}/{secret_id}/metadata' \
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

A request to retrieve the metadata of a `kv` secret in the `default` secret group returns the following response:

```json
{
    "request_id": "0a0000e-0a0f-edfh-000a-ec2000ab00",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-9ca00000-f00d-000e-8b02-ed6b000pl",
        "creation_date": "2022-01-25T19:22:04Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4000000bbea00000000647f000001f5:5f000004-5f00-40000-8bfc-5mnh0a200000:secret:00000ea8e-7d00-00ce-c00poa-0a00000f0000e",
        "description": "Extended description for my secret.",
        "downloaded": true,
        "id": "0a0000e-0a0f-edfh-000a-ec2000ab00",
        "labels": [],
        "last_update_date": "2022-01-25T19:22:04Z",
        "name": "test-kv-secret",
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions_total": 1
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to retrieve the metadata of a `kv` secret in an existing secret group returns the following response:

```json
{
    "request_id": "0a0000e-0a0f-edfh-000a-ec2000ab00",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "id-0000000YC6X",
        "creation_date": "2022-01-26T20:11:29Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4000000bbea00000000647f000001f5:5f000004-5f00-40000-8bfc-5mnh0a200000:secret:00000ea8e-7d00-00ce-c00poa-0a00000f0000e",
        "description": "Test secret in test secret group.",
        "downloaded": true,
        "id": "0a0000e-0a0f-edfh-000a-ec2000ab00",
        "labels": [],
        "last_update_date": "2022-01-26T20:11:29Z",
        "name": "test-kv-secret-from-group",
        "secret_group_id": "aded0a0000e-0a0f-edfh-000a-ec2000ab00",
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions_total": 1
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Update secret metadata
{: #vault-update-secret-metadata}

Update the metadata of a secret, such as its name, description, or expiration date. To rotate the actual value of a secret, use [Rotate a secret](#vault-rotate-secret).

| Request parameters            | Description                                                                         |
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

A request to update the metadata of an `arbitrary` secret in the `default` secret group returns the following response:

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

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `payload`     | The new secret data to assign to an `arbitrary` or a `kv` secret. |
| `password`    | The new password to assign to a `username_password` secret. |
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

Rotate a `kv` secret in the `default` secret group. [Learn more](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv&interface=api#overview-kv).

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/{secret_id}/rotate' \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "payload": {
            "key7":"value7"
            }
    }'
```
{: codeblock}

Rotate a `kv` secret in an existing secret group.

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/kv/secrets/groups/{group_id}/{secret_id}/rotate' \
    -H 'Accept: application/json'
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{
        "payload": {
            "key7":"value7"
            }
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

A request to rotate a `kv` secret in the `default` secret group returns the following response:

```json
{
    "request_id": "e00000b-0000-0ad1-beb0-00000d0000",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "iam-ServiceId-9ca2000007-f0000d-400000e-8b02-ed6b000000",
        "creation_date": "2022-01-25T19:22:04Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/00000a6bbea644a6be000000001f5:5f1a000000-5f000-4000-8bfc-5eef00000:secret:00000ea8e-7d00-00ce-c00a-0a0000f000ee",
        "description": "Extended description for my secret.",
        "downloaded": false,
        "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
        "labels": [],
        "last_update_date": "2022-01-27T21:05:25Z",
        "name": "test-kv-secret",
        "secret_data": {
            "payload": {
                "key7": "value7"
            }
        },
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "iam-ServiceId-9ca2000007-f0000d-400000e-8b02-ed6b000000",
                "creation_date": "2022-01-25T19:22:04Z",
                "downloaded": true,
                "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "payload_available": false
            },
            {
                "created_by": "iam-ServiceId-9ca2000007-f0000d-400000e-8b02-ed6b000000",
                "creation_date": "2022-01-27T21:05:25Z",
                "downloaded": false,
                "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "payload_available": true
            }
        ],
        "versions_total": 2
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to rotate a `kv` secret in an existing secret group returns the following response:

```json
{
    "request_id": "e00000b-0000-0ad1-beb0-00000d0000",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "created_by": "IBMid-662001YC6X",
        "creation_date": "2022-01-26T20:11:29Z",
        "crn": "crn:v1:bluemix:public:secrets-manager:eu-gb:a/4b85ea6bbea644a6be376647f5f961f5:5f1a3554-5f59-4164-8bfc-5eef0a20cf66:secret:e006e8bc-f497-dc93-4102-9d9cf2051a41",
        "description": "Test secret in test secret group.",
        "downloaded": false,
        "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
        "labels": [],
        "last_update_date": "2022-01-27T21:00:27Z",
        "name": "test-kv-secret-from-group",
        "secret_data": {
            "payload": {
                "key7": "value7"
            }
        },
        "secret_group_id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
        "secret_type": "kv",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "created_by": "iam-ServiceId-00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "creation_date": "2022-01-26T20:11:29Z",
                "downloaded": true,
                "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "payload_available": false
            },
            {
                "created_by": "iam-ServiceId-00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "creation_date": "2022-01-27T21:00:03Z",
                "downloaded": false,
                "id": "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "payload_available": false
            },
            {
                "created_by": "iam-ServiceId-00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "creation_date": "2022-01-27T21:00:27Z",
                "downloaded": false,
                "id":  "00000ea00-7d0000-0000ce-c0002a-0a0000f4b3ee",
                "payload_available": true
            }
        ],
        "versions_total": 3
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

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



## Locks
{: #vault-api-secret-locks}

### List secret locks
{: #vault-list-locks}

List the locks that are associated with a specified secret.

| Query parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `limit`     | The number of locks to retrieve. Default is 25. To retrieve a different set of items, use `limit` with `offset` to page through your available resources. |
| `offset`    | The number of locks to skip. Default is 0. By specifying offset, you retrieve a subset of locks that starts with the offset value. Use offset with limit to page through your available secrets locks. |
| `search` | Filter locks that contain the specified string in their name. |
{: caption="Table 9. Lock secret request parameters" caption-side="top"}

#### Example request
{: #vault-list-locks-request}

List locks for an arbitrary secret in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/locks/{secret_id}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

List locks for a user credentials secret in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Page through available locks by using `limit` and `offset`.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}?limit={limit}&offset={offset}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Filter for locks that contain `book` in their names.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}?search=book" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-list-locks-response}

```json
{
    "request_id": "ba51140d-31a8-0a51-dd5b-1ca59838e881",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "locks": [
            {
                "attributes": {
                    "key": "value"
                },
                "created_by": "iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8",
                "creation_date": "2022-06-30T21:41:36.616174Z",
                "description": "Test lock for secret in the default secret group.",
                "last_update_date": "2022-06-30T21:41:36.616174Z",
                "name": "lock-for-app-2",
                "secret_group_id": "default",
                "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
                "secret_version_alias": "current",
                "secret_version_id": "f2b68dbb-c291-87df-6026-7611c324c823"
            },
            {
                "attributes": {
                    "key": "value"
                },
                "created_by": "iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8",
                "creation_date": "2022-06-30T20:56:33.138337Z",
                "description": "Test lock for secret in the default secret group.",
                "last_update_date": "2022-06-30T21:14:14.903163Z",
                "name": "lock-for-app-1",
                "secret_group_id": "default",
                "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
                "secret_version_alias": "previous",
                "secret_version_id": "09d9718b-b411-4111-a8f4-b1397d22d11b"
            }
        ],
        "locks_total": 2
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: codeblock}

### Lock a secret
{: #vault-create-locks}

[Create one or more locks](/docs/secrets-manager?topic=secrets-manager-secret-locks) on the current version of a secret.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it already exists. Additionally, you can use this method to clear any matching locks on a secret by using an optional lock mode.

- `lock_exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `lock_exclusive_delete`: Same as `lock_exclusive`, but also permanently deletes the data of the previous secret version if no locks are found.

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`     | A human-readable name to assign to your secret lock. Names are unique per secret version.  /n **Note:** Creating a lock with an existing name replaces the lock and overrides its attributes. |
| `description`    | An extended description of your secret lock. |
| `attributes` | Optional information to associate with a lock, such as resources CRNs to be used by automation. |
{: caption="Table 9. Lock secret request parameters" caption-side="top"}

#### Example request
{: #vault-create-locks-request}

Create a lock on a secret in the default secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/lock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in the default secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

Create two locks on the current version of a secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/groups/{group_id}/{secret_id}/lock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in a custom secret group.",
                "attributes": {
                    "key": "value"
                }
            },
            {
                "name": "lock-for-app-2",
                "description": "Test lock for secret in a custom secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

Lock a secret version exclusively.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/lock_exclusive" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in the default secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

Lock a secret version exclusively and delete previous version data.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/lock_exclusive_delete" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in the default secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

#### Example response
{: #vault-create-locks-response}

A request to lock the current version of a secret that is in the default secret group returns the following response:
```json
{
    "request_id": "cad3f223-ec90-1e8e-9408-7fc3c9c50b86",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "default",
        "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
        "versions": [
            {
                "alias": "current",
                "id": "f2b68dbb-c291-87df-6026-7611c324c823",
                "locks": [
                    "lock-for-app-1"
                ],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to lock the current version of a secret that is a custom secret group returns the following response:
```json
{
    "request_id": "a717fba0-275d-36d2-49e6-ae54fc820ca4",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "d2e98a96-18ed-f13c-8dee-db955fb94122",
        "secret_id": "c86946e6-b392-2613-159d-aff5a3f095b3",
        "versions": [
            {
                "alias": "current",
                "id": "ad6aa6d9-b43c-4bc3-597d-15c376622e64",
                "locks": [
                    "lock-for-app-1"
                ],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Unlock a secret
{: #vault-delete-locks}

Delete one or more locks that are associated with the current version of a secret.

A successful request deletes the locks that you specify. To remove all locks, you can pass `{"locks": ["*"]}` in the request body. Otherwise, specify the names of the locks that you want to delete. For example, `{"locks": ["lock1", "lock2"]}`.

A secret is considered unlocked and able to be revoked or deleted only after all of its locks are removed. To understand whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.
{: note}

#### Example request
{: #vault-delete-locks-request}

Remove all locks that are associated with a secret.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/unlock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": ["*"]
    }'
```
{: codeblock}

Remove two locks from a secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/groups/{group_id}/{secret_id}/unlock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": ["lock-name-1", "lock-name-2"]
    }'
```
{: codeblock}

#### Example response
{: #vault-delete-locks-response}

A request to remove all locks returns the following response:

```json
{
    "request_id": "4708ebbf-eab0-e68a-9e72-d1c67a209fdc",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "default",
        "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
        "versions": [
            {
                "alias": "current",
                "id": "f2b68dbb-c291-87df-6026-7611c324c823",
                "locks": [],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to remove only specific locks lists the remaining locks in the response:

```json
{
    "request_id": "4d954026-68b3-6506-dc1d-5e77574fd2f0",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "default",
        "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
        "versions": [
            {
                "alias": "current",
                "id": "f2b68dbb-c291-87df-6026-7611c324c823",
                "locks": [
                    "lock-for-app-1"
                ],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### List secret version locks
{: #vault-list-version-locks}

List the locks that are associated with a specified secret version.

Use `{version_id}` in the URL path to specify the version. The aliases `current` or `previous` are also allowed. 

| Query parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `limit`     | The number of locks to retrieve. Default is 25. To retrieve a different set of items, use `limit` with `offset` to page through your available resources. |
| `offset`    | The number of locks to skip. Default is 0. By specifying offset, you retrieve a subset of locks that starts with the offset value. Use offset with limit to page through your available secrets locks. |
| `search` | Filter locks that contain the specified string in their name. |
{: caption="Table 9. Lock secret request parameters" caption-side="top"}

#### Example request
{: #vault-list-version-locks-request}

List locks for a specific version of an arbitrary secret in the `default` secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/arbitrary/locks/{secret_id}/versions/{version_id}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

List locks for the current version of a user credentials secret in an existing secret group.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}/versions/current" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Page through available locks by using `limit` and `offset`.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}/versions/current?limit={limit}&offset={offset}" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

Filter for locks that contain `book` in their names.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/username_password/locks/groups/{group_id}/{secret_id}/versions/current?search=book" \
    -H 'Accept: application/json' \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-list-verison-locks-response}

A request to get the lock details on the current version of a secret in the default secret group returns the following response:
```json
{
    "request_id": "ba51140d-31a8-0a51-dd5b-1ca59838e881",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "locks": [
            {
                "attributes": {
                    "key": "value"
                },
                "created_by": "iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8",
                "creation_date": "2022-06-30T21:41:36.616174Z",
                "description": "Test lock for secret in the default secret group.",
                "last_update_date": "2022-06-30T21:41:36.616174Z",
                "name": "lock-for-app-2",
                "secret_group_id": "default",
                "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
                "secret_version_alias": "current",
                "secret_version_id": "f2b68dbb-c291-87df-6026-7611c324c823"
            },
            {
                "attributes": {
                    "key": "value"
                },
                "created_by": "iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8",
                "creation_date": "2022-06-30T20:56:33.138337Z",
                "description": "Test lock for secret in the default secret group.",
                "last_update_date": "2022-06-30T21:14:14.903163Z",
                "name": "lock-for-app-1",
                "secret_group_id": "default",
                "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
                "secret_version_alias": "current",
                "secret_version_id": "f2b68dbb-c291-87df-6026-7611c324c823"
            }
        ],
        "locks_total": 2
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Lock a secret version
{: #vault-create-version-locks}

[Create one or more locks](/docs/secrets-manager?topic=secrets-manager-secret-locks) on a specified version of a secret. To specify a version, use the `{version_id}` path parameter to provide the unique ID of the current or previous version of your secret. The aliases `current` or `previous` are also allowed.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it already exists. Additionally, you can use this method to clear any matching locks on a secret by using an optional lock mode.

- `lock_exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `lock_exclusive_delete`: Same as `lock_exclusive`, but also permanently deletes the data of the previous secret version if no locks are found.

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`     | A human-readable name to assign to your secret lock. Names are unique per secret version.  /n **Note:** Creating a lock with an existing name replaces the lock and overrides its attributes. |
| `description`    | An extended description of your secret lock. |
| `attributes` | Optional information to associate with a lock, such as resources CRNs to be used by automation. |
{: caption="Table 9. Lock secret request parameters" caption-side="top"}

#### Example request
{: #vault-create-version-locks-request}

Create a lock on the specified version of a secret in the default secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/versions/{version_id}/lock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in the default secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

Replace `{version_id}` in the URL path with the `current` alias to create a lock on the current secret version. The aliases `current` or `previous` are allowed.
{: tip}

Create two locks on the current version of a secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/groups/{group_id}/{secret_id}/versions/current/lock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in a custom secret group.",
                "attributes": {
                    "key": "value"
                }
            },
            {
                "name": "lock-for-app-2",
                "description": "Test lock for secret in a custom secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

Create a lock on the previous version of a secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/groups/{group_id}/{secret_id}/versions/previous/lock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": [
            {
                "name": "lock-for-app-1",
                "description": "Test lock for secret in a custom secret group.",
                "attributes": {
                    "key": "value"
                }
            }
        ]
    }'
```
{: codeblock}

#### Example response
{: #vault-create-version-locks-response}

A request to lock the previous version of a secret in a custom secret group 
```json
{
    "request_id": "97a3d1fb-c137-9c1c-16fb-7aebf05a0eae",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "d2e98a96-18ed-f13c-8dee-db955fb94122",
        "secret_id": "c86946e6-b392-2613-159d-aff5a3f095b3",
        "versions": [
            {
                "alias": "current",
                "id": "3993c39b-3ef5-f6f3-5e20-f6f9c6f8d053",
                "locks": [],
                "payload_available": true
            },
            {
                "alias": "previous",
                "id": "ad6aa6d9-b43c-4bc3-597d-15c376622e64",
                "locks": [
                    "lock-for-app-1"
                ],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: codeblock}

### Unlock a secret version
{: #vault-delete-version-locks}

Delete one or more locks that are associated with the specified secret version.

A successful request deletes the locks that you specify. To remove all locks, you can pass `{"locks": ["*"]}` in in the request body. Otherwise, specify the names of the locks that you want to delete. For example, `{"locks": ["lock-1", "lock-2"]}`.

A secret is considered unlocked and able to be revoked or deleted only after all of its locks are removed. To understand whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.
{: note}

#### Example request
{: #vault-delete-version-locks-request}

Remove all locks on a secret version.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/{secret_id}/versions/{version_id}/unlock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": ["*"]
    }'
```
{: codeblock}

Replace `{version_id}` in the URL path with the `current` alias to remove locks from the current secret version. The aliases `current` or `previous` are allowed.
{: tip}

Remove two locks on the current version of a secret in an existing secret group.

```sh
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/{secret_type}/locks/groups/{group_id}/{secret_id}/versions/current/unlock" \
    -H 'X-Vault-Token: {Vault-Token}'
    -H 'Content-Type: application/json' \
    -D '{
        "locks": ["lock-name-1", "lock-name-2"]
    }'
```
{: codeblock}

#### Example response
{: #vault-delete-version-locks-response}

A request to remove all locks returns the following response:

```json
{
    "request_id": "4708ebbf-eab0-e68a-9e72-d1c67a209fdc",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "default",
        "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
        "versions": [
            {
                "alias": "current",
                "id": "f2b68dbb-c291-87df-6026-7611c324c823",
                "locks": [],
                "payload_available": true
            }
        ]
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to remove only specific locks lists the remaining locks in the response:

```json
{
    "request_id": "4d954026-68b3-6506-dc1d-5e77574fd2f0",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "secret_group_id": "default",
        "secret_id": "184408d6-8264-5ff3-c308-6922ed04ad88",
        "versions": [
            {
                "alias": "current",
                "id": "f2b68dbb-c291-87df-6026-7611c324c823",
                "locks": [
                    "lock-for-app-1"
                ],
                "payload_available": true
            }
        ]
    },
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

Creates or updates an [automatic rotation policy](/docs/secrets-manager?topic=secrets-manager-automatic-rotation) for a secret. Supported secret types include: `username_password`

| Request parameters            | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `interval`     | The length of the secret rotation time interval. |
| `unit`    | The units for the secret rotation time interval. Allowable values are: day, month|
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
{: screen}

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
{: screen}

## Config
{: #vault-api-secrets-engines-config}

### Set the configuration of a secret type
{: #vault-configure-secret-type}

Configures a secrets engine that serves as the backend for a specific type of secret. You can set the configuration for the following secret types: `iam_credentials`

| Request parameters         | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `api_key`     | An {{site.data.keyword.cloud_notm}} API key that can create and manage service IDs. The API key must be assigned the Editor platform role on the Access Groups Service and the Operator platform role on the IAM Identity Service. |
{: caption="Table 10. IAM secrets engine request parameters" caption-side="top"}
{: #iam-secrets-engine-request-params}
{: tab-title="IAM credentials"}
{: tab-group="vault-configure-secret-type-params"}
{: class="simple-tab-table"}


#### Example request
{: #vault-configure-secret-type-request}

Configure the `iam_credentials` secrets engine.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/config/root" \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{ 
        "api_key": "<API_KEY>" 
    }'
```
{: codeblock}

#### Example response
{: #vault-configure-secret-type-response}

A request to configure the `iam_credentials` secrets engine returns the following response:

```json
{
    "request_id": "f7ac2068-6b07-7602-76af-093e354a444a",
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

### Get the configuration of a secret type
{: #vault-get-secrets-engines-config}

Retrieves the configuration of a secrets engine.

#### Example request
{: #vault-get-secrets-engines-config-request}

Get the configuration of `iam_credentials` secrets engine.

```sh
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/iam_credentials/config/root" \
    -H 'X-Vault-Token: {Vault-Token}'
```
{: codeblock}

#### Example response
{: #vault-get-secrets-engines-config-response}

A request to get the configuration of the `iam_credentials` secrets engine returns the following response:

```json
{
    "request_id": "12f0a38d-93a5-6a9a-1997-79928f15c5ea",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "api_key_hash": "caf9eabec3c6dcc7f91cb6ea295eb97c8e34e70b0cf5942d6351d8746d9cc2da"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Add a configuration
{: #vault-add-secrets-engines-config}

Adds a configuration element to a secrets engine. This method is used for more complex engines, for example the `public_cert` and `private_cert` engines.

You can add multiple configurations for your instance:

- Up to 10 public certificate authority configurations
- Up to 10 DNS provider configurations
- Up to 10 private root certificate authority configurations
- Up to 10 private intermediate certificate authority configurations
- Up to 10 certificate templates

| Request parameters           | Description                                                          |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | A human-readable name to assign to your certificate authority configuration.        |
| `type`        | The environment type, for example the Let's Encrypt staging or production environment that corresponds with the URL that you want to target to order public certificates. Allowable values are: `letsencrypt-stage`, `letsencrypt` | 
| `private_key` | The private key that is associated with your registered ACME account.               |
{: caption="Table 11. Public certificates engine request parameters" caption-side="top"}
{: #public-cert-secrets-engine-ca-request-params}
{: tab-title="Public CAs"}
{: tab-group="vault-add-config-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                          |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`     | A human-readable name to assign to your DNS provider configuration.                    |
| `type` | The name of the DNS provider that you want to use. Allowable values are: `cis`             |
| `cis_crn` | The CRN of the Cloud Internet Services (CIS) instance that you want to use.             |
| `cis_apikey` | An API key that has access to both your CIS instance and {{site.data.keyword.secrets-manager_short}} instance. Alternatively, you can also create an authorization between both services by using IAM. |
{: caption="Table 11. Public certificates engine request parameters" caption-side="top"}
{: #public-cert-secrets-engine-dns-request-params}
{: tab-title="DNS providers"}
{: tab-group="vault-add-config-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                          |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | A human-readable name to assign to your certificate authority configuration.        |
| `type`        | The type of certificate authority that you want to create. Allowable values are: `root_certificate_authority`, `intermediate_certificate_authority`. | 
| `[params..]`  | For a complete list of parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}. |
{: caption="Table 11. Private certificates engine request parameters" caption-side="top"}
{: #private-cert-secrets-engine-ca-request-params}
{: tab-title="Private CAs"}
{: tab-group="vault-add-config-params"}
{: class="simple-tab-table"}

| Request parameters           | Description                                                          |
| ------------- | ----------------------------------------------------------------------------------- |
| `name`        | A human-readable name to assign to your certificate template.                       |
| `type`        | The type of configuration that you want to add. For certificate templates, use `certificate_templates`. |
| `[params..]`  | For a complete list of parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}. |
{: caption="Table 11. Private certificates engine request parameters" caption-side="top"}
{: #private-cert-secrets-engine-template-request-params}
{: tab-title="Certificate templates"}
{: tab-group="vault-add-config-params"}
{: class="simple-tab-table"}

#### Example requests
{: #vault-add-secrets-engines-config-request}

Add a public certificate authority configuration.

```sh
curl -X PUT "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/config/certificate_authorities" \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d '{ 
        "name": "test-certificate-authority",
        "type": "letsencrypt-stage",
        "config": {
          "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)"
        }
    }'
```
{: codeblock}

Add a private root certificate authority configuration.

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/config/root_certificate_authorities' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -H '{
        "name": "my-configured-root-ca",
        "type": "root_certificate_authority",
        "config": {
            "max_ttl": "43830h",
            "common_name": "example.com",
            "crl_disable": false,
            "crl_distribution_points_encoded": true,
            "issuing_certificates_urls_encoded": true
        }
    }'
```
{: codeblock}

Add an intermediate certificate authority configuration.

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/config/intermediate_certificate_authorities' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -H '{
        "name": "my-configured-intermediate-ca",
        "type": "intermediate_certificate_authority",
        "config": {
            "max_ttl": "26300h",
            "common_name": "example.com",
            "signing_method": "internal|external",
            "issuer": "my-configured-root-ca",
            "crl_expiry": "72h",
            "crl_disable": false,
            "crl_distribution_points_encoded": true,
            "issuing_certificates_urls_encoded": true
        }
    }
```
{: codeblock}

Add a certificate template.

```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/config/certificate_templates' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -H '{
        "name": "my-configured-certificate-template",
        "type": "certificate_template",
        "config": {
            "certificate_authority": "my-configured-intermediate-ca",
            "max_ttl": "8760h",
            "allow_any_name": true,
            "enforce_hostnames": false,
            "allowed_uri_sans": [
            "https://www.example.com/test"
            ]
        }
    }'
```
{: codeblock}

#### Example responses
{: #vault-add-secrets-engines-config-response}

A request to add a public certificate authority configuration returns the following response:

```json
{
    "request_id": "af1a900d-3cec-7f6d-8878-fa43d1587d90",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "config": {
            "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)"
        },
        "name": "test-certificate-authority",
        "type": "letsencrypt-stage"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

A request to add a private certificate authority configuration returns the following response:

```json
{
    "request_id": "0b221b39-1cd8-fa92-62e5-361c5e1b5d92",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "config": {
            "common_name": "example.com",
            "country": [],
            "crl_disable": false,
            "crl_distribution_points_encoded": true,
            "crl_expiry": 259200,
            "data": {
                "certificate": "-----BEGIN CERTIFICATE-----\nMIIGZjCCBU6gAwIBAgIUFsqE2...(redacted",
                "expiration": 1808862713,
                "issuing_ca": "-----BEGIN CERTIFICATE-----\nMIIGZjCCBU6gAwIBAgIUFsqE2...(redacted)",
                "serial_number": "16:ca:84:d8:4f:e5:b0:6c:5c:06:db:51:52:58:c1:3e:0b:96:ce:4f"
            },
            "exclude_cn_from_sans": false,
            "expiration_date": "2027-04-27T21:51:53Z",
            "format": "pem",
            "issuing_certificates_urls_encoded": true,
            "key_bits": 2048,
            "key_type": "rsa",
            "locality": [],
            "max_path_length": -1,
            "max_ttl": 157788000,
            "organization": [],
            "other_sans": [],
            "ou": [],
            "permitted_dns_domains": [],
            "postal_code": [],
            "private_key_format": "der",
            "province": [],
            "status": "configured",
            "street_address": [],
            "ttl": 157788000
        },
        "name": "my-configured-root-ca",
        "type": "root_certificate_authority"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Update a configuration
{: #vault-update-secrets-engines-config}

Updates the configuration of a secrets engine that serves as the backend for a specific type of secret. You can update the configuration for the following secret types: `iam_credentials`, `private_cert`, `public_cert`

#### Example requests
{: #vault-update-secrets-engines-config-request}

Update a DNS provider configuration for the `public_cert` secrets engine.

```sh
curl -X PUT 'https://https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/config/dns_providers' \
    -H 'X-Vault-Token: {Vault-Token}' \
    -H 'Content-Type: application/json' \
    -d'{
        "name": "my-cis-instance",
        "type": "cis",
        "config": {
          "cis_crn": "crn:v1:bluemix:public:internet-svcs:global:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0::",
          "cis_apikey": "<API_KEY>"
        }
    }'
```
{: codeblock}

#### Example response
{: #vault-update-secrets-engines-config-response}

A request to add a DNS provider configuration for the `public_cert` secrets engine returns the following response:

```json
{
    "request_id": "3c891ae8-18d3-f38e-5b98-dc1db2874f16",
    "lease_id": "",
    "renewable": false,
    "lease_duration": 0,
    "data": {
        "config": {
            "cis_apikey": "mGjiCelas...(redacted)",
            "cis_crn": "crn:v1:bluemix:public:internet-svcs:global:a/a5ebf2570dcaedf18d7ed78e216c263a:0f4c764e-dc3d-44d1-bd60-a2f7cd91e0c0::"
        },
        "name": "my-cis-instance",
        "type": "cis"
    },
    "wrap_info": null,
    "warnings": null,
    "auth": null
}
```
{: screen}

### Delete a configuration
{: #vault-delete-secrets-engines-config}

Removes a configuration for a secrets engine that serves as the backend for a specific type of secret. You can delete configurations for the following secret types: `public_cert`, `private_cert`


#### Example requests
{: #vault-delete-secrets-engines-config-request}

Delete a public certificate authority configuration.

```sh
curl -X DELETE 'https://https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/config/certificate_authorities/my-lets-encrypt' \
    -H 'X-Vault-Token: {Vault-Token}' \
```
{: codeblock}

Delete the DNS provider configuration.

```sh
curl -X DELETE 'https://https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/public_cert/config/dns_providers/my-cis-instance' \
    -H 'X-Vault-Token: {Vault-Token}' \
```
{: codeblock}

Delete a private certificate authority configuration.

```sh
curl -X DELETE 'https://https://{instance_id}.{region}.secrets-manager.appdomain.cloud/v1/ibmcloud/private_cert/config/root_certificate_authorities/my-root-ca' \
    -H 'X-Vault-Token: {Vault-Token}' \
```
{: codeblock}


#### Example response
{: #vault-delete-secrets-engines-config-response}

A successful request returns an `HTTP 204 No Content` response.


