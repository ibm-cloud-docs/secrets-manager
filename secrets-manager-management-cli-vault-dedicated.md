---

copyright:
  years: 2026
lastupdated: "2026-09-01"

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

The Vault Dedicated plan is currently available as a public beta. Beta features are provided for evaluation and testing purposes and have limitations compared to generally available features.
{: beta}

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
{: pre}

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
{: pre}

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
{: pre}

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

Generate a Vault admin token for authenticating to your Vault Dedicated cluster. The token is valid for 1 hour and grants administrative privileges. Use only for initial setup and cluster management, then revoke immediately.

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```
{: pre}


#### Command options
{: #secrets-manager-instance-management-admin-token-create-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-admin-token-create-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id bfc50c2e-d66d-4f37-9ccf-9713f8325b39
```
{: pre}

### `ibmcloud secrets-manager-instance-management admin-tokens-delete`
{: #secrets-manager-instance-management-cli-admin-tokens-delete-command}

Revoke all active Vault admin tokens. This immediately invalidates all existing admin tokens.

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```
{: pre}


#### Command options
{: #secrets-manager-instance-management-admin-tokens-delete-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-admin-tokens-delete-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id bfc50c2e-d66d-4f37-9ccf-9713f8325b39
```
{: pre}

## Instances
{: #secrets-manager-instance-management-instances-cli}

Instance details.

### `ibmcloud secrets-manager-instance-management instance-details`
{: #secrets-manager-instance-management-cli-instance-details-command}

Get service instance details including cluster state, endpoints, and key management service.

```sh
ibmcloud secrets-manager-instance-management instance-details --id ID [--region REGION] [-j, --jmes-query JMES-QUERY] [--output OUTPUT] [-q, --quiet]
```
{: pre}


#### Command options
{: #secrets-manager-instance-management-instance-details-cli-options}

`--id` (string)
:   Secrets Manager instance ID. Required.

    Length must be `36` characters. The value must match regular expression `/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/`.

#### Example
{: #secrets-manager-instance-management-instance-details-examples}

Example request

```sh
ibmcloud secrets-manager-instance-management instance-details --id bfc50c2e-d66d-4f37-9ccf-9713f8325b39
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
