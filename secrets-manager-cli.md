---

copyright:
  years: 2023
lastupdated: "2023-06-01"

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


# {{site.data.keyword.secrets-manager_short}} CLI 
{: #secrets-manager-cli}

You can use the {{site.data.keyword.secrets-manager_full}} command-line interface (CLI) to manage secrets in your {{site.data.keyword.secrets-manager_short}} instance.
{: shortdesc}

Current version: **`2.0.1`**

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

* To target the {{site.data.keyword.secrets-manager_short}} instance, use one of the following options.

  * Run the `ibmcloud secrets-manager config set` command.

      ```sh
       ibmcloud secrets-manager config set service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
      ```
      {: pre}


  * Export an environment variable with your {{site.data.keyword.secrets-manager_short}} service endpoint URL.

      ```sh
      export SECRETS_MANAGER_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
      ```
      {: pre}

  * Set the service endpoint in the command.

      ```sh
      ibmcloud secrets-manager secrets --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
      ```
      {: pre}

    Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)




## Globals
### Options
{: #secrets-manager-global-options}

`--instance-id` (string)
:   The Secrets Manager Instance ID assigned by the service provider.

`--region` (string)
:   The region where you provisioned your Secrets Manager Instance. Available values: us-south, us-east, au-syd, jp-osa, jp-tok, eu-de, eu-gb, ca-tor, br-sao.

`--output` (string)
:   Choose an output format - can be 'json', 'yaml', or 'table'. Defaults to 'table'.

`-j`, `--jmes-query` (string)
:   Provide a JMESPath query to customize output.

`--service-url` (string)
:   Provide the base endpoint URL for the API.

`-q`, `--quiet`
:   Suppresses verbose messages.

#### Example
{: #secrets-manager-global-options-example}
```sh
ibmcloud secrets-manager
    --instance-id=provide-here-your-smgr-instanceuuid \
    --region=us-south \
    --output=json \
    --jmes-query="[:10]" \
    --service-url="https://myservice.test.cloud.ibm.com"
    --quiet
```
Note: This example only demonstrates the global options available to all sub-commands and is not a valid command itself.

### `ibmcloud secrets-manager config`
{: #secrets-manager-cli-config-command}

Global parameters can also be stored in persistent configuration so that they do not need to be manually specified each time the plugin is invoked. Each parameter can be configured with the `config` command and its subcommands.

```sh
ibmcloud secrets-manager config
```

### `ibmcloud secrets-manager config set`
{: #secrets-manager-cli-config-set-command}

Set a new config value for a specific option. Each subcommand of the `set` command maps to a global option. Each subcommand accepts a single argument, the string representation of the value to store for the option.

```sh
ibmcloud secrets-manager config set <option> <value>
```

#### Examples
{: #secrets-manager-config-set-command-examples}

```sh
ibmcloud secrets-manager config set service-url \
    'https://ibm.cloud.com/my-api'
```

### `ibmcloud secrets-manager config get`
{: #secrets-manager-cli-config-get-command}

Print out the currently set value for a specific option. Each subcommand of the `get` command maps to a global option.

```sh
ibmcloud secrets-manager config get <option>
```

#### Examples
{: #secrets-manager-config-get-command-examples}

```sh
ibmcloud secrets-manager config get service-url
```

### `ibmcloud secrets-manager config unset`
{: #secrets-manager-cli-config-unset-command}

Unset the currently set value for a specific option. Each subcommand of the `unset` command maps to a global option.

The subcommands available for this service are: `service-url`, .

```sh
ibmcloud secrets-manager config unset <option>
```

#### Examples
{: #secrets-manager-config-unset-command-examples}

```sh
ibmcloud secrets-manager config unset service-url
```

### `ibmcloud secrets-manager config list`
{: #secrets-manager-cli-config-list-command}

List out all of the currently set config values.

```sh
ibmcloud secrets-manager config list
```

#### Examples
{: #secrets-manager-config-list-command-examples}

```sh
ibmcloud secrets-manager config list
```
{: pre}


## Secret groups
{: #secrets-manager-secret-groups-cli}

Control who can access secrets by creating and managing groups.

### `ibmcloud secrets-manager secret-group-create`
{: #secrets-manager-cli-secret-group-create-command}

Create a secret group that you can use to organize secrets and control who can access them.

A successful request returns the ID value of the secret group, along with other properties. To learn more about secret groups, check out the [docs](/docs/secrets-manager?topic=secrets-manager-secret-groups).

```sh
ibmcloud secrets-manager secret-group-create --name NAME [--description DESCRIPTION]
```


#### Command options
{: #secrets-manager-secret-group-create-cli-options}

`--name` (string)
:   The name of your secret group. Required.

    The maximum length is `64` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--description` (string)
:   An extended description of your secret group.

To protect your privacy, do not use personal data, such as your name or location, as a description for your secret group.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

#### Example
{: #secrets-manager-secret-group-create-examples}

```sh
ibmcloud secrets-manager secret-group-create \
    --name=my-secret-group \
    --description='Extended description for this group.'
```
{: pre}

#### Example output
{: #secrets-manager-secret-group-create-cli-output}

Example of `SecretGroup` response

```json
{
  "created_at" : "2020-10-05T21:33:11Z",
  "description" : "Extended description for this group.",
  "id" : "d898bb90-82f6-4d61-b5cc-b079b66cfa76",
  "name" : "my-secret-group",
  "updated_at" : "2020-11-25T22:13:10Z",
  "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
}
```
{: screen}

### `ibmcloud secrets-manager secret-groups`
{: #secrets-manager-cli-secret-groups-command}

List the secret groups that are available in your Secrets Manager instance.

```sh
ibmcloud secrets-manager secret-groups
```


#### Example
{: #secrets-manager-secret-groups-examples}

```sh
ibmcloud secrets-manager secret-groups
```
{: pre}

#### Example output
{: #secrets-manager-secret-groups-cli-output}

Example `SecretGroup` collection response

```json
{
  "secret_groups" : [ {
    "created_at" : "2020-09-05T21:33:11Z",
    "description" : "Default Secret Group",
    "id" : "ee52ebb6-1728-4580-8ede-13f6504e3ae0",
    "name" : "default",
    "updated_at" : "2020-09-25T22:13:10Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
  }, {
    "created_at" : "2020-10-05T21:33:11Z",
    "description" : "Extended description for this group.",
    "id" : "cb52ebb6-1728-4580-8ede-13f6504e3ae0",
    "name" : "my-secret-group",
    "updated_at" : "2020-11-25T22:13:10Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
  }, {
    "created_at" : "2020-10-05T22:05:15Z",
    "description" : "Extended description for this group.",
    "id" : "19f88b9c-4f2f-405c-b877-a09338575c3f",
    "name" : "my-second-secret-group",
    "updated_at" : "2020-11-25T22:13:10Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
  } ],
  "total_count" : 3
}
```
{: screen}

### `ibmcloud secrets-manager secret-group`
{: #secrets-manager-cli-secret-group-command}

Get the properties of an existing secret group by specifying the ID of the group.

```sh
ibmcloud secrets-manager secret-group --id ID
```


#### Command options
{: #secrets-manager-secret-group-cli-options}

`--id` (string)
:   The ID of the secret group. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|default)$/`.

#### Example
{: #secrets-manager-secret-group-examples}

```sh
ibmcloud secrets-manager secret-group \
    --id=d898bb90-82f6-4d61-b5cc-b079b66cfa76
```
{: pre}

#### Example output
{: #secrets-manager-secret-group-cli-output}

Example of `SecretGroup` response

```json
{
  "created_at" : "2020-10-05T21:33:11Z",
  "description" : "Extended description for this group.",
  "id" : "d898bb90-82f6-4d61-b5cc-b079b66cfa76",
  "name" : "my-secret-group",
  "updated_at" : "2020-11-25T22:13:10Z",
  "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
}
```
{: screen}

### `ibmcloud secrets-manager secret-group-update`
{: #secrets-manager-cli-secret-group-update-command}

Update the properties of an existing secret group, such as its name or description.

```sh
ibmcloud secrets-manager secret-group-update --id ID [--name NAME] [--description DESCRIPTION]
```


#### Command options
{: #secrets-manager-secret-group-update-cli-options}

`--id` (string)
:   The ID of the secret group. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--secret-group-patch` (generic map)
:   The request body to update a secret group.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-group-patch=@path/to/file.json`.

`--name` (string)
:   The name of your secret group.

    The maximum length is `64` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--description` (string)
:   An extended description of your secret group.

To protect your privacy, do not use personal data, such as your name or location, as a description for your secret group.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

#### Example
{: #secrets-manager-secret-group-update-examples}

```sh
ibmcloud secrets-manager secret-group-update \
    --id=d898bb90-82f6-4d61-b5cc-b079b66cfa76 \
    --name=my-secret-group \
    --description='Extended description for this group.'
```
{: pre}

#### Example output
{: #secrets-manager-secret-group-update-cli-output}

Example of `SecretGroup` response

```json
{
  "created_at" : "2020-10-05T21:33:11Z",
  "description" : "Extended description for this group.",
  "id" : "d898bb90-82f6-4d61-b5cc-b079b66cfa76",
  "name" : "my-secret-group",
  "updated_at" : "2020-11-25T22:13:10Z",
  "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21"
}
```
{: screen}

### `ibmcloud secrets-manager secret-group-delete`
{: #secrets-manager-cli-secret-group-delete-command}

Delete a secret group by specifying the ID of the secret group.

**Note:** To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](#delete-secret) that are associated with the group.

```sh
ibmcloud secrets-manager secret-group-delete --id ID
```


#### Command options
{: #secrets-manager-secret-group-delete-cli-options}

`--id` (string)
:   The ID of the secret group. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Example
{: #secrets-manager-secret-group-delete-examples}

```sh
ibmcloud secrets-manager secret-group-delete \
    --id=d898bb90-82f6-4d61-b5cc-b079b66cfa76
```
{: pre}

## Secrets
{: #secrets-manager-secrets-cli}

Create, order, and manage different types of secrets for your apps and services.

### `ibmcloud secrets-manager secret-create`
{: #secrets-manager-cli-secret-create-command}

Create a secret or import an existing value that you can use to access or authenticate to a protected resource.

Use this operation to either generate or import an existing secret, such as a TLS certificate, that you can manage in your Secrets Manager service instance. A successful request stores the secret in your dedicated instance, based on the secret type and data that you specify. The response returns the ID value of the secret, along with other metadata.

To learn more about the types of secrets that you can create with Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

```sh
ibmcloud secrets-manager secret-create --secret-prototype SECRET-PROTOTYPE
```


#### Command options
{: #secrets-manager-secret-create-cli-options}

`--secret-prototype` ([`SecretPrototype`](#cli-secret-prototype-example-schema))
:   Specify the properties for your secret. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-prototype=@path/to/file.json`.

#### Example
{: #secrets-manager-secret-create-examples}

```sh
ibmcloud secrets-manager secret-create \
    --secret-prototype='{"custom_metadata": {"anyKey": "anyValue"}, "description": "Description of my arbitrary secret.", "expiration_date": "2023-10-05T11:49:42Z", "labels": ["dev","us-south"], "name": "example-arbitrary-secret", "secret_group_id": "default", "secret_type": "arbitrary", "payload": "secret-data", "version_custom_metadata": {"anyKey": "anyValue"}}'
```
{: pre}

### `ibmcloud secrets-manager secrets`
{: #secrets-manager-cli-secrets-command}

List the secrets that are available in your Secrets Manager instance.
Note: If the `--all-pages` option is not set, the command will only retrieve a single page of the collection.

```sh
ibmcloud secrets-manager secrets [--offset OFFSET] [--limit LIMIT] [--sort SORT] [--search SEARCH] [--groups GROUPS]
```


#### Command options
{: #secrets-manager-secrets-cli-options}

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

**Usage:** If you have 100 secrets in your instance, and you want to retrieve secrets 26 through 50, use
`..?offset=25&limit=25`.

    The minimum value is `0`.

`--limit` (int64)
:   The number of secrets to list.

    The maximum value is `1000`. The minimum value is `1`.

`--sort` (string)
:   Sort a collection of secrets by the specified field in ascending order. To sort in descending order use the `-` character


**Available values:** id | created_at | updated_at | expiration_date | secret_type | name

**Usage:** To sort a list of secrets by their creation date, use
`../secrets?sort=created_at`.

    The maximum length is `17` characters. The minimum length is `2` characters. The value must match regular expression `/^-?(id|created_at|updated_at|expiration_date|secret_type|name)$/`.

`--search` (string)
:   Filter secrets that contain the specified string in one or more of these fields: "id", "name", "description","labels", "secret_type".

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--groups` ([]string)
:   Filter secrets by groups.

    The list items must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|default)$/`. The maximum length is `201` items. The minimum length is `0` items.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for secrets.

#### Example
{: #secrets-manager-secrets-examples}

```sh
ibmcloud secrets-manager secrets \
    --offset=0 \
    --limit=10 \
    --sort=created_at \
    --search=example \
    --groups=default
```
{: pre}

#### Example output
{: #secrets-manager-secrets-cli-output}

Example secret metadata collection response

```json
{
  "first" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets?limit=50"
  },
  "previous" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets?offset=50&limit=50"
  },
  "last" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets?offset=200&limit=50"
  },
  "limit" : 50,
  "next" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets?offset=150&limit=50"
  },
  "offset" : 100,
  "secrets" : [ {
    "alt_names" : [ "s1.example.com", "*.s2.example.com" ],
    "common_name" : "example.com",
    "created_at" : "2020-10-05T21:33:11Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:a931192f-b6a9-43d6-a59a-834f3003af7b",
    "custom_metadata" : {
      "metadata_custom_key" : "metadata_custom_value"
    },
    "description" : "Extended description for this secret.",
    "downloaded" : false,
    "expiration_date" : "2023-10-05T11:49:42Z",
    "id" : "a931192f-b6a9-43d6-a59a-834f3003af7b",
    "intermediate_included" : true,
    "issuer" : "DigiCert",
    "key_algorithm" : "RSA2048",
    "labels" : [ "dev", "us-south" ],
    "locks_total" : 0,
    "name" : "my-imported-certificate",
    "private_key_included" : true,
    "secret_group_id" : "bfc0a4a9-3d58-4fda-945b-76756af516aa",
    "serial_number" : "03:e2:c6:e4:0b:7d:30:e2:e2:78:1b:b9:13:fd:f0:fc:89:dd",
    "secret_type" : "imported_cert",
    "signing_algorithm" : "SHA256-RSA",
    "state" : 1,
    "state_description" : "active",
    "updated_at" : "2022-10-05T21:33:11Z",
    "validity" : {
      "not_before" : "2020-10-05T21:33:11Z",
      "not_after" : "2023-10-05T11:49:42Z"
    },
    "versions_total" : 1
  }, {
    "alt_names" : [ "s1.example.com", "*.s2.example.com" ],
    "common_name" : "example.com",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "created_at" : "2020-10-05T21:33:11Z",
    "crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
    "custom_metadata" : {
      "metadata_custom_key" : "metadata_custom_value"
    },
    "description" : "Extended description for this secret.",
    "downloaded" : false,
    "expiration_date" : "2023-10-05T11:49:42Z",
    "id" : "f075f0b3-71e4-4a14-b60f-0b38b855a3d1",
    "issuer" : "Lets Encrypt",
    "issuance_info" : {
      "auto_rotated" : false,
      "ordered_on" : "2022-10-06T06:15:55Z",
      "state" : 1,
      "state_description" : "active"
    },
    "bundle_certs" : true,
    "ca" : "lets-encrypt-config",
    "dns" : "cloud-internet-services-config",
    "key_algorithm" : "RSA2048",
    "labels" : [ "dev", "us-south" ],
    "updated_at" : "2022-10-05T21:33:11Z",
    "locks_total" : 0,
    "name" : "my-public-certificate",
    "rotation" : {
      "auto_rotate" : true,
      "rotate_keys" : true
    },
    "secret_group_id" : "bfc0a4a9-3d58-4fda-945b-76756af516aa",
    "serial_number" : "03:e2:c6:e4:0b:7d:30:e2:e2:78:1b:b9:13:fd:f0:fc:89:dd",
    "secret_type" : "public_cert",
    "signing_algorithm" : "SHA256-RSA",
    "state" : 1,
    "state_description" : "active",
    "validity" : {
      "not_before" : "2020-10-05T21:33:11Z",
      "not_after" : "2023-10-05T11:49:42Z"
    },
    "versions_total" : 1
  } ],
  "total_count" : 232
}
```
{: screen}

### `ibmcloud secrets-manager secret`
{: #secrets-manager-cli-secret-command}

Get a secret and its details by specifying the ID of the secret.

A successful request returns the secret data that is associated with your secret, along with other metadata. To view only the details of a specified secret without retrieving its value, use the [Get secret metadata](#get-secret-metadata) operation.

```sh
ibmcloud secrets-manager secret --id ID
```


#### Command options
{: #secrets-manager-secret-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Example
{: #secrets-manager-secret-examples}

```sh
ibmcloud secrets-manager secret \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46
```
{: pre}

### `ibmcloud secrets-manager secret-delete`
{: #secrets-manager-cli-secret-delete-command}

Delete a secret by specifying the ID of the secret.

```sh
ibmcloud secrets-manager secret-delete --id ID
```


#### Command options
{: #secrets-manager-secret-delete-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Example
{: #secrets-manager-secret-delete-examples}

```sh
ibmcloud secrets-manager secret-delete \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46
```
{: pre}

### `ibmcloud secrets-manager secret-metadata`
{: #secrets-manager-cli-secret-metadata-command}

Get the metadata of a secret by specifying the ID of the secret.

```sh
ibmcloud secrets-manager secret-metadata --id ID
```


#### Command options
{: #secrets-manager-secret-metadata-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Example
{: #secrets-manager-secret-metadata-examples}

```sh
ibmcloud secrets-manager secret-metadata \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46
```
{: pre}

### `ibmcloud secrets-manager secret-metadata-update`
{: #secrets-manager-cli-secret-metadata-update-command}

Update the metadata of a secret, such as its name or description.

```sh
ibmcloud secrets-manager secret-metadata-update --id ID [--name NAME] [--description DESCRIPTION] [--labels LABELS] [--custom-metadata CUSTOM-METADATA] [--expiration-date EXPIRATION-DATE] [--ttl TTL] [--rotation ROTATION]
```


#### Command options
{: #secrets-manager-secret-metadata-update-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--secret-metadata-patch` (generic map)
:   JSON Merge-Patch content for update_secret_metadata.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-metadata-patch=@path/to/file.json`.

`--name` (string)
:   A human-readable name to assign to your secret.

To protect your privacy, do not use personal data, such as your name or location, as a name for your secret.

    The maximum length is `256` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--description` (string)
:   An extended description of your secret.

To protect your privacy, do not use personal data, such as your name or location, as a description for your secret group.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

`--labels` ([]string)
:   Labels that you can use to search secrets in your instance. Only 30 labels can be created.

Label can be between 2-30 characters, including spaces.

To protect your privacy, do not use personal data, such as your name or location, as a label for your secret.

    The list items must match regular expression `/(.*?)/`. The maximum length is `30` items. The minimum length is `0` items.

`--custom-metadata` (generic map)
:   The secret metadata that a user can customize.

`--expiration-date` (strfmt.DateTime)
:   The date when the secret material expires. The date format follows the `RFC 3339` format.

`--ttl` (string)
:   The time-to-live (TTL) or lease duration to assign to credentials that are generated.

For `iam_credentials` secrets, the TTL defines for how long each generated API key remains valid. The value can be either an integer that specifies the number of seconds, or the string representation of a duration, such as `120m` or `24h`.

The minimum duration is 1 minute. The maximum is 90 days.

    The maximum length is `10` characters. The minimum length is `2` characters. The value must match regular expression `/^[0-9]+[s,m,h,d]{0,1}$/`.

`--rotation` ([`RotationPolicy`](#cli-rotation-policy-example-schema))
:   This field indicates whether Secrets Manager rotates your secrets automatically.

#### Example
{: #secrets-manager-secret-metadata-update-examples}

```sh
ibmcloud secrets-manager secret-metadata-update \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --name=updated-arbitrary-secret-name \
    --description='updated Arbitrary Secret description' \
    --labels=dev,us-south \
    --custom-metadata='{"anyKey": "anyValue"}' \
    --expiration-date=2033-04-12T23:20:50.520Z
```
{: pre}

### `ibmcloud secrets-manager secret-action-create`
{: #secrets-manager-cli-secret-action-create-command}

Create a secret action. This operation supports the following actions:.

```sh
ibmcloud secrets-manager secret-action-create --id ID --secret-action-prototype SECRET-ACTION-PROTOTYPE
```


#### Command options
{: #secrets-manager-secret-action-create-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--secret-action-prototype` ([`SecretActionPrototype`](#cli-secret-action-prototype-example-schema))
:   The request body to specify the properties for your secret action. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-action-prototype=@path/to/file.json`.

#### Example
{: #secrets-manager-secret-action-create-examples}

```sh
ibmcloud secrets-manager secret-action-create \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --secret-action-prototype='{"action_type": "public_cert_action_validate_dns_challenge"}'
```
{: pre}

## Secret versions
{: #secrets-manager-secret-versions-cli}

Create and manage the versions of your secrets.

### `ibmcloud secrets-manager secret-version-create`
{: #secrets-manager-cli-secret-version-create-command}

Create a new secret version.

```sh
ibmcloud secrets-manager secret-version-create --secret-id SECRET-ID --secret-version-prototype SECRET-VERSION-PROTOTYPE
```


#### Command options
{: #secrets-manager-secret-version-create-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--secret-version-prototype` ([`SecretVersionPrototype`](#cli-secret-version-prototype-example-schema))
:   Specify the properties for your new secret version. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-version-prototype=@path/to/file.json`.

#### Example
{: #secrets-manager-secret-version-create-examples}

```sh
ibmcloud secrets-manager secret-version-create \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --secret-version-prototype='{"payload": "updated secret credentials", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'
```
{: pre}

### `ibmcloud secrets-manager secret-versions`
{: #secrets-manager-cli-secret-versions-command}

List the versions of a secret.

A successful request returns the list of versions of a secret, along with the metadata of each version.

```sh
ibmcloud secrets-manager secret-versions --secret-id SECRET-ID
```


#### Command options
{: #secrets-manager-secret-versions-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

#### Example
{: #secrets-manager-secret-versions-examples}

```sh
ibmcloud secrets-manager secret-versions \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46
```
{: pre}

#### Example output
{: #secrets-manager-secret-versions-cli-output}

Example secret version metadata collection response

```json
{
  "versions" : [ {
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "expiration_date" : "2023-10-05T11:49:42Z",
    "id" : "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
    "payload_available" : true,
    "secret_group_id" : "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
    "secret_id" : "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
    "secret_name" : "example-imported-certificate",
    "secret_type" : "imported_cert",
    "serial_number" : "38:eb:01:a3:22:e9:de:55:24:56:9b:14:cb:e2:f3:e3:e2:fb:f5:18",
    "validity" : {
      "not_after" : "2023-10-05T11:49:42Z",
      "not_before" : "2022-06-27T11:58:15Z"
    },
    "version_custom_metadata" : {
      "custom_version_key" : "custom_version_value"
    }
  } ],
  "total_count" : 1
}
```
{: screen}

### `ibmcloud secrets-manager secret-version`
{: #secrets-manager-cli-secret-version-command}

Get a version of a secret by specifying the ID of the version. You can use the `current` or `previous` aliases to refer to the current or previous secret version.

A successful request returns the secret data that is associated with the specified version of your secret, along with other metadata.

```sh
ibmcloud secrets-manager secret-version --secret-id SECRET-ID --id ID
```


#### Command options
{: #secrets-manager-secret-version-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

#### Example
{: #secrets-manager-secret-version-examples}

```sh
ibmcloud secrets-manager secret-version \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535
```
{: pre}

### `ibmcloud secrets-manager secret-version-data-delete`
{: #secrets-manager-cli-secret-version-data-delete-command}

Delete the data of a secret version by specifying the ID of the version.

This operation is available for secret type: iam_credentials current version.

```sh
ibmcloud secrets-manager secret-version-data-delete --secret-id SECRET-ID --id ID
```


#### Command options
{: #secrets-manager-secret-version-data-delete-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

#### Example
{: #secrets-manager-secret-version-data-delete-examples}

```sh
ibmcloud secrets-manager secret-version-data-delete \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535
```
{: pre}

### `ibmcloud secrets-manager secret-version-metadata`
{: #secrets-manager-cli-secret-version-metadata-command}

Get the metadata of a secret version by specifying the ID of the version. You can use the `current` or `previous` aliases to refer to the current or previous secret version.

A successful request returns the metadata that is associated with the specified version of your secret.

```sh
ibmcloud secrets-manager secret-version-metadata --secret-id SECRET-ID --id ID
```


#### Command options
{: #secrets-manager-secret-version-metadata-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

#### Example
{: #secrets-manager-secret-version-metadata-examples}

```sh
ibmcloud secrets-manager secret-version-metadata \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535
```
{: pre}

### `ibmcloud secrets-manager secret-version-metadata-update`
{: #secrets-manager-cli-secret-version-metadata-update-command}

Update the custom metadata of a secret version.

```sh
ibmcloud secrets-manager secret-version-metadata-update --secret-id SECRET-ID --id ID [--version-custom-metadata VERSION-CUSTOM-METADATA]
```


#### Command options
{: #secrets-manager-secret-version-metadata-update-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

`--secret-version-metadata-patch` (generic map)
:   JSON Merge-Patch content for update_secret_version_metadata.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-version-metadata-patch=@path/to/file.json`.

`--version-custom-metadata` (generic map)
:   The secret version metadata that a user can customize.

#### Example
{: #secrets-manager-secret-version-metadata-update-examples}

```sh
ibmcloud secrets-manager secret-version-metadata-update \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535 \
    --version-custom-metadata='{"anyKey": "anyValue"}'
```
{: pre}

### `ibmcloud secrets-manager secret-version-action-create`
{: #secrets-manager-cli-secret-version-action-create-command}

Create a secret version action. This operation supports the following actions:

- `private_cert_action_revoke_certificate`: Revoke a version of a private certificate.

```sh
ibmcloud secrets-manager secret-version-action-create --secret-id SECRET-ID --id ID --secret-version-action-prototype SECRET-VERSION-ACTION-PROTOTYPE
```


#### Command options
{: #secrets-manager-secret-version-action-create-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

`--secret-version-action-prototype` ([`SecretVersionActionPrototype`](#cli-secret-version-action-prototype-example-schema))
:   The request body to specify the properties of the action to create a secret version. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--secret-version-action-prototype=@path/to/file.json`.

#### Example
{: #secrets-manager-secret-version-action-create-examples}

```sh
ibmcloud secrets-manager secret-version-action-create \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535 \
    --secret-version-action-prototype='{"action_type": "private_cert_action_revoke_certificate"}'
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-action-create-cli-output}

The request body of the action to revoke private certificate versions.

```json
{
  "action_type" : "private_cert_action_revoke_certificate",
  "revocation_time_seconds" : 1667982994
}
```
{: screen}

## Secret locks
{: #secrets-manager-secret-locks-cli}

Secure your secret data by attaching a lock to a secret version and preventing any modification or deletion.

### `ibmcloud secrets-manager secrets-locks`
{: #secrets-manager-cli-secrets-locks-command}

List the secrets and their locks in your Secrets Manager instance.
Note: If the `--all-pages` option is not set, the command will only retrieve a single page of the collection.

```sh
ibmcloud secrets-manager secrets-locks [--offset OFFSET] [--limit LIMIT] [--search SEARCH] [--groups GROUPS]
```


#### Command options
{: #secrets-manager-secrets-locks-cli-options}

`--offset` (int64)
:   The number of secrets to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

**Usage:** If you have 100 secrets in your instance, and you want to retrieve secrets 26 through 50, use
`..?offset=25&limit=25`.

    The minimum value is `0`.

`--limit` (int64)
:   The number of secrets to list.

    The maximum value is `1000`. The minimum value is `1`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--groups` ([]string)
:   Filter secrets by groups.

    The list items must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|default)$/`. The maximum length is `201` items. The minimum length is `0` items.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for secrets-locks.

#### Example
{: #secrets-manager-secrets-locks-examples}

```sh
ibmcloud secrets-manager secrets-locks \
    --offset=0 \
    --limit=10 \
    --search=example \
    --groups=default
```
{: pre}

#### Example output
{: #secrets-manager-secrets-locks-cli-output}

Example response for listing all Secrets and locks

```json
{
  "secrets_locks" : [ {
    "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
    "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
    "secret_type" : "imported_cert",
    "secret_name" : "example-imported-certificate",
    "versions" : [ {
      "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
      "version_alias" : "current",
      "locks" : [ "lock-example-1", "lock-example-2" ]
    } ]
  } ],
  "first" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets_locks?limit=50"
  },
  "previous" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets_locks?offset=50&limit=50"
  },
  "last" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets_locks?offset=200&limit=50"
  },
  "limit" : 50,
  "next" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets_locks?offset=150&limit=50"
  },
  "offset" : 100,
  "total_count" : 1
}
```
{: screen}

### `ibmcloud secrets-manager secret-locks`
{: #secrets-manager-cli-secret-locks-command}

List the locks that are associated with a specified secret.
Note: If the `--all-pages` option is not set, the command will only retrieve a single page of the collection.

```sh
ibmcloud secrets-manager secret-locks --id ID [--offset OFFSET] [--limit LIMIT] [--sort SORT] [--search SEARCH]
```


#### Command options
{: #secrets-manager-secret-locks-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--offset` (int64)
:   The number of locks to skip.

    The minimum value is `0`.

`--limit` (int64)
:   The number of secrets with locks to list.

    The maximum value is `100`. The minimum value is `1`.

`--sort` (string)
:   Sort a collection of locks by the specified field in ascending order. To sort in descending order use the `-` character

**Available values:** created_at | updated_at | name

**Usage:** To sort a list of locks by their creation date, use
`../locks?sort=created_at`.

    The maximum length is `17` characters. The minimum length is `4` characters. The value must match regular expression `/^-?(created_at|updated_at|name)$/`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for secret-locks.

#### Example
{: #secrets-manager-secret-locks-examples}

```sh
ibmcloud secrets-manager secret-locks \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --offset=0 \
    --limit=10 \
    --sort=name \
    --search=example
```
{: pre}

#### Example output
{: #secrets-manager-secret-locks-cli-output}

Example of response body for listing secret locks

```json
{
  "first" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/locks?limit=50"
  },
  "previous" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/locks?offset=50&limit=50"
  },
  "limit" : 50,
  "last" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/locks?offset=200&limit=50"
  },
  "locks" : [ {
    "attributes" : {
      "key" : "value"
    },
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "description" : "Lock for consumer 1.",
    "name" : "lock-example-1",
    "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
    "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
    "secret_version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "secret_version_alias" : "current",
    "updated_at" : "2022-10-05T21:33:11Z"
  } ],
  "next" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/locks?offset=150&limit=50"
  },
  "offset" : 100,
  "total_count" : 1
}
```
{: screen}

### `ibmcloud secrets-manager secret-locks-bulk-create`
{: #secrets-manager-cli-secret-locks-bulk-create-command}

Create a lock on the current version of a secret.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it already exists. Additionally, you can use this operation to clear any matching locks on a secret by using one of the following optional lock modes:

- `remove_previous`: Removes any other locks with matching names if they are found in the previous version of the secret.\n
- `remove_previous_and_delete`: Carries out the same function as `remove_previous`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

```sh
ibmcloud secrets-manager secret-locks-bulk-create --id ID --locks LOCKS [--mode MODE]
```


#### Command options
{: #secrets-manager-secret-locks-bulk-create-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--locks` ([`SecretLockPrototype[]`](#cli-secret-lock-prototype-example-schema))
:   The locks data to be attached to a secret version. Required.

    The maximum length is `1000` items. The minimum length is `0` items.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--locks=@path/to/file.json`.

`--mode` (string)
:   Set the lock mode. Allowable values: `remove_previous`, and `remove_previous_and_delete`. Use `remove_previous` to create a lock that clears existing locks with matching names. Use `remove_previous_and_delete` to also delete data from the previous version.

    Allowable values are: `remove_previous`, `remove_previous_and_delete`.

#### Example
{: #secrets-manager-secret-locks-bulk-create-examples}

```sh
ibmcloud secrets-manager secret-locks-bulk-create \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --locks='[{"name": "lock-example-1", "description": "lock for consumer 1", "attributes": {"anyKey": "anyValue"}}]' \
    --mode=remove_previous
```
{: pre}

#### Example output
{: #secrets-manager-secret-locks-bulk-create-cli-output}

Example of response body to create secret locks

```json
{
  "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
  "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
  "versions" : [ {
    "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "version_alias" : "current",
    "locks" : [ "lock-3", "lock-4" ],
    "payload_available" : true
  }, {
    "version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "version_alias" : "previous",
    "locks" : [ "lock-example-1", "lock-example-2" ],
    "payload_available" : true
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-locks-bulk-delete`
{: #secrets-manager-cli-secret-locks-bulk-delete-command}

Delete all the locks or a subset of the locks that are associated with a version of a secret.

To delete only a subset of the locks, add a query param with a comma to separate the list of lock names:

Example: `?name=lock-example-1,lock-example-2`.

**Note:** A secret is considered unlocked and able to be deleted only after you remove all of its locks. To determine whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.

```sh
ibmcloud secrets-manager secret-locks-bulk-delete --id ID [--name NAME]
```


#### Command options
{: #secrets-manager-secret-locks-bulk-delete-cli-options}

`--id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--name` ([]string)
:   Specify the names of the secret locks to be deleted.

    The list items must match regular expression `/^[a-zA-Z]{1}[a-zA-Z0-9-_.]{1,63}$/`. The maximum length is `1000` items. The minimum length is `1` item.

#### Example
{: #secrets-manager-secret-locks-bulk-delete-examples}

```sh
ibmcloud secrets-manager secret-locks-bulk-delete \
    --id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --name=lock-example-1
```
{: pre}

#### Example output
{: #secrets-manager-secret-locks-bulk-delete-cli-output}

Example of response body to create secret locks

```json
{
  "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
  "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
  "versions" : [ {
    "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "version_alias" : "current",
    "locks" : [ "lock-3", "lock-4" ],
    "payload_available" : true
  }, {
    "version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "version_alias" : "previous",
    "locks" : [ "lock-example-1", "lock-example-2" ],
    "payload_available" : true
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-version-locks`
{: #secrets-manager-cli-secret-version-locks-command}

List the locks that are associated with a specified secret version.
Note: If the `--all-pages` option is not set, the command will only retrieve a single page of the collection.

```sh
ibmcloud secrets-manager secret-version-locks --secret-id SECRET-ID --id ID [--offset OFFSET] [--limit LIMIT] [--sort SORT] [--search SEARCH]
```


#### Command options
{: #secrets-manager-secret-version-locks-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

`--offset` (int64)
:   The number of locks to skip.

    The minimum value is `0`.

`--limit` (int64)
:   The number of secrets with locks to list.

    The maximum value is `100`. The minimum value is `1`.

`--sort` (string)
:   Sort a collection of locks by the specified field in ascending order. To sort in descending order use the `-` character

**Available values:** created_at | updated_at | name

**Usage:** To sort a list of locks by their creation date, use
`../locks?sort=created_at`.

    The maximum length is `17` characters. The minimum length is `4` characters. The value must match regular expression `/^-?(created_at|updated_at|name)$/`.

`--search` (string)
:   Filter locks that contain the specified string in the field "name".

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for secret-version-locks.

#### Example
{: #secrets-manager-secret-version-locks-examples}

```sh
ibmcloud secrets-manager secret-version-locks \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535 \
    --offset=0 \
    --limit=10 \
    --sort=name \
    --search=example
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-locks-cli-output}

Example response for listing secret version locks

```json
{
  "first" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/versions/7bf3814d-58f8-4df8-9cbd-f6860e4ca973/locks?limit=50"
  },
  "previous" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/versions/7bf3814d-58f8-4df8-9cbd-f6860e4ca973/locks?offset=50&limit=50"
  },
  "last" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/versions/7bf3814d-58f8-4df8-9cbd-f6860e4ca973/locks?offset=200&limit=50"
  },
  "limit" : 50,
  "locks" : [ {
    "attributes" : {
      "key" : "value"
    },
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "description" : "lock for consumer 1.",
    "name" : "lock-example-1",
    "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
    "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
    "secret_version_alias" : "current",
    "secret_version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "updated_at" : "2022-10-05T21:33:11Z"
  } ],
  "next" : {
    "href" : "https://us-south.secrets-maanger.cloud.ibm.com/88b75b20-aa21-4174-85c9-1feb3cc93c9a/api/v2/secrets/0cf4addb-7a90-410b-a3a7-a15bbe2b7909/versions/7bf3814d-58f8-4df8-9cbd-f6860e4ca973/locks?offset=150&limit=50"
  },
  "offset" : 100,
  "total_count" : 1
}
```
{: screen}

### `ibmcloud secrets-manager secret-version-locks-bulk-create`
{: #secrets-manager-cli-secret-version-locks-bulk-create-command}

Create a lock on the specified version of a secret.

A lock can be used to prevent a secret from being deleted or modified while it's in use by your applications. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it already exists. Additionally, you can use this operation to clear any matching locks on a secret by using one of the following optional lock modes:

- `remove_previous`: Removes any other locks with matching names if they are found in the previous version of the secret.
- `remove_previous_and_delete`: Carries out the same function as `remove_previous`, but also permanently deletes the data of the previous secret version if it doesn't have any locks.

```sh
ibmcloud secrets-manager secret-version-locks-bulk-create --secret-id SECRET-ID --id ID --locks LOCKS [--mode MODE]
```


#### Command options
{: #secrets-manager-secret-version-locks-bulk-create-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

`--locks` ([`SecretLockPrototype[]`](#cli-secret-lock-prototype-example-schema))
:   The locks data to be attached to a secret version. Required.

    The maximum length is `1000` items. The minimum length is `0` items.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--locks=@path/to/file.json`.

`--mode` (string)
:   Set the lock mode. Allowable values: `remove_previous`, and `remove_previous_and_delete`. Use `remove_previous` to create a lock that clears existing locks with matching names. Use `remove_previous_and_delete` to also delete data from the previous version.

    Allowable values are: `remove_previous`, `remove_previous_and_delete`.

#### Example
{: #secrets-manager-secret-version-locks-bulk-create-examples}

```sh
ibmcloud secrets-manager secret-version-locks-bulk-create \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535 \
    --locks='[{"name": "lock-example-1", "description": "lock for consumer 1", "attributes": {"anyKey": "anyValue"}}]' \
    --mode=remove_previous
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-locks-bulk-create-cli-output}

Example of response body to create secret locks

```json
{
  "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
  "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
  "versions" : [ {
    "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "version_alias" : "current",
    "locks" : [ "lock-3", "lock-4" ],
    "payload_available" : true
  }, {
    "version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "version_alias" : "previous",
    "locks" : [ "lock-example-1", "lock-example-2" ],
    "payload_available" : true
  } ]
}
```
{: screen}

### `ibmcloud secrets-manager secret-version-locks-bulk-delete`
{: #secrets-manager-cli-secret-version-locks-bulk-delete-command}

Delete all the locks or a subset of the locks that are associated with the specified version of a secret.

To delete only a subset of the locks, add a query param with a comma to separate the list of lock names:

Example: `?name=lock-example-1,lock-example-2`.

**Note:** A secret is considered unlocked and able to be deleted only after all of its locks are removed. To determine whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.

```sh
ibmcloud secrets-manager secret-version-locks-bulk-delete --secret-id SECRET-ID --id ID [--name NAME]
```


#### Command options
{: #secrets-manager-secret-version-locks-bulk-delete-cli-options}

`--secret-id` (string)
:   The ID of the secret. Required.

    The maximum length is `36` characters. The minimum length is `36` characters. The value must match regular expression `/[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}/`.

`--id` (string)
:   The ID of the secret version. Required.

    The maximum length is `36` characters. The minimum length is `7` characters. The value must match regular expression `/^([0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}|current|previous)$/`.

`--name` ([]string)
:   Specify the names of the secret locks to be deleted.

    The list items must match regular expression `/^[a-zA-Z]{1}[a-zA-Z0-9-_.]{1,63}$/`. The maximum length is `1000` items. The minimum length is `1` item.

#### Example
{: #secrets-manager-secret-version-locks-bulk-delete-examples}

```sh
ibmcloud secrets-manager secret-version-locks-bulk-delete \
    --secret-id=0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id=eb4cf24d-9cae-424b-945e-159788a5f535 \
    --name=lock-example-1
```
{: pre}

#### Example output
{: #secrets-manager-secret-version-locks-bulk-delete-cli-output}

Example of response body to create secret locks

```json
{
  "secret_id" : "0cf4addb-7a90-410b-a3a7-a15bbe2b7909",
  "secret_group_id" : "d8371728-95c8-4c12-b2af-1af98adb9e41",
  "versions" : [ {
    "version_id" : "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
    "version_alias" : "current",
    "locks" : [ "lock-3", "lock-4" ],
    "payload_available" : true
  }, {
    "version_id" : "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
    "version_alias" : "previous",
    "locks" : [ "lock-example-1", "lock-example-2" ],
    "payload_available" : true
  } ]
}
```
{: screen}

## Configurations
{: #secrets-manager-configurations-cli}

Create and manage configurations so that you can work with specific types of secrets.

### `ibmcloud secrets-manager configuration-create`
{: #secrets-manager-cli-configuration-create-command}

Add a configuration to the specified secret type.

Use this operation to define the configurations that are required to create public certificates (`public_cert`), private certificates (`private_cert`) and IAM Credentials secrets (`iam_credentials`).

You can add multiple configurations for your instance as follows:

- A single configuration for IAM Credentials.
- Up to 10 CA configurations for public certificates.
- Up to 10 DNS configurations for public certificates.
- Up to 10 Root CA configurations for private certificates.
- Up to 10 Intermediate CA configurations for private certificates.
- Up to 10 Certificate Template configurations for private certificates.

```sh
ibmcloud secrets-manager configuration-create --configuration-prototype CONFIGURATION-PROTOTYPE
```


#### Command options
{: #secrets-manager-configuration-create-cli-options}

`--configuration-prototype` ([`ConfigurationPrototype`](#cli-configuration-prototype-example-schema))
:   The details of your configuration. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--configuration-prototype=@path/to/file.json`.

#### Example
{: #secrets-manager-configuration-create-examples}

```sh
ibmcloud secrets-manager configuration-create \
    --configuration-prototype='{"config_type": "private_cert_configuration_root_ca", "name": "example-root-CA", "max_ttl": "43830h", "crl_expiry": "72h", "crl_disable": false, "crl_distribution_points_encoded": true, "issuing_certificates_urls_encoded": true, "common_name": "example.com", "alt_names": ["alt-name-1","alt-name-2"], "ip_sans": "127.0.0.1", "uri_sans": "https://www.example.com/test", "other_sans": ["1.2.3.5.4.3.201.10.4.3;utf8:test@example.com"], "ttl": "2190h", "format": "pem", "private_key_format": "der", "key_type": "rsa", "key_bits": 4096, "max_path_length": -1, "exclude_cn_from_sans": false, "permitted_dns_domains": ["exampleString"], "ou": ["exampleString"], "organization": ["exampleString"], "country": ["exampleString"], "locality": ["exampleString"], "province": ["exampleString"], "street_address": ["exampleString"], "postal_code": ["exampleString"], "serial_number": "d9:be:fe:35:ba:09:42:b5:35:ba:09:42:b5"}'
```
{: pre}

### `ibmcloud secrets-manager configurations`
{: #secrets-manager-cli-configurations-command}

List the configurations that are available in your Secrets Manager instance.
Note: If the `--all-pages` option is not set, the command will only retrieve a single page of the collection.

```sh
ibmcloud secrets-manager configurations [--offset OFFSET] [--limit LIMIT] [--sort SORT] [--search SEARCH]
```


#### Command options
{: #secrets-manager-configurations-cli-options}

`--offset` (int64)
:   The number of configurations to skip. By specifying `offset`, you retrieve a subset of items that starts with the `offset` value. Use `offset` with `limit` to page through your available resources.

**Usage:** If you have 100 configurations in your instance, and you want to retrieve configurations 26 through 50, use
`..?offset=25&limit=25`.

    The minimum value is `0`.

`--limit` (int64)
:   The number of configurations to list.

    The maximum value is `1000`. The minimum value is `1`.

`--sort` (string)
:   Sort a collection of configurations by the specified field in ascending order. To sort in descending order use the `-` character


**Available values:**  config_type | secret_type | name

**Usage:** To sort a list of configurations by their creation date, use
`../configurations?sort=config_type`.

    The maximum length is `17` characters. The minimum length is `2` characters. The value must match regular expression `/^-?(config_type|secret_type|name)$/`.

`--search` (string)
:   Filter configurations that contain the specified string in one or more of the fields: "name", "config_type", "secret_type".

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for configurations.

#### Example
{: #secrets-manager-configurations-examples}

```sh
ibmcloud secrets-manager configurations \
    --offset=0 \
    --limit=10 \
    --sort=config_type \
    --search=example
```
{: pre}

#### Example output
{: #secrets-manager-configurations-cli-output}

Example response for configuring metadata collection

```json
{
  "first" : {
    "href" : "https://something.com"
  },
  "next" : {
    "href" : "https://something.com"
  },
  "previous" : {
    "href" : "https://something.com"
  },
  "last" : {
    "href" : "https://something.com"
  },
  "limit" : 25,
  "offset" : 0,
  "configurations" : [ {
    "config_type" : "public_cert_configuration_ca_lets_encrypt",
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "lets_encrypt_environment" : "production",
    "name" : "lets-encrypt-config",
    "secret_type" : "public_cert",
    "updated_at" : "2022-10-05T21:33:11Z"
  }, {
    "config_type" : "public_cert_configuration_dns_cloud_internet_services",
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "name" : "cloud-internet-services-config",
    "secret_type" : "public_cert",
    "updated_at" : "2022-10-05T21:33:11Z"
  }, {
    "config_type" : "public_cert_configuration_dns_classic_infrastructure",
    "created_at" : "2022-06-27T11:58:15Z",
    "created_by" : "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
    "name" : "classic-infrastructure-config",
    "secret_type" : "public_cert",
    "updated_at" : "2022-10-05T21:33:11Z"
  }, {
    "common_name" : "ibm.com",
    "config_type" : "private_cert_configuration_root_ca",
    "created_at" : "2022-11-08T11:22:19Z",
    "created_by" : "iam-ServiceId-1ba95813-1b0f-45c1-b46c-969a5fda08d1",
    "expiration_date" : "2023-03-31T01:13:07Z",
    "key_bits" : 2048,
    "key_type" : "rsa",
    "name" : "internal-root",
    "secret_type" : "private_cert",
    "status" : "configured",
    "updated_at" : "2022-11-08T11:22:19Z"
  }, {
    "common_name" : "ibm.com",
    "config_type" : "private_cert_configuration_intermediate_ca",
    "created_at" : "2022-11-08T11:22:19Z",
    "created_by" : "iam-ServiceId-1ba95813-1b0f-45c1-b46c-969a5fda08d1",
    "expiration_date" : "2023-03-31T01:13:07Z",
    "issuer" : "internal-root",
    "name" : "example-intermediate-CA",
    "key_bits" : 2048,
    "key_type" : "rsa",
    "secret_type" : "private_cert",
    "status" : "configured",
    "updated_at" : "2022-11-08T11:22:19Z"
  }, {
    "certificate_authority" : "inter-1",
    "config_type" : "private_cert_configuration_template",
    "created_at" : "2022-11-08T11:22:23Z",
    "created_by" : "iam-ServiceId-8e54a866-476b-46cd-ba05-dbeae5f1d984",
    "name" : "example-certificate-template",
    "secret_type" : "private_cert",
    "updated_at" : "2022-11-08T11:22:23Z"
  } ],
  "total_count" : 6
}
```
{: screen}

### `ibmcloud secrets-manager configuration`
{: #secrets-manager-cli-configuration-command}

Get a configuration by specifying its name.

A successful request returns the details of your configuration.

```sh
ibmcloud secrets-manager configuration --name NAME [--x-sm-accept-configuration-type X-SM-ACCEPT-CONFIGURATION-TYPE]
```


#### Command options
{: #secrets-manager-configuration-cli-options}

`--name` (string)
:   The name of the configuration. Required.

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--x-sm-accept-configuration-type` (string)
:   The configuration type of this configuration - use this header to resolve 300 error responses.

    Allowable values are: `public_cert_configuration_ca_lets_encrypt`, `public_cert_configuration_dns_classic_infrastructure`, `public_cert_configuration_dns_cloud_internet_services`, `iam_credentials_configuration`, `private_cert_configuration_root_ca`, `private_cert_configuration_intermediate_ca`, `private_cert_configuration_template`.

#### Example
{: #secrets-manager-configuration-examples}

```sh
ibmcloud secrets-manager configuration \
    --name=configuration-name \
    --x-sm-accept-configuration-type=private_cert_configuration_root_ca
```
{: pre}

### `ibmcloud secrets-manager configuration-update`
{: #secrets-manager-cli-configuration-update-command}

Update a configuration.

```sh
ibmcloud secrets-manager configuration-update --name NAME [--api-key API-KEY] [--lets-encrypt-environment LETS-ENCRYPT-ENVIRONMENT] [--lets-encrypt-private-key LETS-ENCRYPT-PRIVATE-KEY] [--lets-encrypt-preferred-chain LETS-ENCRYPT-PREFERRED-CHAIN] [--cloud-internet-services-apikey CLOUD-INTERNET-SERVICES-APIKEY] [--cloud-internet-services-crn CLOUD-INTERNET-SERVICES-CRN] [--classic-infrastructure-username CLASSIC-INFRASTRUCTURE-USERNAME] [--classic-infrastructure-password CLASSIC-INFRASTRUCTURE-PASSWORD] [--max-ttl MAX-TTL] [--crl-expiry CRL-EXPIRY] [--crl-disable CRL-DISABLE] [--crl-distribution-points-encoded CRL-DISTRIBUTION-POINTS-ENCODED] [--issuing-certificates-urls-encoded ISSUING-CERTIFICATES-URLS-ENCODED] [--allowed-secret-groups ALLOWED-SECRET-GROUPS] [--ttl TTL] [--allow-localhost ALLOW-LOCALHOST] [--allowed-domains ALLOWED-DOMAINS] [--allowed-domains-template ALLOWED-DOMAINS-TEMPLATE] [--allow-bare-domains ALLOW-BARE-DOMAINS] [--allow-subdomains ALLOW-SUBDOMAINS] [--allow-glob-domains ALLOW-GLOB-DOMAINS] [--allow-any-name ALLOW-ANY-NAME] [--enforce-hostnames ENFORCE-HOSTNAMES] [--allow-ip-sans ALLOW-IP-SANS] [--allowed-uri-sans ALLOWED-URI-SANS] [--allowed-other-sans ALLOWED-OTHER-SANS] [--server-flag SERVER-FLAG] [--client-flag CLIENT-FLAG] [--code-signing-flag CODE-SIGNING-FLAG] [--email-protection-flag EMAIL-PROTECTION-FLAG] [--key-type KEY-TYPE] [--key-bits KEY-BITS] [--key-usage KEY-USAGE] [--ext-key-usage EXT-KEY-USAGE] [--ext-key-usage-oids EXT-KEY-USAGE-OIDS] [--use-csr-common-name USE-CSR-COMMON-NAME] [--use-csr-sans USE-CSR-SANS] [--ou OU] [--organization ORGANIZATION] [--country COUNTRY] [--locality LOCALITY] [--province PROVINCE] [--street-address STREET-ADDRESS] [--postal-code POSTAL-CODE] [--serial-number SERIAL-NUMBER] [--require-cn REQUIRE-CN] [--policy-identifiers POLICY-IDENTIFIERS] [--basic-constraints-valid-for-non-ca BASIC-CONSTRAINTS-VALID-FOR-NON-CA] [--not-before-duration NOT-BEFORE-DURATION] [--x-sm-accept-configuration-type X-SM-ACCEPT-CONFIGURATION-TYPE]
```


#### Command options
{: #secrets-manager-configuration-update-cli-options}

`--name` (string)
:   The name of the configuration. Required.

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--configuration-patch` (generic map)
:   JSON Merge-Patch content for update_configuration.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--configuration-patch=@path/to/file.json`.

`--x-sm-accept-configuration-type` (string)
:   The configuration type of this configuration - use this header to resolve 300 error responses.

    Allowable values are: `public_cert_configuration_ca_lets_encrypt`, `public_cert_configuration_dns_classic_infrastructure`, `public_cert_configuration_dns_cloud_internet_services`, `iam_credentials_configuration`, `private_cert_configuration_root_ca`, `private_cert_configuration_intermediate_ca`, `private_cert_configuration_template`.

`--api-key` (string)
:   The API key that is generated for this secret.

After the secret reaches the end of its lease (see the `ttl` field), the API key is deleted automatically. If you want to continue to use the same API key for future read operations, see the `reuse_api_key` field.

    The maximum length is `60` characters. The minimum length is `5` characters. The value must match regular expression `/^(?:[A-Za-z0-9_\\-]{4})*(?:[A-Za-z0-9_\\-]{2}==|[A-Za-z0-9_\\-]{3}=)?$/`.

`--lets-encrypt-environment` (string)
:   The configuration of the Let's Encrypt CA environment.

    Allowable values are: `production`, `staging`.

`--lets-encrypt-private-key` (string)
:   The PEM-encoded private key of your Let's Encrypt account.

    The maximum length is `100000` characters. The minimum length is `50` characters. The value must match regular expression `/(^-----BEGIN PRIVATE KEY-----.*?)|(^-----BEGIN RSA PRIVATE KEY-----.*?)/`.

`--lets-encrypt-preferred-chain` (string)
:   The preferred chain with an issuer that matches this Subject Common Name.

    The maximum length is `30` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--cloud-internet-services-apikey` (string)
:   An IBM Cloud API key that can to list domains in your Cloud Internet Services instance.

To grant Secrets Manager the ability to view the Cloud Internet Services instance and all of its domains, the API key must be assigned the Reader service role on Internet Services (`internet-svcs`).

If you need to manage specific domains, you can assign the Manager role. For production environments, it is recommended that you assign the Reader access role, and then use the
[IAM Policy Management API](https://cloud.ibm.com/apidocs/iam-policy-management#create-policy) to control specific domains. For more information, see the [docs](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-specific-domains).

    The maximum length is `128` characters. The minimum length is `4` characters. The value must match regular expression `/(.*?)/`.

`--cloud-internet-services-crn` (string)
:   A CRN that uniquely identifies an IBM Cloud resource.

    The maximum length is `512` characters. The minimum length is `9` characters. The value must match regular expression `/^crn:v[0-9](:([A-Za-z0-9-._~!$&'()*+,;=@\/]|%[0-9A-Z]{2})*){8}$/`.

`--classic-infrastructure-username` (string)
:   The username that is associated with your classic infrastructure account.

In most cases, your classic infrastructure username is your `<account_id>_<email_address>`. For more information, see the [docs](/docs/account?topic=account-classic_keys).

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--classic-infrastructure-password` (string)
:   Your classic infrastructure API key.

For information about viewing and accessing your classic infrastructure API key, see the [docs](/docs/account?topic=account-classic_keys).

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--max-ttl` (string)
:   The maximum time-to-live (TTL) for certificates that are created by this CA.

The value can be supplied as a string representation of a duration in hours, for example '8760h'. In the API response, this value is returned in seconds (integer).

Minimum value is one hour (`1h`). Maximum value is 100 years (`876000h`).

    The maximum length is `10` characters. The minimum length is `2` characters. The value must match regular expression `/^[0-9]+[s,m,h,d]{0,1}$/`.

`--crl-expiry` (string)
:   The time until the certificate revocation list (CRL) expires.

The value can be supplied as a string representation of a duration in hours, such as `48h`. The default is 72 hours. In the API response, this value is returned in seconds (integer).

**Note:** The CRL is rotated automatically before it expires.

    The maximum length is `10` characters. The minimum length is `2` characters. The value must match regular expression `/^[0-9]+[s,m,h,d]{0,1}$/`.

`--crl-disable` (bool)
:   This field disables or enables certificate revocation list (CRL) building.

If CRL building is disabled, a signed but zero-length CRL is returned when you're downloading the CRL. If CRL building is enabled, it rebuilds the CRL.

`--crl-distribution-points-encoded` (bool)
:   This field determines whether to encode the certificate revocation list (CRL) distribution points in the certificates that are issued by this certificate authority.

`--issuing-certificates-urls-encoded` (bool)
:   This field determines whether to encode the URL of the issuing certificate in the certificates that are issued by this certificate authority.

`--allowed-secret-groups` (string)
:   This field scopes the creation of private certificates to only the secret groups that you specify.

This field can be supplied as a comma-delimited list of secret group IDs.

    The maximum length is `1024` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--ttl` (string)
:   The requested time-to-live (TTL) for certificates that are created by this CA. This field's value can't be longer than the `max_ttl` limit.

The value can be supplied as a string representation of a duration in hours, for example '8760h'. In the API response, this value is returned in seconds (integer).

    The maximum length is `10` characters. The minimum length is `2` characters. The value must match regular expression `/^[0-9]+[s,m,h,d]{0,1}$/`.

`--allow-localhost` (bool)
:   This field indicates whether to allow `localhost` to be included as one of the requested common names.

`--allowed-domains` ([]string)
:   The domains to define for the certificate template. This property is used along with the `allow_bare_domains` and `allow_subdomains` options.

    The list items must match regular expression `/(.*?)/`. The maximum length is `100` items. The minimum length is `0` items.

`--allowed-domains-template` (bool)
:   This field indicates whether to allow the domains that are supplied in the `allowed_domains` field to contain access control list (ACL) templates.

`--allow-bare-domains` (bool)
:   This field indicates whether to allow clients to request private certificates that match the value of the actual domains on the final certificate.

For example, if you specify `example.com` in the `allowed_domains` field, you grant clients the ability to request a certificate that contains the name `example.com` as one of the DNS values on the final certificate.

**Important:** In some scenarios, allowing bare domains can be considered a security risk.

`--allow-subdomains` (bool)
:   This field indicates whether to allow clients to request private certificates with common names (CN) that are subdomains of the CNs that are allowed by the other certificate template options. This includes wildcard subdomains.

For example, if `allowed_domains` has a value of `example.com` and `allow_subdomains`is set to `true`, then the following subdomains are allowed: `foo.example.com`, `bar.example.com`, `*.example.com`.

**Note:** This field is redundant if you use the `allow_any_name` option.

`--allow-glob-domains` (bool)
:   This field indicates whether to allow glob patterns, for example, `ftp*.example.com`, in the names that are specified in the `allowed_domains` field.

If set to `true`, clients are allowed to request private certificates with names that match the glob patterns.

`--allow-any-name` (bool)
:   This field indicates whether to allow clients to request a private certificate that matches any common name.

`--enforce-hostnames` (bool)
:   This field indicates whether to enforce only valid hostnames for common names, DNS Subject Alternative Names, and the host section of email addresses.

`--allow-ip-sans` (bool)
:   This field indicates whether to allow clients to request a private certificate with IP Subject Alternative Names.

`--allowed-uri-sans` ([]string)
:   The URI Subject Alternative Names to allow for private certificates.

Values can contain glob patterns, for example `spiffe://hostname/_*`.

    The list items must match regular expression `/(.*?)/`. The maximum length is `100` items. The minimum length is `0` items.

`--allowed-other-sans` ([]string)
:   The custom Object Identifier (OID) or UTF8-string Subject Alternative Names (SANs) to allow for private certificates.

The format for each element in the list is the same as OpenSSL: `<oid>:<type>:<value>` where the current valid type is `UTF8`. To allow any value for an OID, use `*` as its value. Alternatively, specify a single `*` to allow any `other_sans` input.

    The list items must match regular expression `/(.*?)/`. The maximum length is `100` items. The minimum length is `0` items.

`--server-flag` (bool)
:   This field indicates whether private certificates are flagged for server use.

`--client-flag` (bool)
:   This field indicates whether private certificates are flagged for client use.

`--code-signing-flag` (bool)
:   This field indicates whether private certificates are flagged for code signing use.

`--email-protection-flag` (bool)
:   This field indicates whether private certificates are flagged for email protection use.

`--key-type` (string)
:   The type of private key to generate.

    Allowable values are: `rsa`, `ec`.

`--key-bits` (int64)
:   The number of bits to use to generate the private key.

Allowable values for RSA keys are: `2048` and `4096`. Allowable values for EC keys are: `224`, `256`, `384`, and `521`. The default for RSA keys is `2048`. The default for EC keys is `256`.

`--key-usage` ([]string)
:   The allowed key usage constraint to define for private certificates.

You can find valid values in the [Go x509 package documentation](https://pkg.go.dev/crypto/x509#KeyUsage). Omit the `KeyUsage` part of the value. Values are not case-sensitive. To specify no key usage constraints, set this field to an empty list.

    The list items must match regular expression `/^[a-zA-Z]+$/`. The maximum length is `100` items. The minimum length is `0` items.

`--ext-key-usage` ([]string)
:   The allowed extended key usage constraint on private certificates.

You can find valid values in the [Go x509 package documentation](https://golang.org/pkg/crypto/x509/#ExtKeyUsage). Omit the `ExtKeyUsage` part of the value. Values are not case-sensitive. To specify no key usage constraints, set this field to an empty list.

    The list items must match regular expression `/^[a-zA-Z]+$/`. The maximum length is `100` items. The minimum length is `0` items.

`--ext-key-usage-oids` ([]string)
:   A list of extended key usage Object Identifiers (OIDs).

    The list items must match regular expression `/(.*?)/`. The maximum length is `100` items. The minimum length is `0` items.

`--use-csr-common-name` (bool)
:   When used with the `private_cert_configuration_action_sign_csr` action, this field determines whether to use the common name (CN) from a certificate signing request (CSR) instead of the CN that is included in the data of the certificate.

Does not include any requested Subject Alternative Names (SANs) in the CSR. To use the alternative names, include the `use_csr_sans` property.

`--use-csr-sans` (bool)
:   When used with the `private_cert_configuration_action_sign_csr` action, this field determines whether to use the Subject Alternative Names
(SANs) from a certificate signing request (CSR) instead of the SANs that are included in the data of the certificate.

This field does not include the common name in the CSR. To use the common name, include the `use_csr_common_name` property.

`--ou` ([]string)
:   The Organizational Unit (OU) values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--organization` ([]string)
:   The Organization (O) values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--country` ([]string)
:   The Country (C) values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--locality` ([]string)
:   The Locality (L) values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--province` ([]string)
:   The Province (ST) values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--street-address` ([]string)
:   The street address values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--postal-code` ([]string)
:   The postal code values to define in the subject field of the resulting certificate.

    The list items must match regular expression `/(.*?)/`. The maximum length is `10` items. The minimum length is `0` items.

`--serial-number` (string)
:   The serial number to assign to the generated certificate. To assign a random serial number, you can omit this field.

    The maximum length is `64` characters. The minimum length is `32` characters. The value must match regular expression `/[^a-fA-F0-9]/`.

`--require-cn` (bool)
:   This field indicates whether to require a common name to create a private certificate.

By default, a common name is required to generate a certificate. To make the `common_name` field optional, set the `require_cn` option to `false`.

`--policy-identifiers` ([]string)
:   A list of policy Object Identifiers (OIDs).

    The list items must match regular expression `/(.*?)/`. The maximum length is `100` items. The minimum length is `0` items.

`--basic-constraints-valid-for-non-ca` (bool)
:   This field indicates whether to mark the Basic Constraints extension of an issued private certificate as valid for non-CA certificates.

`--not-before-duration` (string)
:   The duration in seconds by which to backdate the `not_before` property of an issued private certificate.

The value can be supplied as a string representation of a duration, such as `30s`. In the API response, this value is returned in seconds (integer).

    The maximum length is `10` characters. The minimum length is `2` characters. The value must match regular expression `/^[0-9]+[s,m,h,d]{0,1}$/`.

#### Example
{: #secrets-manager-configuration-update-examples}

```sh
ibmcloud secrets-manager configuration-update \
    --name=configuration-name \
    --x-sm-accept-configuration-type=private_cert_configuration_root_ca \
    --api-key=RmnPBn6n1dzoo0v3kyznKEpg0WzdTpW9lW7FtKa017_u
```
{: pre}

### `ibmcloud secrets-manager configuration-delete`
{: #secrets-manager-cli-configuration-delete-command}

Delete a configuration by specifying its name.

```sh
ibmcloud secrets-manager configuration-delete --name NAME [--x-sm-accept-configuration-type X-SM-ACCEPT-CONFIGURATION-TYPE]
```


#### Command options
{: #secrets-manager-configuration-delete-cli-options}

`--name` (string)
:   The name of the configuration. Required.

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--x-sm-accept-configuration-type` (string)
:   The configuration type of this configuration - use this header to resolve 300 error responses.

    Allowable values are: `public_cert_configuration_ca_lets_encrypt`, `public_cert_configuration_dns_classic_infrastructure`, `public_cert_configuration_dns_cloud_internet_services`, `iam_credentials_configuration`, `private_cert_configuration_root_ca`, `private_cert_configuration_intermediate_ca`, `private_cert_configuration_template`.

#### Example
{: #secrets-manager-configuration-delete-examples}

```sh
ibmcloud secrets-manager configuration-delete \
    --name=configuration-name \
    --x-sm-accept-configuration-type=private_cert_configuration_root_ca
```
{: pre}

### `ibmcloud secrets-manager configuration-action-create`
{: #secrets-manager-cli-configuration-action-create-command}

Create a configuration action. This operation supports the following actions:

- `private_cert_configuration_action_sign_intermediate`: Sign an intermediate certificate authority.
- `private_cert_configuration_action_sign_csr`: Sign a certificate signing request.
- `private_cert_configuration_action_set_signed`: Set a signed intermediate certificate authority.
- `private_cert_configuration_action_revoke_ca_certificate`: Revoke an internally signed intermediate certificate authority certificate.
- `private_cert_configuration_action_rotate_crl`: Rotate the certificate revocation list (CRL) of an intermediate certificate authority.

```sh
ibmcloud secrets-manager configuration-action-create --name NAME --config-action-prototype CONFIG-ACTION-PROTOTYPE [--x-sm-accept-configuration-type X-SM-ACCEPT-CONFIGURATION-TYPE]
```


#### Command options
{: #secrets-manager-configuration-action-create-cli-options}

`--name` (string)
:   The name of the configuration. Required.

    The maximum length is `128` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9][A-Za-z0-9]*(?:_*-*\\.*[A-Za-z0-9]+)*$/`.

`--config-action-prototype` ([`ConfigurationActionPrototype`](#cli-configuration-action-prototype-example-schema))
:   The request body to specify the properties of the action to create a configuration. Required.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, e.g. `--config-action-prototype=@path/to/file.json`.

`--x-sm-accept-configuration-type` (string)
:   The configuration type of this configuration - use this header to resolve 300 error responses.

    Allowable values are: `public_cert_configuration_ca_lets_encrypt`, `public_cert_configuration_dns_classic_infrastructure`, `public_cert_configuration_dns_cloud_internet_services`, `iam_credentials_configuration`, `private_cert_configuration_root_ca`, `private_cert_configuration_intermediate_ca`, `private_cert_configuration_template`.

#### Example
{: #secrets-manager-configuration-action-create-examples}

```sh
ibmcloud secrets-manager configuration-action-create \
    --name=configuration-name \
    --config-action-prototype='{"action_type": "private_cert_configuration_action_rotate_crl"}' \
    --x-sm-accept-configuration-type=private_cert_configuration_root_ca
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
:   A CRN that uniquely identifies an IBM Cloud resource. Required.

    The maximum length is `512` characters. The minimum length is `9` characters. The value must match regular expression `/^crn:v[0-9](:([A-Za-z0-9-._~!$&'()*+,;=@\/]|%[0-9A-Z]{2})*){8}$/`.

`--event-notifications-source-name` (string)
:   The name that is displayed as a source that is in your Event Notifications instance. Required.

    The maximum length is `256` characters. The minimum length is `2` characters. The value must match regular expression `/(.*?)/`.

`--event-notifications-source-description` (string)
:   An optional description for the source that is in your Event Notifications instance.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

#### Example
{: #secrets-manager-notifications-registration-create-examples}

```sh
ibmcloud secrets-manager notifications-registration-create \
    --event-notifications-instance-crn=crn:v1:bluemix:public:event-notifications:us-south:a/22018f3c34ff4ff193698d15ca316946:578ad1a4-2fd8-4e66-95d5-79a842ba91f8:: \
    --event-notifications-source-name='My Secrets Manager' \
    --event-notifications-source-description='Optional description of this source in an Event Notifications instance.'
```
{: pre}

#### Example output
{: #secrets-manager-notifications-registration-create-cli-output}

Example Notifications registration configuration details for Event Notifications.

```json
{
  "event_notifications_instance_crn" : "crn:v1:bluemix:public:event-notifications:us-south:a/22018f3c34ff4ff193698d15ca316946:578ad1a4-2fd8-4e66-95d5-79a842ba91f8::"
}
```
{: screen}

### `ibmcloud secrets-manager notifications-registration`
{: #secrets-manager-cli-notifications-registration-command}

Get the details of the registration between your Secrets Manager instance and Event Notifications.

```sh
ibmcloud secrets-manager notifications-registration
```


#### Example
{: #secrets-manager-notifications-registration-examples}

```sh
ibmcloud secrets-manager notifications-registration
```
{: pre}

#### Example output
{: #secrets-manager-notifications-registration-cli-output}

Example Notifications registration configuration details for Event Notifications.

```json
{
  "event_notifications_instance_crn" : "crn:v1:bluemix:public:event-notifications:us-south:a/22018f3c34ff4ff193698d15ca316946:578ad1a4-2fd8-4e66-95d5-79a842ba91f8::"
}
```
{: screen}

### `ibmcloud secrets-manager notifications-registration-delete`
{: #secrets-manager-cli-notifications-registration-delete-command}

Delete the registration between your Secrets Manager instance and Event Notifications.

A successful request removes your Secrets Manager instance as a source in Event Notifications.

```sh
ibmcloud secrets-manager notifications-registration-delete
```


#### Example
{: #secrets-manager-notifications-registration-delete-examples}

```sh
ibmcloud secrets-manager notifications-registration-delete
```
{: pre}

### `ibmcloud secrets-manager notifications-registration-test`
{: #secrets-manager-cli-notifications-registration-test-command}

Send a test event from a Secrets Manager instance to a configured [Event Notifications](https://cloud.ibm.com/apidocs/event-notifications) instance.

A successful request sends a test event to the Event Notifications instance. For more information about enabling notifications for Secrets Manager, check out the [docs](/docs/secrets-manager?topic=secrets-manager-event-notifications).

```sh
ibmcloud secrets-manager notifications-registration-test
```


#### Example
{: #secrets-manager-notifications-registration-test-examples}

```sh
ibmcloud secrets-manager notifications-registration-test
```
{: pre}

## Schema examples
{: #secrets-manager-schema-examples}

The following schema examples represent the data that you need to specify for a command option. These examples model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment as appropriate.

### ConfigurationActionPrototype
{: #cli-configuration-action-prototype-example-schema}

The following example shows the format of the ConfigurationActionPrototype object.

```json

{
  "action_type" : "private_cert_configuration_action_rotate_crl"
}
```
{: codeblock}

### ConfigurationPrototype
{: #cli-configuration-prototype-example-schema}

The following example shows the format of the ConfigurationPrototype object.

```json

{
  "config_type" : "private_cert_configuration_root_ca",
  "name" : "example-root-CA",
  "max_ttl" : "43830h",
  "crl_expiry" : "72h",
  "crl_disable" : false,
  "crl_distribution_points_encoded" : true,
  "issuing_certificates_urls_encoded" : true,
  "common_name" : "example.com",
  "alt_names" : [ "alt-name-1", "alt-name-2" ],
  "ip_sans" : "127.0.0.1",
  "uri_sans" : "https://www.example.com/test",
  "other_sans" : [ "1.2.3.5.4.3.201.10.4.3;utf8:test@example.com" ],
  "ttl" : "2190h",
  "format" : "pem",
  "private_key_format" : "der",
  "key_type" : "rsa",
  "key_bits" : 4096,
  "max_path_length" : -1,
  "exclude_cn_from_sans" : false,
  "permitted_dns_domains" : [ "exampleString" ],
  "ou" : [ "exampleString" ],
  "organization" : [ "exampleString" ],
  "country" : [ "exampleString" ],
  "locality" : [ "exampleString" ],
  "province" : [ "exampleString" ],
  "street_address" : [ "exampleString" ],
  "postal_code" : [ "exampleString" ],
  "serial_number" : "d9:be:fe:35:ba:09:42:b5:35:ba:09:42:b5"
}
```
{: codeblock}

### SecretActionPrototype
{: #cli-secret-action-prototype-example-schema}

The following example shows the format of the SecretActionPrototype object.

```json

{
  "action_type" : "public_cert_action_validate_dns_challenge"
}
```
{: codeblock}

### SecretLockPrototype[]
{: #cli-secret-lock-prototype-example-schema}

The following example shows the format of the SecretLockPrototype[] object.

```json

[ {
  "name" : "lock-example-1",
  "description" : "lock for consumer 1",
  "attributes" : {
    "anyKey" : "anyValue"
  }
} ]
```
{: codeblock}

### SecretPrototype
{: #cli-secret-prototype-example-schema}

The following example shows the format of the SecretPrototype object.

```json

{
  "custom_metadata" : {
    "anyKey" : "anyValue"
  },
  "description" : "Description of my arbitrary secret.",
  "expiration_date" : "2023-10-05T11:49:42Z",
  "labels" : [ "dev", "us-south" ],
  "name" : "example-arbitrary-secret",
  "secret_group_id" : "default",
  "secret_type" : "arbitrary",
  "payload" : "secret-data",
  "version_custom_metadata" : {
    "anyKey" : "anyValue"
  }
}
```
{: codeblock}

### SecretVersionActionPrototype
{: #cli-secret-version-action-prototype-example-schema}

The following example shows the format of the SecretVersionActionPrototype object.

```json

{
  "action_type" : "private_cert_action_revoke_certificate"
}
```
{: codeblock}

### SecretVersionPrototype
{: #cli-secret-version-prototype-example-schema}

The following example shows the format of the SecretVersionPrototype object.

```json

{
  "payload" : "updated secret credentials",
  "custom_metadata" : {
    "anyKey" : "anyValue"
  },
  "version_custom_metadata" : {
    "anyKey" : "anyValue"
  }
}
```
{: codeblock}
