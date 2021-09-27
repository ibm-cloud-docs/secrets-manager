---

copyright:
  years: 2021
lastupdated: "2021-09-27"

subcollection: secrets-manager

keywords: Secrets Manager CLI, Secrets Manager command line , Secrets Manager terminal, Secrets Manager shell

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

# {{site.data.keyword.secrets-manager_short}} CLI
{: #secrets-manager-cli}

You can use the {{site.data.keyword.secrets-manager_full}} command-line interface (CLI) to manage secrets in your {{site.data.keyword.secrets-manager_short}} instance.
{: shortdesc}

Current version: **`0.1.12`**

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

    If you're using plug-in version `0.0.8` or later, export the following variable.
    ```
    export SECRETS_MANAGER_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
    ```
    {: pre}

    If you're using plug-in version `0.0.6` or earlier, export the following variable.
    ```
    export IBM_CLOUD_SECRETS_MANAGER_API_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
    ```
    {: pre}

    Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

    If you copy your service endpoint URL from the {{site.data.keyword.secrets-manager_short}} UI, be sure to trim `/api` from the URL before you export it as variable to use with the CLI plug-in.
    {: important}

    





## Secret groups
{: #secrets-manager-secret-groups-cli}

Control who on your team has access to secrets by creating and managing groups.

### ibmcloud secrets-manager secret-group-create
{: #secrets-manager-cli-secret-group-create-command}

Creates a secret group that you can use to organize secrets and control who on your team has access to them.

A successful request returns the ID value of the secret group, along with other metadata. To learn more about secret groups, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-groups).

```sh
ibmcloud secrets-manager secret-group-create --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-group-create-cli-options}

--metadata ([CollectionMetadata](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

--resources ([SecretGroupResource[]](#cli-secret-group-resource-example-schema))
:   A collection of resources. Required.

### ibmcloud secrets-manager secret-groups
{: #secrets-manager-cli-secret-groups-command}

Retrieves the list of secret groups that are available in your Secrets Manager instance.

```sh
ibmcloud secrets-manager secret-groups 
```


### ibmcloud secrets-manager secret-group
{: #secrets-manager-cli-secret-group-command}

Retrieves the metadata of an existing secret group by specifying the ID of the group.

```sh
ibmcloud secrets-manager secret-group --id ID 
```


#### Command options
{: #secrets-manager-secret-group-cli-options}

--id (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

### ibmcloud secrets-manager secret-group-metadata-update
{: #secrets-manager-cli-secret-group-metadata-update-command}

Updates the metadata of an existing secret group, such as its name or description.

```sh
ibmcloud secrets-manager secret-group-metadata-update --id ID --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-group-metadata-update-cli-options}

--id (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--metadata ([CollectionMetadata](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

--resources ([SecretGroupMetadataUpdatable[]](#cli-secret-group-metadata-updatable-example-schema))
:   A collection of resources. Required.

### ibmcloud secrets-manager secret-group-delete
{: #secrets-manager-cli-secret-group-delete-command}

Deletes a secret group by specifying the ID of the secret group.

**Note:** To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](#secrets-manager-cli-secret-delete-command) that are associated with the group.

```sh
ibmcloud secrets-manager secret-group-delete --id ID [--force]
```


#### Command options
{: #secrets-manager-secret-group-delete-cli-options}

--id (string)
:   The v4 UUID that uniquely identifies the secret group. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

-f, --force
:   Force the operation without a confirmation.

## Secrets
{: #secrets-manager-secrets-cli}

Create, import, and manage different types of secrets for your apps and services.

### ibmcloud secrets-manager secret-create
{: #secrets-manager-cli-secret-create-command}

Creates a secret or imports an existing value that you can use to access or authenticate to a protected resource.

Use this method to either generate or import an existing secret, such as an arbitrary value or a TLS certificate, that you can manage in your Secrets Manager service instance. A successful request stores the secret in your dedicated instance based on the secret type and data that you specify. The response returns the ID value of the secret, along with other metadata.

To learn more about the types of secrets that you can create with Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

```sh
ibmcloud secrets-manager secret-create --secret-type SECRET-TYPE --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-create-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--metadata ([CollectionMetadata](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

--resources ([SecretResource[]](#cli-secret-resource-example-schema))
:   A collection of resources. Required.

### ibmcloud secrets-manager secrets
{: #secrets-manager-cli-secrets-command}

Retrieves a list of secrets based on the type that you specify.

```sh
ibmcloud secrets-manager secrets --secret-type SECRET-TYPE [--limit LIMIT] [--offset OFFSET] 
```


#### Command options
{: #secrets-manager-secrets-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--limit (int64)
:   The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `5000`. The minimum value is `1`.

--offset (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

### ibmcloud secrets-manager all-secrets
{: #secrets-manager-cli-all-secrets-command}

Retrieves a list of all secrets in your Secrets Manager instance.

```sh
ibmcloud secrets-manager all-secrets [--limit LIMIT] [--offset OFFSET] [--search SEARCH] [--sort-by SORT-BY] [--groups GROUPS] 
```


#### Command options
{: #secrets-manager-all-secrets-cli-options}

--limit (int64)
:   The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.

    The maximum value is `5000`. The minimum value is `1`.

--offset (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

    The minimum value is `0`.

--search (string)
:   Filter secrets that contain the specified string. The fields that are searched include: id, name, description, labels, secret_type.

**Usage:** If you want to list only the secrets that contain the string "text", use
`../secrets/{secret-type}?search=text`.

    The maximum length is `128` characters.

--sort-by (string)
:   Sort a list of secrets by the specified field.

**Usage:** To sort a list of secrets by their creation date, use
`../secrets/{secret-type}?sort_by=creation_date`.

    Allowable values are: `id`, `creation_date`, `expiration_date`, `secret_type`, `name`.

--groups ([]string)
:   Filter secrets by groups.

You can apply multiple filters by using a comma-separated list of secret group IDs. If you need to filter secrets that are in the default secret group, use the `default` keyword.

**Usage:** To retrieve a list of secrets that are associated with an existing secret group or the default group, use `../secrets?groups={secret_group_ID},default`.

### ibmcloud secrets-manager secret
{: #secrets-manager-cli-secret-command}

Retrieves a secret and its details by specifying the ID of the secret.

A successful request returns the secret data that is associated with your secret, along with other metadata. To view only the details of a specified secret without retrieving its value, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method.

```sh
ibmcloud secrets-manager secret --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

### ibmcloud secrets-manager secret-update
{: #secrets-manager-cli-secret-update-command}

Invokes an action on a specified secret. This method supports the following actions:

- `rotate`: Replace the value of an `arbitrary`, `username_password`, `public_cert` or `imported_cert` secret. 
- `delete_credentials`: Delete the API key that is associated with an `iam_credentials` secret.

```sh
ibmcloud secrets-manager secret-update --secret-type SECRET-TYPE --id ID --action ACTION --body BODY 
```


#### Command options
{: #secrets-manager-secret-update-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--action (string)
:   The action to perform on the specified secret. Required.

    Allowable values are: `rotate`, `delete_credentials`.

--body ([SecretAction](#cli-secret-action-example-schema))
:   The properties to update for the secret. Required.

#### Example output
{: #secrets-manager-secret-update-cli-output}

Example response for rotating a `username_password` secret.

```json
{
  "metadata" : {
    "collection_type" : "application/vnd.ibm.secrets-manager.secret+json",
    "collection_total" : 1
  },
  "resources" : [ {
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "creation_date" : "2020-10-05T21:33:11Z",
    "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:24ec2c34-38ee-4038-9f1d-9a629423158d",
    "description" : "Extended description for this secret.",
    "expiration_date" : "2021-01-01T00:00:00Z",
    "id" : "24ec2c34-38ee-4038-9f1d-9a629423158d",
    "labels" : [ "dev", "us-south" ],
    "last_update_date" : "2020-10-05T21:33:11Z",
    "name" : "example-username-password-secret",
    "next_rotation_at" : "2020-12-24T03:36:27Z",
    "secret_data" : {
      "username" : "user123",
      "password" : "rainy-cloudy-coffee-book"
    },
    "secret_group_id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "secret_type" : "username_password",
    "state" : 1,
    "state_description" : "Active",
    "versions_total" : 1
  } ]
}
```
{: screen}

### ibmcloud secrets-manager secret-delete
{: #secrets-manager-cli-secret-delete-command}

Deletes a secret by specifying the ID of the secret.

```sh
ibmcloud secrets-manager secret-delete --secret-type SECRET-TYPE --id ID [--force]
```


#### Command options
{: #secrets-manager-secret-delete-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

-f, --force
:   Force the operation without a confirmation.

### ibmcloud secrets-manager secret-version
{: #secrets-manager-cli-secret-version-command}

Retrieves a version of a secret by specifying the ID of the version or the alias `previous`.

A successful request returns the secret data that is associated with the specified version of your secret, along with other metadata.

```sh
ibmcloud secrets-manager secret-version --secret-type SECRET-TYPE --id ID --version-id VERSION-ID 
```


#### Command options
{: #secrets-manager-secret-version-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `imported_cert`, `public_cert`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--version-id (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

**Note:** To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details. Required.

    The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

#### Example output
{: #secrets-manager-secret-version-cli-output}

Get certificate version.

```json
{
  "id" : "24ec2c34-38ee-4038-9f1d-9a629423158d",
  "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:24ec2c34-38ee-4038-9f1d-9a629423158d",
  "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
  "payload_available" : true,
  "serial_number" : "d9:be:fe:35:ba:09:42:b5",
  "expiration_date" : "2021-01-01T00:00:00Z",
  "validity" : {
    "not_before" : "2020-10-05T21:33:11Z",
    "not_after" : "2021-01-01T00:00:00Z"
  },
  "secret_data" : {
    "certificate" : "certificate_content",
    "private_key" : "certificate_private_key",
    "intermediate" : "intermediate_certificate_content"
  }
}
```
{: screen}

### ibmcloud secrets-manager secret-version-metadata
{: #secrets-manager-cli-secret-version-metadata-command}

Retrieves secret version metadata by specifying the ID of the version or the alias `previous`.

A successful request returns the metadata that is associated with the specified version of your secret.

```sh
ibmcloud secrets-manager secret-version-metadata --secret-type SECRET-TYPE --id ID --version-id VERSION-ID 
```


#### Command options
{: #secrets-manager-secret-version-metadata-cli-options}

--secret-type (string)
:   The secret type. Supported options include: imported_cert. Required.

    Allowable values are: `imported_cert`, `public_cert`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--version-id (string)
:   The v4 UUID that uniquely identifies the secret version. You can also use `previous` to retrieve the previous version.

**Note:** To find the version ID of a secret, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command) method and check the response details. Required.

    The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|previous)$/`.

#### Example output
{: #secrets-manager-secret-version-metadata-cli-output}

Get certificate version.

```json
{
  "id" : "24ec2c34-38ee-4038-9f1d-9a629423158d",
  "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:24ec2c34-38ee-4038-9f1d-9a629423158d",
  "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
  "payload_available" : true,
  "serial_number" : "d9:be:fe:35:ba:09:42:b5",
  "expiration_date" : "2021-01-01T00:00:00Z",
  "validity" : {
    "not_before" : "2020-10-05T21:33:11Z",
    "not_after" : "2021-01-01T00:00:00Z"
  }
}
```
{: screen}

### ibmcloud secrets-manager secret-metadata
{: #secrets-manager-cli-secret-metadata-command}

Retrieves the details of a secret by specifying the ID.

A successful request returns only metadata about the secret, such as its name and creation date. To retrieve the value of a secret, use the [Get a secret](#secrets-manager-cli-secret-command) or [Get a version of a secret](#secrets-manager-cli-secret-version-command) methods.

```sh
ibmcloud secrets-manager secret-metadata --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-metadata-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

### ibmcloud secrets-manager secret-metadata-update
{: #secrets-manager-cli-secret-metadata-update-command}

Updates the metadata of a secret, such as its name or description.

To update the actual contents of a secret, rotate the secret by using the [Invoke an action on a secret](#secrets-manager-cli-secret-update-command) method.

```sh
ibmcloud secrets-manager secret-metadata-update --secret-type SECRET-TYPE --id ID --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-metadata-update-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `arbitrary`, `iam_credentials`, `imported_cert`, `public_cert`, `username_password`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--metadata ([CollectionMetadata](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

--resources ([SecretMetadata[]](#cli-secret-metadata-example-schema))
:   A collection of resources. Required.

## Policies
{: #secrets-manager-policies-cli}

Define rotation policies for secrets.

### ibmcloud secrets-manager policy-update
{: #secrets-manager-cli-policy-update-command}

Creates or updates one or more policies, such as an [automatic rotation policy](http://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-rotate-secrets#auto-rotate-secret), for the specified secret.

```sh
ibmcloud secrets-manager policy-update --secret-type SECRET-TYPE --id ID --metadata METADATA --resources RESOURCES [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-update-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `username_password`, `public_cert`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--metadata ([CollectionMetadata](#cli-collection-metadata-example-schema))
:   The metadata that describes the resource array. Required.

--resources ([SecretPolicyRotation[]](#cli-secret-policy-rotation-example-schema))
:   A collection of resources. Required.

--policy (string)
:   The type of policy that is associated with the specified secret.

    Allowable values are: `rotation`.

### ibmcloud secrets-manager policy
{: #secrets-manager-cli-policy-command}

Retrieves a list of policies that are associated with a specified secret.

```sh
ibmcloud secrets-manager policy --secret-type SECRET-TYPE --id ID [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `username_password`, `public_cert`.

--id (string)
:   The v4 UUID that uniquely identifies the secret. Required.

    The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

--policy (string)
:   The type of policy that is associated with the specified secret.

    Allowable values are: `rotation`.

## Config
{: #secrets-manager-config-cli}

Configure secrets engines for your instance so that you can work with specific types of secrets.

### ibmcloud secrets-manager config-update
{: #secrets-manager-cli-config-update-command}

Sets the configuration for the specified secret type.

Use this method to configure the IAM credentials (`iam_credentials`) engine for your service instance. Looking to set up certificate ordering? To configure the public certificates (`public_cert`) engine, use the [Add a configuration](#secrets-manager-cli-config-element-create-command) method.

```sh
ibmcloud secrets-manager config-update --secret-type SECRET-TYPE 
```


#### Command options
{: #secrets-manager-config-update-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `iam_credentials`, `public_cert`.

### ibmcloud secrets-manager config
{: #secrets-manager-cli-config-command}

Retrieves the configuration that is associated with the specified secret type.

```sh
ibmcloud secrets-manager config --secret-type SECRET-TYPE 
```


#### Command options
{: #secrets-manager-config-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `iam_credentials`, `public_cert`.

### ibmcloud secrets-manager config-element-create
{: #secrets-manager-cli-config-element-create-command}

Adds a configuration element to the specified secret type.

Use this method to define the configurations that are required to enable the public certificates (`public_cert`) engine. You can add up to 10 certificate authority and DNS provider configurations for your instance.

```sh
ibmcloud secrets-manager config-element-create --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --name NAME --type TYPE --config CONFIG 
```


#### Command options
{: #secrets-manager-config-element-create-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `public_cert`.

--config-element (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`.

--name (string)
:   The human-readable name to assign to your configuration. Required.

    The maximum length is `256` characters. The minimum length is `2` characters.

--type (string)
:   The type of configuration. Value options differ depending on the `config_element` property that you want to define. Required.

    Allowable values are: `letsencrypt`, `letsencrypt-stage`, `cis`, `classic_infrastructure`. The maximum length is `128` characters. The minimum length is `2` characters.

--config ([ConfigElementDefConfig](#cli-config-element-def-config-example-schema))
:   The configuration to define for the specified secret type. Required.

### ibmcloud secrets-manager config-elements
{: #secrets-manager-cli-config-elements-command}

Lists the configuration elements that are associated with a specified secret type.

```sh
ibmcloud secrets-manager config-elements --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT 
```


#### Command options
{: #secrets-manager-config-elements-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `public_cert`.

--config-element (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`.

### ibmcloud secrets-manager config-element-update
{: #secrets-manager-cli-config-element-update-command}

Updates a configuration element that is associated with the specified secret type.

```sh
ibmcloud secrets-manager config-element-update --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME --type TYPE --config CONFIG 
```


#### Command options
{: #secrets-manager-config-element-update-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `public_cert`.

--config-element (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`.

--config-name (string)
:   The name of your configuration. Required.

--type (string)
:   The type of configuration. Value options differ depending on the `config_element` property that you want to define. Required.

    Allowable values are: `letsencrypt`, `letsencrypt-stage`, `cis`, `classic_infrastructure`. The maximum length is `128` characters. The minimum length is `2` characters.

--config (interface{})
:   &nbsp; Required.

### ibmcloud secrets-manager config-element-delete
{: #secrets-manager-cli-config-element-delete-command}

Removes a configuration element from the specified secret type.

```sh
ibmcloud secrets-manager config-element-delete --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME 
```


#### Command options
{: #secrets-manager-config-element-delete-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `public_cert`.

--config-element (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`.

--config-name (string)
:   The name of your configuration. Required.

### ibmcloud secrets-manager config-element
{: #secrets-manager-cli-config-element-command}

Retrieves the details of a specific configuration that is associated with a secret type.

```sh
ibmcloud secrets-manager config-element --secret-type SECRET-TYPE --config-element CONFIG-ELEMENT --config-name CONFIG-NAME 
```


#### Command options
{: #secrets-manager-config-element-cli-options}

--secret-type (string)
:   The secret type. Required.

    Allowable values are: `public_cert`.

--config-element (string)
:   The configuration element to define or manage. Required.

    Allowable values are: `certificate_authorities`, `dns_providers`.

--config-name (string)
:   The name of your configuration. Required.

## Schema examples
{: #secrets-manager-schema-examples}

The following schema examples represent the data that you need to specify for a command option. These examples model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment as appropriate.

### `CollectionMetadata`
{: #cli-collection-metadata-example-schema}

The following example shows the format of the `CollectionMetadata` object.

```json
{
  "collection_type" : "application/vnd.ibm.secrets-manager.secret.group+json",
  "collection_total" : 1
}
```
{: codeblock}

### `ConfigElementDefConfig`
{: #cli-config-element-def-config-example-schema}

The following example shows the format of the `ConfigElementDefConfig` object.

```json
{
  "private_key" : "exampleString"
}
```
{: codeblock}

### `SecretAction`
{: #cli-secret-action-example-schema}

The following example shows the format of the `SecretAction` object.

```json
{
  "payload" : "exampleString"
}
```
{: codeblock}

### `SecretGroupMetadataUpdatable[]`
{: #cli-secret-group-metadata-updatable-example-schema}

The following example shows the format of the `SecretGroupMetadataUpdatable[]` object.

```json
[ {
  "name" : "updated-secret-group-name",
  "description" : "Updated description for this group."
} ]
```
{: codeblock}

### `SecretGroupResource[]`
{: #cli-secret-group-resource-example-schema}

The following example shows the format of the `SecretGroupResource[]` object.

```json
[ {
  "name" : "my-secret-group",
  "description" : "Extended description for this group."
} ]
```
{: codeblock}

### `SecretMetadata[]`
{: #cli-secret-metadata-example-schema}

The following example shows the format of the `SecretMetadata[]` object.

```json
[ {
  "labels" : [ "dev", "us-south" ],
  "name" : "example-secret",
  "description" : "Extended description for this secret.",
  "expiration_date" : "2030-04-01T09:30:00.000Z"
} ]
```
{: codeblock}

### `SecretPolicyRotation[]`
{: #cli-secret-policy-rotation-example-schema}

The following example shows the format of the `SecretPolicyRotation[]` object.

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

### `SecretResource[]`
{: #cli-secret-resource-example-schema}

The following example shows the format of the `SecretResource[]` object.

```json
[ {
  "name" : "exampleString",
  "description" : "exampleString",
  "secret_group_id" : "exampleString",
  "labels" : [ "exampleString" ],
  "expiration_date" : "2030-04-01T09:30:00.000Z",
  "payload" : "exampleString"
} ]
```
{: codeblock}


