---

copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: Secrets Manager CLI, Vault Dedicated, instance management CLI, command line, terminal

subcollection: secrets-manager

content-type: cli-docs

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


# {{site.data.keyword.secrets-manager_short}} instance management CLI
{: #secrets-manager-management-cli}

You can use the {{site.data.keyword.secrets-manager_full}} instance management command-line interface (CLI) to manage your {{site.data.keyword.secrets-manager_short}} Vault Dedicated instance from the command line.
{: shortdesc}

Current version: **`2.0.7`**

## Before you begin
{: #secrets-manager-management-cli-prereq}

Before you get started, install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started), and then install the {{site.data.keyword.secrets-manager_short}} instance management CLI plug-in by running the following command:

```sh
ibmcloud plugin install secrets-manager-instance-management
```
{: pre}

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Keep your CLI up to date to use the latest commands, so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}

## Targeting a {{site.data.keyword.secrets-manager_short}} instance
{: #target-sm-management-instance}

To target the {{site.data.keyword.secrets-manager_short}} Vault Dedicated instance, use one of the following options.

* Run the `ibmcloud secrets-manager-instance-management config set` command.

   ```sh
   ibmcloud secrets-manager-instance-management config set service-url https://{instance_ID}.{region}.secrets-manager.cloud.ibm.com
   ```
   {: pre}

* Export an environment variable with your {{site.data.keyword.secrets-manager_short}} control plane service endpoint URL.

   ```sh
   export SECRETS_MANAGER_INSTANCE_MANAGEMENT_URL=https://{instance_ID}.{region}.secrets-manager.cloud.ibm.com
   ```
   {: pre}

* Set the service endpoint in the command.

   ```sh
   ibmcloud secrets-manager-instance-management instance --service-url https://{instance_ID}.{region}.secrets-manager.cloud.ibm.com
   ```
   {: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} Vault Dedicated service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls).

