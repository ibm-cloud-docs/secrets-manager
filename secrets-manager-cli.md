---
 
copyright:
  years: 2021
lastupdated: "2021-02-05"

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

    ```
    export IBM_CLOUD_SECRETS_MANAGER_API_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
    ```
    {: pre}
  
    Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Retrieving your service endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#retrieve-service-endpoints)

    If you copy your service endpoint URL from the {{site.data.keyword.secrets-manager_short}} UI, be sure to trim `/api` from the URL before you export it as variable to use with the CLI plug-in.
    {: important}





## Config
{: #secrets-manager-config-cli}

### ibmcloud secrets-manager config-update
{: #secrets-manager-cli-config-update-command}

Updates the configuration for the given secret type.

```sh
ibmcloud secrets-manager config-update --secret-type SECRET-TYPE --engine-config-one-of ENGINE-CONFIG-ONE-OF 
```


#### Command options
{: #secrets-manager-config-update-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: iam_credentials</dd>
<dt>--engine-config-one-of</dt>
<dd> Required.</dd>
</dl>

### ibmcloud secrets-manager config
{: #secrets-manager-cli-config-command}

Retrieves the configuration that is associated with the given secret type.

```sh
ibmcloud secrets-manager config --secret-type SECRET-TYPE 
```


#### Command options
{: #secrets-manager-config-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: iam_credentials</dd>
</dl>

## Policies
{: #secrets-manager-policies-cli}

### ibmcloud secrets-manager policy-update
{: #secrets-manager-cli-policy-update-command}

Creates or updates one or more policies, such as an [automatic rotation policy](/docs/secrets-manager?topic=secrets-manager-rotate-secrets#auto-rotate-secret),  for the specified secret.

```sh
ibmcloud secrets-manager policy-update --secret-type SECRET-TYPE --id ID --metadata METADATA --resources RESOURCES [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-update-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: username_password</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
<dt>--metadata</dt>
<dd>The metadata that describes the resource array. Required.</dd>
<dt>--resources (array)</dt>
<dd>A collection of resources. Required.</dd>
<dt>--policy (string)</dt>
<dd>The type of policy that is associated with the specified secret.</dd>
<dd>Allowable values are: rotation</dd>
</dl>

### ibmcloud secrets-manager policy
{: #secrets-manager-cli-policy-command}

Retrieves a list of policies that are associated with a specified secret.

```sh
ibmcloud secrets-manager policy --secret-type SECRET-TYPE --id ID [--policy POLICY] 
```


#### Command options
{: #secrets-manager-policy-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: username_password</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
<dt>--policy (string)</dt>
<dd>The type of policy that is associated with the specified secret.</dd>
<dd>Allowable values are: rotation</dd>
</dl>

## Secret groups
{: #secrets-manager-secret-groups-cli}

### ibmcloud secrets-manager secret-group-create
{: #secrets-manager-cli-secret-group-create-command}

Creates a secret group that you can use to organize secrets and control who on your team has access to them.

A successful request returns the ID value of the secret group, along with other metadata. To learn more about  secret groups, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-groups).

```sh
ibmcloud secrets-manager secret-group-create --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-group-create-cli-options}

<dl> 
<dt>--metadata</dt>
<dd>The metadata that describes the resource array. Required.</dd>
<dt>--resources (array)</dt>
<dd>A collection of resources. Required.</dd>
</dl>

### ibmcloud secrets-manager secret-groups
{: #secrets-manager-cli-secret-groups-command}

Retrieves the list of secret groups that are available in your {{site.data.keyword.secrets-manager_short}} instance.

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

<dl> 
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret group. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
</dl>

### ibmcloud secrets-manager secret-group-metadata-update
{: #secrets-manager-cli-secret-group-metadata-update-command}

Updates the metadata of an existing secret group, such as its name or description.

```sh
ibmcloud secrets-manager secret-group-metadata-update --id ID --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-group-metadata-update-cli-options}

<dl> 
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret group. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
<dt>--metadata</dt>
<dd>The metadata that describes the resource array. Required.</dd>
<dt>--resources (array)</dt>
<dd>A collection of resources. Required.</dd>
</dl>

### ibmcloud secrets-manager secret-group-delete
{: #secrets-manager-cli-secret-group-delete-command}

Deletes a secret group by specifying the ID of the secret group.

**Note:** To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](#secrets-manager-cli-secret-delete-command) that are associated with the group.

```sh
ibmcloud secrets-manager secret-group-delete --id ID [--force]
```


#### Command options
{: #secrets-manager-secret-group-delete-cli-options}

<dl>
<dt>-f, --force</dt>
<dd>Force the operation without a confirmation.</dd> 
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret group. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
</dl>

## Secrets
{: #secrets-manager-secrets-cli}

### ibmcloud secrets-manager secret-create
{: #secrets-manager-cli-secret-create-command}

Creates a secret that you can use to access or authenticate to a protected resource.

A successful request stores the secret in your dedicated instance based on the secret type and data that you  specify. The response returns the ID value of the secret, along with other metadata.

To learn more about the types of secrets that you can create with {{site.data.keyword.secrets-manager_short}}, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-basics).

```sh
ibmcloud secrets-manager secret-create --secret-type SECRET-TYPE --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-create-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--metadata</dt>
<dd>The metadata that describes the resource array. Required.</dd>
<dt>--resources (array)</dt>
<dd>A collection of resources. Required.</dd>
</dl>

### ibmcloud secrets-manager secrets
{: #secrets-manager-cli-secrets-command}

Retrieves a list of secrets based on the type that you specify.

```sh
ibmcloud secrets-manager secrets --secret-type SECRET-TYPE [--limit LIMIT] [--offset OFFSET] 
```


#### Command options
{: #secrets-manager-secrets-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--limit (integer)</dt>
<dd>The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.</dd>
<dd>**Usage:** If you have 20 secrets in your instance, and you want to retrieve only the first 5 secrets, use
`../secrets/{secret-type}?limit=5`.</dd>
<dd>The maximum value is `5000`. The minimum value is `1`.</dd>
<dt>--offset (integer)</dt>
<dd>The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.</dd>
<dd>**Usage:** If you have 100 secrets in your instance, and you want to retrieve secrets 26 through 50, use
`../secrets/{secret-type}?offset=25&limit=25`.</dd>
<dd>The minimum value is `0`.</dd>
</dl>

### ibmcloud secrets-manager all-secrets
{: #secrets-manager-cli-all-secrets-command}

Retrieves a list of all secrets in your {{site.data.keyword.secrets-manager_short}} instance.

```sh
ibmcloud secrets-manager all-secrets [--limit LIMIT] [--offset OFFSET] 
```


#### Command options
{: #secrets-manager-all-secrets-cli-options}

<dl> 
<dt>--limit (integer)</dt>
<dd>The number of secrets to retrieve. By default, list operations return the first 200 items. To retrieve a different set of items, use `limit` with `offset` to page through your available resources.</dd>
<dd>**Usage:** If you have 20 secrets in your instance, and you want to retrieve only the first 5 secrets, use
`../secrets/{secret-type}?limit=5`.</dd>
<dd>The maximum value is `5000`. The minimum value is `1`.</dd>
<dt>--offset (integer)</dt>
<dd>The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.</dd>
<dd>**Usage:** If you have 100 secrets in your instance, and you want to retrieve secrets 26 through 50, use
`../secrets/{secret-type}?offset=25&limit=25`.</dd>
<dd>The minimum value is `0`.</dd>
</dl>

### ibmcloud secrets-manager secret
{: #secrets-manager-cli-secret-command}

Retrieves a secret and its details by specifying the ID of the secret.

A successful request returns the secret data that is associated with your secret, along with other metadata. To  view only the details of a specified secret without retrieving its value, use the [Get secret metadata](#secrets-manager-cli-secret-metadata-command)  method.

```sh
ibmcloud secrets-manager secret --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
</dl>

### ibmcloud secrets-manager secret-update
{: #secrets-manager-cli-secret-update-command}

Invokes an action on a specified secret. This method supports the following actions:

- `rotate`: Replace the value of an `arbitrary` or `username_password` secret.
- `delete_credentials`: Delete the API key that is associated with an `iam_credentials` secret.

```sh
ibmcloud secrets-manager secret-update --secret-type SECRET-TYPE --id ID --action ACTION --secret-action-one-of SECRET-ACTION-ONE-OF 
```


#### Command options
{: #secrets-manager-secret-update-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
<dt>--action (string)</dt>
<dd>The action to perform on the specified secret. Required.</dd>
<dd>Allowable values are: rotate, delete_credentials</dd>
<dt>--secret-action-one-of</dt>
<dd>The base request for invoking an action on a secret. Required.</dd>
</dl>

### ibmcloud secrets-manager secret-delete
{: #secrets-manager-cli-secret-delete-command}

Deletes a secret by specifying the ID of the secret.

```sh
ibmcloud secrets-manager secret-delete --secret-type SECRET-TYPE --id ID [--force]
```


#### Command options
{: #secrets-manager-secret-delete-cli-options}

<dl> 
<dt>-f, --force</dt>
<dd>Force the operation without a confirmation.</dd> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
</dl>

### ibmcloud secrets-manager secret-metadata
{: #secrets-manager-cli-secret-metadata-command}

Retrieves the details of a secret by specifying the ID.

A successful request returns only metadata about the secret, such as its name and creation date. To retrieve the  value of a secret, use the [Get a secret](#secrets-manager-cli-secret-command) method.

```sh
ibmcloud secrets-manager secret-metadata --secret-type SECRET-TYPE --id ID 
```


#### Command options
{: #secrets-manager-secret-metadata-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
</dl>

### ibmcloud secrets-manager secret-metadata-update
{: #secrets-manager-cli-secret-metadata-update-command}

Updates the metadata of a secret, such as its name or description.

To update the actual contents of a secret, rotate the secret by using the [Invoke an action on a secret](#secrets-manager-cli-secret-update-command) method.

```sh
ibmcloud secrets-manager secret-metadata-update --secret-type SECRET-TYPE --id ID --metadata METADATA --resources RESOURCES 
```


#### Command options
{: #secrets-manager-secret-metadata-update-cli-options}

<dl> 
<dt>--secret-type (string)</dt>
<dd>The secret type. Required.</dd>
<dd>Allowable values are: arbitrary, username_password, iam_credentials</dd>
<dt>--id (string)</dt>
<dd>The v4 UUID that uniquely identifies the secret. Required.</dd>
<dd>The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`</dd>
<dt>--metadata</dt>
<dd>The metadata that describes the resource array. Required.</dd>
<dt>--resources (array)</dt>
<dd>A collection of resources. Required.</dd>
</dl>