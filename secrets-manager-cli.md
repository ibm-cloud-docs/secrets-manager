---

copyright:
  years: 2022
lastupdated: "2022-09-13"

subcollection: secrets-manager

keywords: Secrets Manager CLI, Secrets Manager command line, Secrets Manager terminal, Secrets Manager shell

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


# {{site.data.keyword.secrets-manager_short}} CLI
{: #secrets-manager-cli}

You can use the {{site.data.keyword.secrets-manager_full}} command-line interface (CLI) to manage secrets in your {{site.data.keyword.secrets-manager_short}} instance.
{: shortdesc}

Current version: **`0.1.22`**

## Prerequisites
{: #secrets-manager-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install the {{site.data.keyword.secrets-manager_short}} CLI by running the following command:

    ```sh
    ibmcloud plugin install secrets-manager
    ```
    {: pre}

    You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
    {: tip}

* Export an environment variable with your {{site.data.keyword.secrets-manager_short}} service endpoint URL.

    To target an instance for operations, export the following variable.
    ```sh
    export SECRETS_MANAGER_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
    ```
    {: pre}

    Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

    Need to override the URL? You can also specify the endpoint at the command-level by using the `--service-url INSTANCE` flag. For example, `ibmcloud secrets-manager secrets --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud`.
    {: tip}



## Secret groups
{: #secrets-manager-secret-groups-cli}

Control who on your team has access to secrets by creating and managing groups.

### `ibmcloud secrets-manager secret-group-create`
{: #secrets-manager-cli-secret-group-create-command}

Create a secret group that you can use to organize secrets and control who on your team has access to them.

A successful request returns the ID value of the secret group, along with other metadata. To learn more about secret groups, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-groups).

```sh
ibmcloud secrets-manager secret-group-create --resources RESOURCES
```


#### Command options
{: #secrets-manager-secret-group-create-cli-options}

`--resources` ([`SecretGroupResource[]`](#cli-secret-group-resource-example-schema))
:   A collection of resources. Required.

#### Examples
{: #secrets-manager-cli-secret-group-create-command-examples}

```sh
ibmcloud secrets-manager secret-group-create \
    --resources='[{"name": "my-secret-group", "description": "Extended description for this group."}]'
```
{: pre}

### `ibmcloud secrets-manager secret-groups`
{: #secrets-manager-cli-secret-groups-command}

List the secret groups that are available in your Secrets Manager instance.

```sh
ibmcloud secrets-manager secret-groups 
```


#### Examples
{: #secrets-manager-cli-secret-groups-command-examples}

```sh
ibmcloud secrets-manager secret-groups
```
{: pre}

### `ibmcloud secrets-manager secret-group`
{: #secrets-manager-cli-secret-group-command}

Get the metadata of an existing secret group by specifying the ID of the group.

```sh
ibmcloud secrets-manager secret-group --id ID 
```


#### Command options
{: #secrets-manager-secret-group-cli-options}

`--id` (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Examples
{: #secrets-manager-cli-secret-group-command-examples}

```sh
ibmcloud secrets-manager secret-group \
    --id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-group-metadata-update`
{: #secrets-manager-cli-secret-group-metadata-update-command}

Update the metadata of an existing secret group, such as its name or description.

```sh
ibmcloud secrets-manager secret-group-metadata-update --id ID --metadata METADATA --resources RESOURCES  
```


#### Command options
{: #secrets-manager-secret-group-metadata-update-cli-options}

`--id` (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--resources` ([`SecretGroupMetadataUpdatable[]`](#cli-secret-group-metadata-updatable-example-schema))
:   A collection of resources. Required.

#### Examples
{: #secrets-manager-cli-secret-group-metadata-update-command-examples}

```sh
ibmcloud secrets-manager secret-group-metadata-update \
    --id=exampleString \
    --resources='[{"name": "updated-secret-group-name", "description": "Updated description for this group."}]'
```
{: pre}

### `ibmcloud secrets-manager secret-group-delete`
{: #secrets-manager-cli-secret-group-delete-command}

Delete a secret group by specifying the ID of the secret group.

To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](#secrets-manager-cli-secret-delete-command) that are associated with the group.
{: note}


```sh
ibmcloud secrets-manager secret-group-delete --id ID [--force]
```

#### Command options
{: #secrets-manager-secret-group-delete-cli-options}

`--id` (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`-f, --force`
:   Force a delete operation without a confirmation.

#### Examples
{: #secrets-manager-cli-secret-group-delete-command-examples}

```sh
ibmcloud secrets-manager secret-group-delete \
    --id=exampleString
```
{: pre}

## Secrets
{: #secrets-manager-secrets-cli}

Create, import, and manage different types of secrets for your apps and services.

### `ibmcloud secrets-manager secret-create`
{: #secrets-manager-cli-secret-create-command}

Create a secret or import an existing value that you can use to access or authenticate to a protected resource.

Use this method to either generate or import an existing secret, such as an arbitrary value or a TLS certificate, that you can manage in your Secrets Manager service instance. A successful request stores the secret in your dedicated instance based on the secret type and data that you specify. The response returns the ID value of the secret, along with other metadata.

To learn more about the types of secrets that you can create with Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

```sh
ibmcloud secrets-manager secret-create --secret-type SECRET-TYPE --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-create-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--resources` ([`SecretResource[]`](#cli-secret-resource-example-schema))
:   A collection of resources. Required.

#### Examples
{: #secrets-manager-cli-secret-create-command-examples}

```sh
ibmcloud secrets-manager secret-create \
    --secret-type=arbitrary \
    --resources='[{"name": "example-arbitrary-secret", "description": "Extended description for this secret.", "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", "labels": ["dev","us-south"], "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}, "expiration_date": "2030-01-01T00:00:00Z", "payload": "secret-data"}]'
```
{: pre}

### `ibmcloud secrets-manager secrets`
{: #secrets-manager-cli-secrets-command}

List the secrets in your Secrets Manager instance based on the type that you specify.

```sh
ibmcloud secrets-manager secrets --secret-type SECRET-TYPE [--limit LIMIT] [--offset OFFSET] 
```


#### Command options
{: #secrets-manager-secrets-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--limit` (int64)
:   The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `5000`. The minimum value is `1`.

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

#### Examples
{: #secrets-manager-cli-secrets-command-examples}

```sh
ibmcloud secrets-manager secrets \
    --secret-type=arbitrary \
    --limit=1 \
    --offset=0
```
{: pre}

### `ibmcloud secrets-manager all-secrets`
{: #secrets-manager-cli-all-secrets-command}

List all the secrets in your Secrets Manager instance.

```sh
ibmcloud secrets-manager all-secrets [--limit LIMIT] [--offset OFFSET] [--search SEARCH] [--sort-by SORT-BY] [--groups GROUPS] 
```


#### Command options
{: #secrets-manager-all-secrets-cli-options}

`--limit` (int64)
:   The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `5000`. The minimum value is `1`.

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

`--search` (string)
:   Filter secrets that contain the specified string. The fields that are searched include: id, name, description, labels, secret_type.

**Usage:** If you want to list only the secrets that contain the string "text", use
`../secrets/{secret_type}?search=text`.

    The maximum length is `128` characters.

`--sort-by` (string)
:   Sort a list of secrets by the specified field.

**Usage:** To sort a list of secrets by their creation date, use
`../secrets/{secret_type}?sort_by=creation_date`.

    Allowable values are: `id`, `creation_date`, `expiration_date`, `secret_type`, `name`.

`--groups` ([]string)
:   Filter secrets by groups.

You can apply multiple filters by using a comma-separated list of secret group IDs. If you need to filter secrets that are in the default secret group, use the `default` keyword.

**Usage:** To retrieve a list of secrets that are associated with an existing secret group or the default group, use `..?groups={secret_group_ID},default`.

    The list items must match regular expression `/^([0-9a-f]{8}(-[0-9a-f]{4}){3}-[0-9a-f]{12}|default)$/`.

#### Examples
{: #secrets-manager-cli-all-secrets-command-examples}

```sh
ibmcloud secrets-manager all-secrets \
    --limit=1 \
    --offset=0 \
    --search=exampleString \
    --sort-by=id \
    --groups=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret`
{: #secrets-manager-cli-secret-command}

Get a secret and its details by specifying the ID of the secret.

A successful request returns the secret data that is associated with your secret, along with other metadata. To view only the details of a specified secret without retrieving its value, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method.

```sh
ibmcloud secrets-manager secret --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Examples
{: #secrets-manager-cli-secret-command-examples}

```sh
ibmcloud secrets-manager secret \
    --secret-type=arbitrary \
    --id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-update`
{: #secrets-manager-cli-secret-update-command}

Invoke an action on a specified secret. This method supports the following actions:

- `rotate`: Replace the value of a secret.
- `restore`: Restore a previous version of an `iam_credentials` secret.
- `revoke`: Revoke a private certificate.
- `delete_credentials`: Delete the API key that is associated with an `iam_credentials` secret.
```sh
ibmcloud secrets-manager secret-update --secret-type SECRET-TYPE --id ID --action ACTION [--body BODY] 
```


#### Command options
{: #secrets-manager-secret-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--action` (string)
:   The action to perform on the specified secret. Required.

    Allowable values are: `rotate`, `restore`, `revoke`, `delete_credentials`.

`--body` ([`SecretAction`](#cli-secret-action-example-schema))
:   The properties to update for the secret.

#### Examples
{: #secrets-manager-cli-secret-update-command-examples}

```sh
ibmcloud secrets-manager secret-update \
    --secret-type=arbitrary \
    --id=exampleString \
    --action=rotate \
    --body='{"payload": "exampleString", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'
```
{: pre}

### `ibmcloud secrets-manager secret-delete`
{: #secrets-manager-cli-secret-delete-command}

Delete a secret by specifying the ID of the secret.

```sh
ibmcloud secrets-manager secret-delete --secret-type SECRET-TYPE --id ID [--force]
```


#### Command options
{: #secrets-manager-secret-delete-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`-f, --force`
:   Force a delete operation without a confirmation.    

#### Examples
{: #secrets-manager-cli-secret-delete-command-examples}

```sh
ibmcloud secrets-manager secret-delete \
    --secret-type=arbitrary \
    --id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-versions`
{: #secrets-manager-cli-secret-versions-command}

List the versions of a secret.

A successful request returns the list of the versions along with the metadata of each version.

```sh
ibmcloud secrets-manager secret-versions --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-versions-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Examples
{: #secrets-manager-cli-secret-versions-command-examples}

```sh
ibmcloud secrets-manager secret-versions \
    --secret-type=arbitrary \
    --id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-version`
{: #secrets-manager-cli-secret-version-command}

Get a version of a secret by specifying the ID of the version or the alias `previous`.

A successful request returns the secret data that is associated with the specified version of your secret, along with other metadata.

```sh
ibmcloud secrets-manager secret-version --secret-type SECRET-TYPE --id ID --version-id VERSION-ID 
```


#### Command options
{: #secrets-manager-secret-version-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details. Required.
{: note}

The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

#### Examples
{: #secrets-manager-cli-secret-version-command-examples}

```sh
ibmcloud secrets-manager secret-version \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-version-update`
{: #secrets-manager-cli-secret-version-update-command}

Invoke an action on a specified version of a secret. This method supports the following actions:

- `revoke`: Revoke a version of a private certificate.

```sh
ibmcloud secrets-manager secret-version-update --secret-type SECRET-TYPE --id ID --version-id VERSION-ID --action ACTION 
```


#### Command options
{: #secrets-manager-secret-version-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `private_cert`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details. Required.
{: note}

The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

`--action` (string)
:   The action to perform on the specified secret version. Required.

    Allowable values are: `revoke`.

#### Examples
{: #secrets-manager-cli-secret-version-update-command-examples}

```sh
ibmcloud secrets-manager secret-version-update \
    --secret-type=private_cert \
    --id=exampleString \
    --version-id=exampleString \
    --action=revoke
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-update-cli-output}

Revoke private certificate secret version

Example response for revoking a version of a `private_cert` secret.

```json
{
  "metadata" : {
    "collection_type" : "application/vnd.ibm.secrets-manager.secret+json",
    "collection_total" : 1
  },
  "resources" : [ {
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "creation_date" : "2022-03-07T13:06:15Z",
    "state" : 1,
    "state_description" : "Active",
    "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:24ec2c34-38ee-4038-9f1d-9a629423158d",
    "description" : "Extended description for this secret.",
    "expiration_date" : "2022-03-24T05:06:15Z",
    "id" : "24ec2c34-38ee-4038-9f1d-9a629423158d",
    "last_update_date" : "2022-03-16T09:55:32Z",
    "name" : "example-private-cert-secret",
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "secret_type" : "private_cert",
    "algorithm" : "SHA256-RSA",
    "key_algorithm" : "RSA2048",
    "serial_number" : "d9:be:fe:35:ba:09:42:b5",
    "certificate_authority" : "test-intermediate-CA",
    "certificate_template" : "test-certificate-template",
    "issuer" : "example.com",
    "common_name" : "localhost",
    "downloaded" : true,
    "rotation" : {
      "auto_rotate" : true,
      "interval" : 1,
      "unit" : "month"
    },
    "validity" : {
      "not_after" : "2022-03-24T05:06:15Z",
      "not_before" : "2022-03-07T13:05:46Z"
    },
    "versions_total" : 2,
    "versions" : [ {
      "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
      "creation_date" : "2022-03-06T09:44:52Z",
      "id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
      "state" : 3,
      "state_description" : "Deactivated",
      "revocation_time" : 1647425316,
      "revocation_time_rfc3339" : "2022-03-16T10:08:36Z",
      "payload_available" : true,
      "downloaded" : true,
      "serial_number" : "d9:be:fe:35:ba:09:42:b5",
      "expiration_date" : "2022-03-24T05:06:15Z",
      "version_custom_metadata" : {
        "custom_version_key" : "custom_version_value"
      },
      "validity" : {
        "not_after" : "2022-03-24T05:06:15Z",
        "not_before" : "2022-03-07T13:05:46Z"
      }
    }, {
      "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
      "creation_date" : "2022-03-07T13:06:15Z",
      "id" : "a89a962a-a518-11ec-b909-0242ac120003",
      "state" : 1,
      "state_description" : "Active",
      "payload_available" : true,
      "downloaded" : true,
      "serial_number" : "d9:be:fe:35:ba:09:42:b5",
      "expiration_date" : "2022-03-24T05:06:15Z",
      "version_custom_metadata" : {
        "custom_version_key" : "custom_version_value"
      },
      "validity" : {
        "not_after" : "2022-03-24T05:06:15Z",
        "not_before" : "2022-03-07T13:05:46Z"
      }
    } ],
    "locks_total" : 1,
    "custom_metadata" : {
      "custom_key" : "custom_value"
    }
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-version-metadata`
{: #secrets-manager-cli-secret-version-metadata-command}

Get the metadata of a secret version by specifying the ID of the version or the alias `previous`.

A successful request returns the metadata that is associated with the specified version of your secret.

```sh
ibmcloud secrets-manager secret-version-metadata --secret-type SECRET-TYPE --id ID --version-id VERSION-ID 
```


#### Command options
{: #secrets-manager-secret-version-metadata-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details.
{: note}

#### Examples
{: #secrets-manager-cli-secret-version-metadata-command-examples}

```sh
ibmcloud secrets-manager secret-version-metadata \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-version-metadata-update`
{: #secrets-manager-cli-secret-version-metadata-update-command}

Update the metadata of a secret version, such as `version_custom_metadata`.

```sh
ibmcloud secrets-manager secret-version-metadata-update --secret-type SECRET-TYPE --id ID --version-id VERSION-ID --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-version-metadata-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details. Required.
{: note}

The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

`--metadata` ([`CollectionMetadata`](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

`--resources` ([`UpdateSecretVersionMetadata[]`](#cli-update-secret-version-metadata-example-schema))
:   A collection of resources. Required.

#### Examples
{: #secrets-manager-secret-version-metadata-update-examples}

```sh
ibmcloud secrets-manager secret-version-metadata-update \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString \
    --metadata='{"collection_type": "application/vnd.ibm.secrets-manager.secret+json", "collection_total": 1}' \
    --resources='[{"version_custom_metadata": {"anyKey": "anyValue"}}]'
```
{: pre}


### `ibmcloud secrets-manager secret-metadata`
{: #secrets-manager-cli-secret-metadata-command}

Get the details of a secret by specifying its ID.

A successful request returns only metadata about the secret, such as its name and creation date. To retrieve the value of a secret, use the [Get a secret](#secrets-manager-cli-secret-command) or [Get a version of a secret](#secrets-manager-cli-secret-version-command) methods.

```sh
ibmcloud secrets-manager secret-metadata --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-metadata-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Examples
{: #secrets-manager-secret-metadata-cli-examples}

```sh
ibmcloud secrets-manager secret-metadata \
    --secret-type=arbitrary \
    --id=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-metadata-update`
{: #secrets-manager-cli-secret-metadata-update-command}

Update the metadata of a secret, such as its name or description.

To update the actual contents of a secret, rotate the secret by using the [Invoke an action on a secret](#secrets-manager-cli-secret-update-command) method.

```sh
ibmcloud secrets-manager secret-metadata-update --secret-type SECRET-TYPE --id ID --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-metadata-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--resources` ([`SecretMetadata[]`](#cli-secret-metadata-example-schema))
:   A collection of resources. Required.

#### Examples
{: #secrets-manager-cli-secret-metadata-update-command-examples}

```sh
ibmcloud secrets-manager secret-metadata-update \
    --secret-type=arbitrary \
    --id=exampleString \
    --resources='[{"labels": ["dev","us-south"], "name": "updated-secret-name", "description": "Updated description for this secret.", "custom_metadata": {"anyKey": "anyValue"}, "expiration_date": "2030-04-01T09:30:00Z"}]'
```
{: pre}

## Locks
{: #secrets-manager-locks-cli}

Secure your secret data by attaching a lock to a secret version and preventing any modification or deletion.

### `ibmcloud secrets-manager locks`
{: #secrets-manager-cli-locks-command}

List the locks that are associated with a specified secret.

```sh
ibmcloud secrets-manager locks --secret-type SECRET-TYPE --id ID [--limit LIMIT] [--offset OFFSET] [--search SEARCH] 
```


#### Command options
{: #secrets-manager-locks-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--limit` (int64)
:   The number of locks to retrieve. By default, list operations return the first 25 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `100`. The minimum value is `1`.

`--offset` (int64)
:   The number of locks to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

**Usage:** If you want to list only the locks that contain the string "text" in the field "name", use
`..?search=text`.
    The maximum length is `64` characters.

#### Examples
{: #secrets-manager-cli-locks-examples}

```sh
ibmcloud secrets-manager locks \
    --secret-type=arbitrary \
    --id=exampleString \
    --limit=1 \
    --offset=0 \
    --search=exampleString
```
{: pre}

#### Example output
{: #secrets-manager-locks-cli-output}

Example response for listing the locks that are associated with a specified secret.

```json
{
  "metadata" : {
    "collection_type" : "application/vnd.ibm.secrets-manager.secret.lock+json",
    "collection_total" : 2
  },
  "resources" : [ {
    "name" : "lock-1",
    "description" : "lock for consumer-1",
    "creation_date" : "2020-08-27T14:28:54Z",
    "created_by" : "IBM-id-1",
    "attributes" : {
      "key" : "value"
    },
    "secret_version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "secret_id" : "cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "last_update_date" : "2022-04-24T16:50:09.692573+03:00",
    "secret_version_alias" : "current"
  }, {
    "name" : "lock-2",
    "description" : "lock for consumer-2",
    "creation_date" : "2020-08-27T14:29:01Z",
    "created_by" : "IBM-id-1",
    "attributes" : {
      "key" : "value"
    },
    "secret_version_id" : "50277266-d706-4b3e-badb-f07257f8f581",
    "secret_id" : "cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "last_update_date" : "2022-04-27T18:50:09.692573+03:00",
    "secret_version_alias" : "previous"
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-lock`
{: #secrets-manager-cli-secret-lock-command}

Create a lock on the current version of a secret.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it exists. Additionally, you can use this method to clear any matching locks on a secret by using one of the following optional lock modes:

- `exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `exclusive_delete`: Same as `exclusive`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

For more information about locking secrets, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-locks).

```sh
ibmcloud secrets-manager secret-lock --secret-type SECRET-TYPE --id ID [--locks LOCKS] [--mode MODE] 
```


#### Command options
{: #secrets-manager-secret-lock-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--locks` ([LockSecretBodyLocksItem[]](#cli-lock-secret-body-locks-item-example-schema))
:   The lock data to be attached to a secret version.

`--mode` (string)
:   An optional lock mode. At lock creation, you can set one of the following modes to clear any matching locks on a secret version.

- `exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `exclusive_delete`: Same as `exclusive`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

    Allowable values are: `exclusive`, `exclusive_delete`.

#### Examples
{: #secrets-manager-secret-lock-cli-examples}

```sh
ibmcloud secrets-manager secret-lock \
    --secret-type=arbitrary \
    --id=exampleString \
    --locks='[{"name": "lock-1", "description": "lock for consumer-1", "attributes": {"anyKey": "anyValue"}}]' \
    --mode=exclusive
```
{: pre}

### `ibmcloud secrets-manager secret-unlock`
{: #secrets-manager-cli-secret-unlock-command}

Delete one or more locks that are associated with the current version of a secret.

A successful request deletes the locks that you specify. To remove all locks, you can pass `{"locks": ["*"]}` in the request body. Otherwise, specify the names of the locks that you want to delete. For example, `{"locks": ["lock1", "lock2"]}`.

A secret is considered unlocked and able to be revoked or deleted only after all of its locks are removed. To understand whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.
{: note}

```sh
ibmcloud secrets-manager secret-unlock --secret-type SECRET-TYPE --id ID [--locks LOCKS] 
```


#### Command options
{: #secrets-manager-secret-unlock-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--locks` ([]string)
:   A comma-separated list of locks to delete.

    The list items must match regular expression `/^(\\w(([\\w-.]+)?\\w)?|[*])$/`.

#### Examples
{: #secrets-manager-secret-unlock-cli-examples}

```sh
ibmcloud secrets-manager secret-unlock \
    --secret-type=arbitrary \
    --id=exampleString \
    --locks=exampleString
```
{: pre}

### `ibmcloud secrets-manager secret-version-locks`
{: #secrets-manager-cli-secret-version-locks-command}

List the locks that are associated with a specified secret version.

```sh
ibmcloud secrets-manager secret-version-locks --secret-type SECRET-TYPE --id ID --version-id VERSION-ID [--limit LIMIT] [--offset OFFSET] [--search SEARCH] 
```


#### Command options
{: #secrets-manager-secret-version-locks-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.
    
    The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details.
{: note}

--limit (int64)
:   The number of secrets with locks to retrieve. By default, list operations return the first 25 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `100`. The minimum value is `1`.

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

**Usage:** If you want to list only the locks that contain the string "text" in the field "name", use
`..?search=text`.

    The maximum length is `64` characters.

#### Examples
{: #secrets-manager-secret-version-locks-cli-examples}

```sh
ibmcloud secrets-manager secret-version-locks \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString \
    --limit=1 \
    --offset=0 \
    --search=exampleString
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-locks-cli-output}

Example response for listing the locks that are associated with a specified secret version.

```json
{
  "metadata" : {
    "collection_type" : "application/vnd.ibm.secrets-manager.secret.lock+json",
    "collection_total" : 2
  },
  "resources" : [ {
    "name" : "lock-1",
    "description" : "lock for consumer-1",
    "creation_date" : "2020-08-27T14:28:54Z",
    "created_by" : "IBM-id-1",
    "attributes" : {
      "key" : "value"
    },
    "secret_version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "secret_id" : "cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "last_update_date" : "2022-04-24T16:50:09.692573+03:00",
    "secret_version_alias" : "current"
  }, {
    "name" : "lock-2",
    "description" : "lock for consumer-2",
    "creation_date" : "2020-08-27T14:29:01Z",
    "created_by" : "IBM-id-1",
    "attributes" : {
      "key" : "value"
    },
    "secret_version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "secret_id" : "cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "last_update_date" : "2022-04-24T16:50:09.692573+03:00",
    "secret_version_alias" : "current"
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-version-lock`
{: #secrets-manager-cli-secret-version-lock-command}

Create a lock on the specified version of a secret.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to the specified version, or replaces a lock of the same name if it exists. Additionally, you can use this method to clear any matching locks on a secret version by using one of the following optional lock modes:

- `exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `exclusive_delete`: Same as `exclusive`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

For more information about locking secrets, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-locks).

```sh
ibmcloud secrets-manager secret-version-lock --secret-type SECRET-TYPE --id ID --version-id VERSION-ID [--locks LOCKS] [--mode MODE] 
```


#### Command options
{: #secrets-manager-secret-version-lock-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version. Required.

    The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

    To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details.
    {: note} 

`--locks` ([LockSecretBodyLocksItem[]](#cli-lock-secret-body-locks-item-example-schema))
:   The lock data to be attached to a secret version.

`--mode` (string)
:   An optional lock mode. At lock creation, you can set one of the following modes to clear any matching locks on a secret version.

When you are locking the `previous` version, the mode parameter is ignored.
{: note}

- `exclusive`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `exclusive_delete`: Same as `exclusive`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

    Allowable values are: `exclusive`, `exclusive_delete`.

#### Examples
{: #secrets-manager-secret-version-lock-cli-examples}

```sh
ibmcloud secrets-manager secret-version-lock \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString \
    --locks='[{"name": "lock-1", "description": "lock for consumer-1", "attributes": {"anyKey": "anyValue"}}]' \
    --mode=exclusive
```
{: pre}

### `ibmcloud secrets-manager secret-version-unlock`
{: #secrets-manager-cli-secret-version-unlock-command}

Delete one or more locks that are associated with the specified secret version.

A successful request deletes the locks that you specify. To remove all locks, you can pass `{"locks": ["*"]}` in the request body. Otherwise, specify the names of the locks that you want to delete. For example, `{"locks":
["lock-1", "lock-2"]}`.

A secret is considered unlocked and able to be revoked or deleted only after all of its locks are removed. To understand whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.
{: note}

```sh
ibmcloud secrets-manager secret-version-unlock --secret-type SECRET-TYPE --id ID --version-id VERSION-ID [--locks LOCKS] 
```


#### Command options
{: #secrets-manager-secret-version-unlock-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `private_cert`, `username_password`, `kv`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--version-id` (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version. Required.

    The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details.
{: note}

`--locks` ([]string)
:   A comma-separated list of locks to delete.

    The list items must match regular expression `/^(\\w(([\\w-.]+)?\\w)?|[*])$/`.

#### Examples
{: #secrets-manager-secret-version-unlock-cli-examples}

```sh
ibmcloud secrets-manager secret-version-unlock \
    --secret-type=arbitrary \
    --id=exampleString \
    --version-id=exampleString \
    --locks=exampleString
```
{: pre}

### `ibmcloud secrets-manager all-locks`
{: #secrets-manager-cli-all-locks-command}

List the lock details that are associated with all secrets in your Secrets Manager instance.

```sh
ibmcloud secrets-manager all-locks [--limit LIMIT] [--offset OFFSET] [--search SEARCH] [--groups GROUPS] 
```


#### Command options
{: #secrets-manager-all-locks-cli-options}

`--limit` (int64)
:   The number of secrets with locks to retrieve. By default, list operations return the first 25 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `100`. The minimum value is `1`.

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

**Usage:** If you want to list only the locks that contain the string "text" in the field "name", use
`..?search=text`.
    The maximum length is `64` characters.

`--groups` ([]string)
:   Filter secrets by groups.

    You can apply multiple filters by using a comma-separated list of secret group IDs. If you need to filter secrets that are in the default secret group, use the `default` keyword.


#### Examples
{: #secrets-manager-all-locks-cli-examples}

```sh
ibmcloud secrets-manager all-locks \
    --limit=1 \
    --offset=0 \
    --search=exampleString \
    --groups=exampleString
```
{: pre}

## Policies
{: #secrets-manager-policies-cli}

Define rotation policies for secrets.

### `ibmcloud secrets-manager policy-update`
{: #secrets-manager-cli-policy-update-command}

Create or update one or more policies, such as an [automatic rotation policy](/docs/secrets-manager?topic=secrets-manager-automatic-rotation), for the specified secret.

```sh
ibmcloud secrets-manager policy-update --secret-type SECRET-TYPE --id ID --resources RESOURCES [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `username_password`, `iam_credentials`,`public_cert`, `private_cert`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--resources` ([`SecretPolicyRotation[]`](#cli-secret-policy-rotation-example-schema))
:   A collection of resources. Required.

`--policy` (string)
:   The type of policy that is associated with the specified secret.

    Allowable values are: `rotation`.

#### Examples
{: #secrets-manager-cli-policy-update-command-examples}

```sh
ibmcloud secrets-manager policy-update \
    --secret-type=username_password \
    --id=exampleString \
    --resources='[{"type": "application/vnd.ibm.secrets-manager.secret.policy+json", "rotation": {"interval": 1, "unit": "day"}}]' \
    --policy=rotation
```
{: pre}

### `ibmcloud secrets-manager policy`
{: #secrets-manager-cli-policy-command}

List the rotation policies that are associated with a specified secret.

```sh
ibmcloud secrets-manager policy --secret-type SECRET-TYPE --id ID [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `username_password`, `public_cert`, `private_cert`.

`--id` (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--policy` (string)
:   The type of policy that is associated with the specified secret.

    Allowable values are: `rotation`.

#### Examples
{: #secrets-manager-cli-policy-command-examples}

```sh
ibmcloud secrets-manager policy \
    --secret-type=username_password \
    --id=exampleString \
    --policy=rotation
```
{: pre}

## Config
{: #secrets-manager-config-cli}

Configure secrets engines for your instance so that you can work with specific types of secrets.

### `ibmcloud secrets-manager config-update`
{: #secrets-manager-cli-config-update-command}

Set the configuration for the specified secret type.

Use this method to configure the IAM credentials (`iam_credentials`) engine for your service instance. Looking to order or generate certificates? To configure the public certificates (`public_cert`) or private certificates (`private_cert`) engines, use the [Add a configuration](#secrets-manager-cli-config-element-create-command) method.

```sh
ibmcloud secrets-manager config-update --secret-type SECRET-TYPE --engine-config ENGINE-CONFIG 
```


#### Command options
{: #secrets-manager-config-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `iam_credentials`.

`--engine-config` ([`EngineConfig`](#cli-engine-config-example-schema))
:   Properties to update for a secrets engine. Required.

#### Examples
{: #secrets-manager-cli-config-update-command-examples}

```sh
ibmcloud secrets-manager config-update \
    --secret-type=iam_credentials \
    --engine-config='{"api_key": "API_KEY"}'
```
{: pre}

### `ibmcloud secrets-manager config`
{: #secrets-manager-cli-config-command}

Get the configuration that is associated with the specified secret type.

```sh
ibmcloud secrets-manager config --secret-type SECRET-TYPE 
```


#### Command options
{: #secrets-manager-config-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `iam_credentials`, `public_cert`, `private_cert`.

#### Examples
{: #secrets-manager-cli-config-command-examples}

```sh
ibmcloud secrets-manager config \
    --secret-type=iam_credentials
```
{: pre}

### `ibmcloud secrets-manager config-element-create`
{: #secrets-manager-cli-config-element-create-command}

Add a configuration element to the specified secret type.

Use this method to define the configurations that are required to enable the public certificates (`public_cert`) and private certificates (`private_cert`) engines.

You can add multiple configurations for your instance as follows:

- Up to 10 public certificate authority configurations
- Up to 10 DNS provider configurations
- Up to 10 private root certificate authority configurations
- Up to 10 private intermediate certificate authority configurations
- Up to 10 certificate templates.

```sh
ibmcloud secrets-manager config-element-create --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --name NAME --type TYPE --config CONFIG 
```


#### Command options
{: #secrets-manager-config-element-create-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `public_cert`, `private_cert`.

`--config-element` (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`, `root_certificate_authorities`, `intermediate_certificate_authorities`, `certificate_templates`.

`--name` (string)
:   The human-readable name to assign to your configuration. Required.

    The maximum length is `256` characters. The minimum length is `2` characters.

`--type` (string)
:   The type of configuration. Value options differ depending on the `config_element` property that you want to define. Required.

    Allowable values are: `letsencrypt`, `letsencrypt-stage`, `cis`, `classic_infrastructure`, `root_certificate_authority`, `intermediate_certificate_authority`, `certificate_template`. The maximum length is `128` characters. The minimum length is `2` characters.

`--config` ([`ConfigElementDefConfig`](#cli-config-element-def-config-example-schema))
:   The configuration to define for the specified secret type. Required.

#### Examples
{: #secrets-manager-cli-config-element-create-command-examples}

```sh
ibmcloud secrets-manager config-element-create \
    --secret-type=public_cert \
    --config-element=certificate_authorities \
    --name=cis-example-config \
    --type=cis \
    --config='{"cis_crn": "crn:v1:bluemix:public:internet-svcs:global:a/<account-id>:<service-instance>::", "cis_apikey": "cis_apikey_value"}'
```
{: pre}

### `ibmcloud secrets-manager config-elements`
{: #secrets-manager-cli-config-elements-command}

List the configuration elements that are associated with a specified secret type.

```sh
ibmcloud secrets-manager config-elements --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT 
```


#### Command options
{: #secrets-manager-config-elements-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `public_cert`, `private_cert`.

`--config-element` (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`, `root_certificate_authorities`, `intermediate_certificate_authorities`, `certificate_templates`.

#### Examples
{: #secrets-manager-cli-config-elements-command-examples}

```sh
ibmcloud secrets-manager config-elements \
    --secret-type=public_cert \
    --config-element=certificate_authorities
```
{: pre}

### `ibmcloud secrets-manager config-element`
{: #secrets-manager-cli-config-element-command}

Get the details of a specific configuration that is associated with a secret type.

```sh
ibmcloud secrets-manager config-element --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME 
```


#### Command options
{: #secrets-manager-config-element-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `public_cert`, `private_cert`.

`--config-element` (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`, `root_certificate_authorities`, `intermediate_certificate_authorities`, `certificate_templates`.

`--config-name` (string)
:   The name of your configuration. Required.

#### Examples
{: #secrets-manager-cli-config-element-command-examples}

```sh
ibmcloud secrets-manager config-element \
    --secret-type=public_cert \
    --config-element=certificate_authorities \
    --config-name=exampleString
```
{: pre}

### `ibmcloud secrets-manager config-element-update`
{: #secrets-manager-cli-config-element-update-command}

Update a configuration element that is associated with the specified secret type.

```sh
ibmcloud secrets-manager config-element-update --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME --type TYPE --config CONFIG 
```


#### Command options
{: #secrets-manager-config-element-update-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `public_cert`, `private_cert`.

`--config-element` (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`, `root_certificate_authorities`, `intermediate_certificate_authorities`, `certificate_templates`.

`--config-name` (string)
:   The name of your configuration. Required.

`--type` (string)
:   The type of configuration. Value options differ depending on the `config_element` property that you want to define. Required.

    Allowable values are: `letsencrypt`, `letsencrypt-stage`, `cis`, `classic_infrastructure`, `root_certificate_authority`, `intermediate_certificate_authority`, `certificate_template`. The maximum length is `128` characters. The minimum length is `2` characters.

`--config` (interface{})
:   Properties that describe a configuration, which depends on type. Required.

#### Examples
{: #secrets-manager-cli-config-element-update-examples}

```sh
ibmcloud secrets-manager config-element-update \
    --secret-type=public_cert \
    --config-element=certificate_authorities \
    --config-name=exampleString \
    --type=cis \
    --config='{"anyKey": "anyValue"}'
```
{: pre}

### `ibmcloud secrets-manager config-element-action`
{: #secrets-manager-cli-config-element-action-command}

Invoke an action on a specified configuration element. This method supports the following actions:

- `sign_intermediate`: Sign an intermediate certificate authority.
- `sign_csr`: Sign a certificate signing request.
- `set_signed`: Set a signed intermediate certificate authority.
- `revoke`: Revoke an internally signed intermediate certificate authority certificate.
- `rotate_crl`: Rotate the certificate revocation list (CRL) of an intermediate certificate authority.

```sh
ibmcloud secrets-manager config-element-action --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME --action ACTION [--config CONFIG] 
```


#### Command options
{: #secrets-manager-config-element-action-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `private_cert`.

`--config-element` (string)
:   The configuration element on which the action is applied. Required.

    Allowable values are: `root_certificate_authorities`, `intermediate_certificate_authorities`.

`--config-name` (string)
:   The name of the certificate authority. Required.

`--action` (string)
:   The action to perform on the specified configuration element. Required.

    Allowable values are: `sign_intermediate`, `sign_csr`, `set_signed`, `revoke`, `rotate_crl`.

`--config` ([`ConfigAction`](#cli-config-action-example-schema))
:   Properties that describe an action on a configuration element.

#### Examples
{: #secrets-manager-cli-config-element-action-command-examples}

```sh
ibmcloud secrets-manager config-element-action \
    --secret-type=private_cert \
    --config-element=root_certificate_authorities \
    --config-name=exampleString \
    --action=sign_intermediate \
    --config='{"common_name": "example.com", "alt_names": "exampleString", "ip_sans": "exampleString", "uri_sans": "exampleString", "other_sans": ["exampleString"], "ttl": "12h", "format": "pem", "max_path_length": 38, "exclude_cn_from_sans": false, "permitted_dns_domains": ["exampleString"], "use_csr_values": false, "ou": ["exampleString"], "organization": ["exampleString"], "country": ["exampleString"], "locality": ["exampleString"], "province": ["exampleString"], "street_address": ["exampleString"], "postal_code": ["exampleString"], "serial_number": "d9:be:fe:35:ba:09:42:b5", "csr": "exampleString"}'
```
{: pre}

### `ibmcloud secrets-manager config-element-delete`
{: #secrets-manager-cli-config-element-delete-command}

Delete a configuration element from the specified secret type.

```sh
ibmcloud secrets-manager config-element-delete --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME 
```


#### Command options
{: #secrets-manager-config-element-delete-cli-options}

`--secret-type` (string)
:   The secret type. Required.

    Allowable values are: `public_cert`, `private_cert`.

`--config-element` (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`, `root_certificate_authorities`, `intermediate_certificate_authorities`, `certificate_templates`.

`--config-name` (string)
:   The name of your configuration. Required.

#### Examples
{: #secrets-manager-cli-config-element-delete-command-examples}

```sh
ibmcloud secrets-manager config-element-delete \
    --secret-type=public_cert \
    --config-element=certificate_authorities \
    --config-name=exampleString
```
{: pre}

## Notifications
{: #secrets-manager-notifications-cli}

Enable lifecycle notifications for your instance by connecting to Event Notifications.

### `ibmcloud secrets-manager notifications-registration-create`
{: #secrets-manager-cli-notifications-registration-create-command}

Create a registration between a Secrets Manager instance and [Event Notifications](https://cloud.ibm.com/apidocs/event-notifications).

A successful request adds Secrets Manager as a source that you can reference from your Event Notifications instance. For more information about enabling notifications for Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-event-notifications).

```sh
ibmcloud secrets-manager notifications-registration-create --event-notifications-instance-crn EVENT-NOTIFICATIONS-INSTANCE-CRN --event-notifications-source-name EVENT-NOTIFICATIONS-SOURCE-NAME [--event-notifications-source-description EVENT-NOTIFICATIONS-SOURCE-DESCRIPTION] 
```


#### Command options
{: #secrets-manager-notifications-registration-create-cli-options}

`--event-notifications-instance-crn` (string)
:   The Cloud Resource Name (CRN) of the connected Event Notifications instance. Required.

`--event-notifications-source-name` (string)
:   The name that is displayed as a source in your Event Notifications instance. Required.

`--event-notifications-source-description` (string)
:   An optional description for the source in your Event Notifications instance.

#### Examples
{: #secrets-manager-cli-notifications-registration-create-command-examples}

```sh
ibmcloud secrets-manager notifications-registration-create \
    --event-notifications-instance-crn=crn:v1:bluemix:public:event-notifications:us-south:a/<account-id>:<service-instance>:: \
    --event-notifications-source-name='My Secrets Manager' \
    --event-notifications-source-description='Optional description of this source in an Event Notifications instance.'
```
{: pre}

### `ibmcloud secrets-manager notifications-registration`
{: #secrets-manager-cli-notifications-registration-command}

Get the details of an existing registration between a Secrets Manager instance and Event Notifications.

```sh
ibmcloud secrets-manager notifications-registration 
```


#### Examples
{: #secrets-manager-cli-notifications-registration-command-examples}

```sh
ibmcloud secrets-manager notifications-registration
```
{: pre}

### `ibmcloud secrets-manager notifications-registration-delete`
{: #secrets-manager-cli-notifications-registration-delete-command}

Delete a registration between a Secrets Manager instance and Event Notifications.

A successful request removes your Secrets Manager instance as a source in Event Notifications.

```sh
ibmcloud secrets-manager notifications-registration-delete 
```


#### Examples
{: #secrets-manager-cli-notifications-registration-delete-command-examples}

```sh
ibmcloud secrets-manager notifications-registration-delete
```
{: pre}

### `ibmcloud secrets-manager notifications-test`
{: #secrets-manager-cli-notifications-test-command}

Send a test event from a Secrets Manager instance to a configured [Event Notifications](https://cloud.ibm.com/apidocs/event-notifications) instance.

A successful request sends a test event to the Event Notifications instance. For more information about enabling notifications for Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-event-notifications).

```sh
ibmcloud secrets-manager notifications-test 
```
{: pre}


#### Examples
{: #secrets-manager-cli-notifications-test-command-examples}

```sh
ibmcloud secrets-manager notifications-test
```
{: pre}

## Schema examples
{: #secrets-manager-schema-examples}

The following schema examples represent the data that you need to specify for a command option. These examples model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment.

### CollectionMetadata
{: #cli-collection-metadata-example-schema}

The following example shows the format of the CollectionMetadata object.

```json

{
  "collection_type" : "application/vnd.ibm.secrets-manager.secret.group+json",
  "collection_total" : 1
}
```
{: codeblock}

### ConfigAction
{: #cli-config-action-example-schema}

The following example shows the format of the ConfigAction object.

```json

{
  "common_name" : "example.com",
  "alt_names" : "exampleString",
  "ip_sans" : "exampleString",
  "uri_sans" : "exampleString",
  "other_sans" : [ "exampleString" ],
  "ttl" : "12h",
  "format" : "pem",
  "max_path_length" : 38,
  "exclude_cn_from_sans" : false,
  "permitted_dns_domains" : [ "exampleString" ],
  "use_csr_values" : false,
  "ou" : [ "exampleString" ],
  "organization" : [ "exampleString" ],
  "country" : [ "exampleString" ],
  "locality" : [ "exampleString" ],
  "province" : [ "exampleString" ],
  "street_address" : [ "exampleString" ],
  "postal_code" : [ "exampleString" ],
  "serial_number" : "d9:be:fe:35:ba:09:42:b5",
  "csr" : "exampleString"
}
```
{: codeblock}

### ConfigElementDefConfig
{: #cli-config-element-def-config-example-schema}

The following example shows the format of the ConfigElementDefConfig object.

```json

{
  "cis_crn" : "crn:v1:bluemix:public:internet-svcs:global:a/<account-id>:<service-instance>::",
  "cis_apikey" : "cis_apikey_value"
}
```
{: codeblock}

### EngineConfig
{: #cli-engine-config-example-schema}

The following example shows the format of the EngineConfig object.

```json

{
  "api_key" : "API_KEY"
}
```
{: codeblock}

### LockSecretBodyLocksItem[]
{: #cli-lock-secret-body-locks-item-example-schema}

The following example shows the format of the LockSecretBodyLocksItem[] object.

```json

[ {
  "name" : "lock-1",
  "description" : "lock for consumer-1",
  "attributes" : {
    "anyKey" : "anyValue"
  }
} ]
```
{: codeblock}

### SecretAction
{: #cli-secret-action-example-schema}

The following example shows the format of the SecretAction object.

```json

{
   "payload" : "exampleString",
  "custom_metadata" : {
    "anyKey" : "anyValue"
  },
  "version_custom_metadata" : {
    "anyKey" : "anyValue"
  }
}
```
{: codeblock}

### SecretGroupMetadataUpdatable[]
{: #cli-secret-group-metadata-updatable-example-schema}

The following example shows the format of the SecretGroupMetadataUpdatable[] object.

```json

[ {
  "name" : "updated-secret-group-name",
  "description" : "Updated description for this group."
} ]
```
{: codeblock}

### SecretGroupResource[]
{: #cli-secret-group-resource-example-schema}

The following example shows the format of the SecretGroupResource[] object.

```json

[ {
  "name" : "my-secret-group",
  "description" : "Extended description for this group."
} ]
```
{: codeblock}

### SecretMetadata[]
{: #cli-secret-metadata-example-schema}

The following example shows the format of the SecretMetadata[] object.

```json

[ {
  "labels" : [ "dev", "us-south" ],
  "name" : "updated-secret-name",
  "description" : "Updated description for this secret.",
  "expiration_date" : "2030-04-01T09:30:00Z"
} ]
```
{: codeblock}

### SecretPolicyRotation[]
{: #cli-secret-policy-rotation-example-schema}

The following example shows the format of the SecretPolicyRotation[] object.

```json

[ {
  "type" : "application/vnd.ibm.secrets-manager.secret.policy+json",
  "rotation" : {
    "interval" : 1,
    "unit" : "day"
  }
} ]
```
{: codeblock}

### SecretResource[]
{: #cli-secret-resource-example-schema}

The following example shows the format of the SecretResource[] object.

```json

[ {
  "name" : "example-arbitrary-secret",
  "description" : "Extended description for this secret.",
  "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
  "labels" : [ "dev", "us-south" ],
  "custom_metadata" : {
    "anyKey" : "anyValue"
  },
  "version_custom_metadata" : {
    "anyKey" : "anyValue"
  },
  "expiration_date" : "2030-01-01T00:00:00Z",
  "payload" : "secret-data"
} ]
```
{: codeblock}

### UpdateSecretVersionMetadata[]
{: #cli-update-secret-version-metadata-example-schema}

The following example shows the format of the UpdateSecretVersionMetadata[] object.

```json

[ {
  "version_custom_metadata" : {
    "anyKey" : "anyValue"
  }
} ]
```
{: codeblock}