## Globals
{: #secrets-manager-instance-management-globals}

### Commands
{: #secrets-manager-instance-management-commands}

#### `ibmcloud secrets-manager-instance-management docs`
{: #secrets-manager-instance-management-cli-docs-command}

Opens the plug-in documentation in the web browser.

```sh
ibmcloud secrets-manager-instance-management docs
```

##### Example
{: #secrets-manager-instance-management-docs-examples}

```sh
ibmcloud secrets-manager-instance-management docs
```
{: pre}

### Options
{: #secrets-manager-instance-management-global-options}

`--region` (string)
:   The region where you provisioned your Vault Dedicated Instance. Available regions: us-south, eu-de.

`--output` (string)
:   Choose an output format - can be 'json', 'yaml', or 'table'. Defaults to 'table'.

`-j`, `--jmes-query` (string)
:   Provide a JMESPath query to customize output.

`--service-url` (string)
:   Provide the base endpoint URL for the API.

`-q`, `--quiet`
:   Suppresses verbose messages.

`-v`, `--version`
:   Prints the plug-in version.

#### Example
{: #secrets-manager-instance-management-global-options-example}

```sh
ibmcloud secrets-manager-instance-management \
    --region=us-south \
    --output=json \
    --jmes-query="[:10]" \
    --service-url="https://myservice.cloud.ibm.com" \
    --quiet
```
{: pre}

This example only demonstrates the global options available to all sub-commands and is not a valid command itself.
{: note}

## Config
{: #secrets-manager-instance-management-cli-config-command}

Global parameters can also be stored in persistent configuration so that they do not need to be manually specified each time the plug-in is invoked. Each parameter can be configured with the `config` command and its subcommands.

```sh
ibmcloud secrets-manager-instance-management config
```

### `ibmcloud secrets-manager-instance-management config set`
{: #secrets-manager-instance-management-cli-config-set-command}

Set a new config value for a specific option. Each subcommand of the `set` command maps to a global option. Each subcommand accepts a single argument, the string representation of the value to store for the option.

```sh
ibmcloud secrets-manager-instance-management config set <option> <value>
```

#### Examples
{: #secrets-manager-instance-management-config-set-command-examples}

```sh
ibmcloud secrets-manager-instance-management config set service-url \
    'https://{region}.secrets-manager.cloud.ibm.com'
```
{: pre}

### `ibmcloud secrets-manager-instance-management config get`
{: #secrets-manager-instance-management-cli-config-get-command}

Print out the currently set value for a specific option. Each subcommand of the `get` command maps to a global option.

```sh
ibmcloud secrets-manager-instance-management config get <option>
```

#### Examples
{: #secrets-manager-instance-management-config-get-command-examples}

```sh
ibmcloud secrets-manager-instance-management config get service-url
```
{: pre}

### `ibmcloud secrets-manager-instance-management config unset`
{: #secrets-manager-instance-management-cli-config-unset-command}

Unset the currently set value for a specific option. Each subcommand of the `unset` command maps to a global option.

The subcommands available for this service are: `service-url`, .

```sh
ibmcloud secrets-manager-instance-management config unset <option>
```

#### Examples
{: #secrets-manager-instance-management-config-unset-command-examples}

```sh
ibmcloud secrets-manager-instance-management config unset service-url
```
{: pre}

### `ibmcloud secrets-manager-instance-management config list`
{: #secrets-manager-instance-management-cli-config-list-command}

List out all of the currently set config values.

```sh
ibmcloud secrets-manager-instance-management config list
```

#### Examples
{: #secrets-manager-instance-management-config-list-command-examples}

```sh
ibmcloud secrets-manager-instance-management config list
```
{: pre}

## Tokens
{: #secrets-manager-instance-management-tokens-cli}

Manage admin tokens.

### `ibmcloud secrets-manager-instance-management admin-token-create`
{: #secrets-manager-instance-management-cli-admin-token-create-command}

Create a Vault admin token (valid for 1 hour). Use only for setup and cluster management, then delete it. Pass --response-wrapping=true to receive a Vault response-wrapped token instead of a plain admin token.

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id ID [--response-wrapping=RESPONSE-WRAPPING] [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-admin-token-create-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--response-wrapping` (bool)
:   If true, returns a Vault response-wrapped token (wrapped_token). If false or absent, returns a plain admin token (token).

#### Example
{: #secrets-manager-instance-management-admin-token-create-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id=fe5c5c27-3e44-474a-9484-55892e12d6d1
```
{: pre}

### `ibmcloud secrets-manager-instance-management admin-tokens-delete`
{: #secrets-manager-instance-management-cli-admin-tokens-delete-command}

Delete all active Vault admin tokens.

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-admin-tokens-delete-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-admin-tokens-delete-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id=fe5c5c27-3e44-474a-9484-55892e12d6d1
```
{: pre}

## Instances
{: #secrets-manager-instance-management-instances-cli}

Instance details.

### `ibmcloud secrets-manager-instance-management instance-details`
{: #secrets-manager-instance-management-cli-instance-details-command}

Get instance details including cluster state, endpoints, and key management service.

```sh
ibmcloud secrets-manager-instance-management instance-details --id ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-instance-details-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-instance-details-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management instance-details --id=fe5c5c27-3e44-474a-9484-55892e12d6d1
```
{: pre}

#### Example output
{: #secrets-manager-instance-management-instance-details-cli-output}

Example Instance response

```json
{
  "id" : "12345678-abcd-1234-abcd-1234567890ab",
  "name" : "Example Instance",
  "href" : "https://us-south.secrets-manager.cloud.ibm.com/v2/instances/12345678-abcd-1234-abcd-1234567890ab",
  "instance_crn" : "crn:v1:bluemix:public:secrets-manager:us-south:a/a1b2c3d4e5f61234567890abcdef1234:12345678-abcd-1234-abcd-1234567890ab::",
  "plan" : "dedicated",
  "vault_cluster" : {
    "status" : "healthy",
    "version" : "1.21.2+ent.hsm"
  },
  "endpoints" : {
    "public" : {
      "vault_api" : "https://12345678-abcd-1234-abcd-1234567890ab.us-south.secrets-manager.appdomain.cloud",
      "vault_ui" : "https://12345678-abcd-1234-abcd-1234567890ab.us-south.secrets-manager.appdomain.cloud/ui"
    },
    "private" : {
      "vault_api" : "https://private.12345678-abcd-1234-abcd-1234567890ab.us-south.secrets-manager.appdomain.cloud",
      "vault_ui" : "https://private.12345678-abcd-1234-abcd-1234567890ab.us-south.secrets-manager.appdomain.cloud/ui"
    }
  },
  "encryption" : {
    "mode" : "customer_managed",
    "provider" : "key_protect",
    "key_crn" : "crn:v1:bluemix:public:kms:us-south:a/a1b2c3d4e5f61234567890abcdef1234:abcd1234-ab12-ab12-ab12-abcdef123456:key:12ab34cd-12ab-12ab-12ab-123456abcdef"
  }
}
```
{: screen}

## Destinations
{: #secrets-manager-instance-management-destinations-cli}

Manage outbound connectivity destinations for your Vault Dedicated cluster.

### `ibmcloud secrets-manager-instance-management destinations-list`
{: #secrets-manager-instance-management-cli-destinations-list-command}

List all destinations for your Vault Dedicated cluster.

```sh
ibmcloud secrets-manager-instance-management destinations-list --instance-id INSTANCE-ID [--state (not_started | provisioning | succeeded | failed | deleting)] [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-destinations-list-cli-options}

`--instance-id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--state` (string)
:   Filter by destination state.

    Allowable values are: `not_started`, `provisioning`, `succeeded`, `failed`, `deleting`.

#### Example
{: #secrets-manager-instance-management-destinations-list-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management destinations-list --id=fe5c5c27-3e44-474a-9484-55892e12d6d1
```
{: pre}

### `ibmcloud secrets-manager-instance-management destination-create`
{: #secrets-manager-instance-management-cli-destination-create-command}

Create a new destination between your Vault Dedicated cluster and an IBM Cloud service instance.

```sh
ibmcloud secrets-manager-instance-management destination-create --instance-id INSTANCE-ID (--destination-prototype (DESTINATION-PROTOTYPE | @DESTINATION-PROTOTYPE-FILE) | --destination-name DESTINATION-NAME --destination-type ibm_cloud_database --destination-description DESTINATION-DESCRIPTION --destination-crn DESTINATION-CRN) [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-destination-create-cli-options}

`--instance-id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--destination-prototype` ([`CreateDestinationRequest`](#cli-create-destination-request-example-schema))
:   Request body for creating a destination. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example, `--destination-prototype=@path/to/file.json`.

`--destination-name` (string)
:   Destination name. This option provides a value for a sub-field of the JSON option 'destination-prototype'. It is mutually exclusive with that option.

    The maximum length is `256` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9_][A-Za-z0-9_]*(?:_*-*\\.*[A-Za-z0-9]*)*[A-Za-z0-9]+$/`.

`--destination-type` (string)
:   Destination type. This option provides a value for a sub-field of the JSON option 'destination-prototype'. It is mutually exclusive with that option.

    Allowable values are: `ibm_cloud_database`.

`--destination-description` (string)
:   Optional description. This option provides a value for a sub-field of the JSON option 'destination-prototype'. It is mutually exclusive with that option.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

`--destination-crn` (string)
:   IBM Cloud Database service instance CRN. This option provides a value for a sub-field of the JSON option 'destination-prototype'. It is mutually exclusive with that option.

    The maximum length is `512` characters. The minimum length is `9` characters. The value must match regular expression `/^crn:v[0-9](:([A-Za-z0-9-._~!$&'()*+,;=@\/]|%[0-9A-Z]{2})*){8}$/`.

#### Example
{: #secrets-manager-instance-management-destination-create-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management destination-create --id=fe5c5c27-3e44-474a-9484-55892e12d6d1 --name=my-postgres --type=ibm_cloud_database --crn=crn:v1:bluemix:public:databases-for-postgresql:us-south:a/e91c8f42b3d74e1a9c2f05d8b67a3e10:3f8b1c7a-9d42-4e6f-b8a5-2c1d9e7f4b83::
```
{: pre}

### `ibmcloud secrets-manager-instance-management destination-details`
{: #secrets-manager-instance-management-cli-destination-details-command}

Get details and current state for a specific destination for your Vault Dedicated cluster.

```sh
ibmcloud secrets-manager-instance-management destination-details --instance-id INSTANCE-ID --destination-id DESTINATION-ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-destination-details-cli-options}

`--instance-id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--destination-id` (string)
:   Destination ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-destination-details-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management destination-details --id=fe5c5c27-3e44-474a-9484-55892e12d6d1 --destination-id=ce8aa9a4-8cde-4b63-8156-d4a8b252beeb
```
{: pre}

### `ibmcloud secrets-manager-instance-management destination-update`
{: #secrets-manager-instance-management-cli-destination-update-command}

Update mutable fields (name, description) on a destination for your Vault Dedicated cluster.

```sh
ibmcloud secrets-manager-instance-management destination-update --instance-id INSTANCE-ID --destination-id DESTINATION-ID (--destination-patch (DESTINATION-PATCH | @DESTINATION-PATCH-FILE) | --name NAME --description DESCRIPTION) [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-destination-update-cli-options}

`--instance-id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--destination-id` (string)
:   Destination ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--destination-patch` (generic map)
:   JSON Merge-Patch content for update_instance_destination. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example, `--destination-patch=@path/to/file.json`.

`--name` (string)
:   Updated name. This option provides a value for a sub-field of the JSON option 'destination-patch'. It is mutually exclusive with that option.

    The maximum length is `256` characters. The minimum length is `2` characters. The value must match regular expression `/^[A-Za-z0-9_][A-Za-z0-9_]*(?:_*-*\.*[A-Za-z0-9]*)*[A-Za-z0-9]+$/`.

`--description` (string)
:   Updated description. This option provides a value for a sub-field of the JSON option 'destination-patch'. It is mutually exclusive with that option.

    The maximum length is `1024` characters. The minimum length is `0` characters. The value must match regular expression `/(.*?)/`.

#### Example
{: #secrets-manager-instance-management-destination-update-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management destination-update --id=fe5c5c27-3e44-474a-9484-55892e12d6d1 --destination-id=ce8aa9a4-8cde-4b63-8156-d4a8b252beeb --name=my-postgres-updated
```
{: pre}

### `ibmcloud secrets-manager-instance-management destination-delete`
{: #secrets-manager-instance-management-cli-destination-delete-command}

Delete a destination for your Vault Dedicated cluster.

```sh
ibmcloud secrets-manager-instance-management destination-delete --instance-id INSTANCE-ID --destination-id DESTINATION-ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```


#### Command options
{: #secrets-manager-instance-management-destination-delete-cli-options}

`--instance-id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

`--destination-id` (string)
:   Destination ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-destination-delete-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management destination-delete --id=fe5c5c27-3e44-474a-9484-55892e12d6d1 --destination-id=ce8aa9a4-8cde-4b63-8156-d4a8b252beeb
```
{: pre}

## Schema examples
{: #secrets-manager-instance-management-schema-examples}

The following schema examples represent the data that you need to specify for a command option. These examples model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment as appropriate.

### CreateDestinationRequest
{: #cli-create-destination-request-example-schema}

The following example shows the format of the CreateDestinationRequest object.

```json

{
  "name" : "my-postgres",
  "type" : "ibm_cloud_database",
  "description" : "Production PostgreSQL database",
  "crn" : "crn:v1:bluemix:public:databases-for-postgresql:us-south:a/e91c8f42b3d74e1a9c2f05d8b67a3e10:3f8b1c7a-9d42-4e6f-b8a5-2c1d9e7f4b83::"
}
```
{: codeblock}
