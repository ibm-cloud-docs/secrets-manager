---

copyright:
  years: 2022
lastupdated: "2022-08-24"

keywords: Secrets Manager Vault, Vault CLI, HashiCorp, Vault, Vault wrapper, use Vault with Secrets Manager, KV, key-value, KV CLI

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

# Managing key-value secrets with Vault CLI
{: #vault-manage-kv-cli}

If you already use HashiCorp Vault, you can use the command-line interface (CLI) to interact with {{site.data.keyword.secrets-manager_full}} to manage your [key-value secrets](/docs/secrets-manager?topic=secrets-manager-key-value&interface=ui). All operations follow the guidelines that are available for the Vault CLI.
{: shortdesc}

Before you get started, [configure the Vault CLI](/docs/secrets-manager?topic=secrets-manager-configure-vault-cli) so that you're able to access your {{site.data.keyword.secrets-manager_short}} instance by using Vault commands. To learn more about using the Vault CLI, check out the Vault documentation.

To use the standard CLI for Secrets Manager, check out the [{{site.data.keyword.secrets-manager_short}} CLI reference](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli).

## Create or update a key-value secret
{: #update-kv-secret-cli}

Create a version of a key-value secret.

```sh
vault kv put [-format=FORMAT] ibmcloud/kv/SECRET_NAME [KEY_VALUE_PAIRS]
```
{: codeblock}


Create a version of a key-value secret in a custom group.

```sh
vault kv put [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME [KEY_VALUE_PAIRS]
```
{: codeblock}

### Prerequisites
{: #update-kv-secret-cli-prereq}

You need the [**Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to create a key-value secret.

### Command options
{: #update-kv-secret-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #update-kv-secret-cli-example}

Create or update the payload of a key-value secret. 

```sh
vault kv put ibmcloud/kv/example-kv-secret key1=value1 key2=value2 
```
{: pre}

Create or update the payload of a key-value secret in a custom group.

```sh
vault kv put ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/example-kv-secret key1=value1 key2=value2 
```
{: pre}

### Output
{: #update-kv-secret-cli-output}

The command to create a new version of a `kv` secret returns the following output:

```plaintext
Key              Value
---              -----
created_time     2022-03-08T18:32:43.610242127Z
deletion_time    n/a
destroyed        false
version          2
```
{: screen}

## Read a version of a key-value secret
{: #kv-version-cli}

Get a version of a key-value secret. A successful request returns the secret data that is associated with the specified version of your secret, along with other metadata.

```sh
vault kv get [-version=VERSION] [-format=FORMAT] ibmcloud/kv/SECRET_NAME
```
{: codeblock}

Get a version of a key-value secret in a custom group.

```sh
vault kv get [-version=VERSION] [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME
```
{: codeblock}

### Prerequisites
{: #kv-version-cli-prereq}

You need the [**Reader** or **Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to read versions of key-value secrets.

### Command options
{: #kv-version-cli-option}

-version
:   The version that you want to read. If omitted, the latest version is returned.

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #kv-version-cli-example}

Read a version of a key-value secret.

```sh
vault kv get -version=1 ibmcloud/kv/my-test-kv-secret
```
{: pre}

Read a version of a key-value secret in a custom group.

```sh
vault kv get -version=1 ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}

### Output
{: #kv-version-cli-output}

The command to read a version of a `kv` secret returns the following output:

```plaintext
====== Metadata ======
Key              Value
---              -----
created_time     2022-03-04T17:08:34.406336489Z
deletion_time    n/a
destroyed        false
version          1

==== Data ====
Key     Value
---     -----
key1    value1
```
{: screen}


## Delete the latest version of a key-value secret
{: #kv-delete-cli}

Delete the latest version of a key-value secret. You can undo the deletion by calling the [undelete](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv-cli#kv-version-restore-cli) command.

```sh
vault kv delete [-format=FORMAT] ibmcloud/kv/SECRET_NAME 
```
{: codeblock}

Delete the latest version of a key-value secret in a custom group.

```sh
vault kv delete [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME 
```
{: codeblock}

### Prerequisites
{: #kv-delete-cli-prereq}

You need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to delete key-value secrets.

### Command option
{: #kv-delete-cli-option}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #kv-delete-cli-example}

Delete the latest version of a key-value secret. 

```sh
vault kv delete -format=json ibmcloud/kv/my-test-kv-secret
```
{: pre}

Delete the latest version of a key-value secret in a custom group.

```sh
vault kv delete -format=json ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}

### Output
{: #kv-version-delete-cli-output}

The command to delete the latest version of a `kv` secret returns no output.


## Delete specified versions of a key-value secret
{: #kv-delete-versions-cli}

Delete the specified versions of a key-value secret. You can undo the deletion by calling the [undelete](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv-cli&interface=ui#kv-version-restore-cli) command.

```sh
vault kv delete [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/SECRET_NAME
```
{: codeblock}

Delete the specified versions of a key-value secret in a custom group.

```sh
vault kv delete [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME
```
{: codeblock}


### Prerequisites
{: #kv-delete-versions-cli-prereq}

You need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to delete key-value secrets.

### Command options
{: #kv-delete-versions-cli-options}

-versions
:   The versions that you want to delete.

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #kv-delete-versions-cli-example}

Delete specified versions of a key-value secret. 

```sh
vault kv delete -versions=2 ibmcloud/kv/my-test-kv-secret
```
{: pre}

Delete specified versions of a key-value secret in a custom group.

```sh
vault kv delete -versions=2 ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}


### Output
{: #kv-delete-versions-cli-output}

The command to delete specified versions of a `kv` secret returns no output.

## Undelete a key-value secret
{: #kv-version-restore-cli}

Restore a previously deleted version of a key-value secret.

```sh
vault kv undelete [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/SECRET_NAME 
```
{: codeblock}

Restore a previously deleted version of a key-value secret in a custom group.

```sh
vault kv undelete [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME 
```
{: codeblock}

### Prerequisites
{: #kv-version-restore-cli-prereq}

You need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to restore secrets.


### Command options
{: #kv-version-restore-cli-options}

-versions
:   The versions that you want to delete.

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.


### Example
{: #kv-version-restore-cli-example}

Undelete specified versions of a key-value secret. 

```sh
vault kv undelete -versions=2 ibmcloud/kv/my-test-kv-secret
```
{: pre}

Undelete specified versions of a key-value secret. 


```sh
vault kv undelete -versions=2 ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}

### Output
{: #kv-version-restore-cli-output}

The command to undelete specified versions of a `kv` secret returns the following output:

```plaintext
Success! Data written to: ibmcloud/kv/undelete/my-test-kv-secret
```
{: screen}


## Destroy versions of a secret
{: #kv-destroy-cli}

Destroy specified versions of a key-value secret permanently. To soft delete versions of a secret instead, use the [delete specified versions](/docs/secrets-manager?topic=secrets-manager-vault-manage-kv-cli&interface=ui#kv-delete-versions-cli) command.


```sh
vault kv destroy [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/SECRET_NAME
```
{: codeblock}

Destroy the specified versions of a key-value secret in a custom group.

```sh
vault kv destroy [-versions=VERSIONS] [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME
```
{: codeblock}


### Prerequisites
{: #kv-destroy-cli-prereq}

You need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to delete secrets.

### Command options
{: #kv-destroy-cli-options}

-versions
:   The versions that you want to destroy.

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #kv-destroy-cli-example}

Delete permanently specified versions of a key-value secret. 

```sh
vault kv destroy -versions=2 ibmcloud/kv/my-test-kv-secret
```
{: pre}

Delete permanently specified versions of a key-value secret in a custom group. 
```sh
vault kv destroy -versions=2 ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}

### Output
{: #kv-destroy-cli-output}

The command to destroy versions of a `kv` secret returns the following output:

```plaintext
Success! Data written to: ibmcloud/kv/undelete/my-test-kv-secret
```
{: screen}


## Create or update metadata key-value secret
{: #update-kv-secret-metadata-cli}

Create or update metadata of a key-value secret.

```sh
vault kv metadata put [-format=FORMAT] [METADATA_KEY_VALUE_PAIRS] ibmcloud/kv/SECRET_NAME 
```
{: codeblock}


Create or update metadata of a key-value secret in a custom group.

```sh
vault kv metadata put [-format=FORMAT] [METADATA_KEY_VALUE_PAIRS] ibmcloud/kv/GROUP_ID/SECRET_NAME 
```
{: codeblock}

### Prerequisites
{: #update-kv-secret-metadata-cli-prereq}

You need the [**Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to create a key-value secret.

### Command options
{: #update-kv-secret-metadata-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #update-kv-secret-metadata-cli-example}

Create or update the payload of a key-value secret. 

```sh
vault kv metadata put -custom-metadata=key1=value1 -custom-metadata=key2=value2 ibmcloud/kv/mysecret
```
{: pre}

Create or update the payload of a key-value secret in a custom group.

```sh
vault kv metadata put -custom-metadata=key1=value1 -custom-metadata=key2=value2 ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/mysecret 
```
{: pre}

### Output
{: #update-kv-secret-metadata-cli-output}

The command to create a new version of a `kv` secret returns the following output:

```plaintext
Key              Value
---              -----
created_time     2022-03-08T18:32:43.610242127Z
deletion_time    n/a
destroyed        false
version          2
```
{: screen}


## Read metadata of key-value secret
{: #kv-metadata-cli}

Get a key-value secret's metadata. 

```sh
vault kv metadata get ibmcloud/kv/SECRET_NAME
```
{: pre}

Get a key-value secret's metadata in a custom group. 

```sh
vault kv metadata get ibmcloud/kv/GROUP_ID/SECRET_NAME
```
{: pre}

### Prerequisites
{: #kv-metadata-cli-prereq}

You need the [**Reader** or **Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to read the metadata of key-value secrets.

### Command options
{: #kv-metadata-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.


### Example
{: #kv-metadata-cli-example}

Read the metadata of a key-value secret. 

```sh
vault kv metadata get ibmcloud/kv/my-test-kv-secret
```
{: pre}

Read the metadata of a key-value secret in a custom group
. 
```sh
vault kv metadata get ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}


### Output
{: #kv-metadata-cli-output}

The command to read the metadata of a `kv` secret returns the following output:

```plaintext
========== Metadata ==========
Key                     Value
---                     -----
cas_required            false
created_time            2022-03-04T17:08:34.406336489Z
current_version         3
custom_metadata         map[key1:value1 key2:value2]
delete_version_after    0s
max_versions            10
oldest_version          0
updated_time            2022-03-08T20:06:39.585190049Z

====== Version 1 ======
Key              Value
---              -----
created_time     2022-03-04T17:08:34.406336489Z
deletion_time    n/a
destroyed        true

====== Version 2 ======
Key              Value
---              -----
created_time     2022-03-08T18:32:43.610242127Z
deletion_time    2022-03-08T20:05:08.850704454Z
destroyed        false

====== Version 3 ======
Key              Value
---              -----
created_time     2022-03-08T20:06:39.585190049Z
deletion_time    n/a
destroyed        false
```
{: screen}


## Delete the metadata and all versions of a key-value secret
{: #kv-delete-metadata-cli}

Delete the metadata and all version data of a specified key-value secret permanently. All version history is removed when you use this command.

```sh
vault kv metadata delete ibmcloud/kv/SECRET_NAME
```
{: pre}

```sh
vault kv metadata delete ibmcloud/kv/GROUP_ID/SECRET_NAME
```
{: pre}

### Prerequisites
{: #kv-delete-metadata-cli-prereq}

You need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to delete the metadata of key-value secrets.


### Command options
{: #kv-delete-metadata-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #kv-delete-metadata-cli-example}

Delete permanently the data and metadata of a key-value secret. 

```sh
vault kv metadata delete ibmcloud/kv/my-test-kv-secret
```
{: pre}

Delete permanently the data and metadata of a key-value secret in a custom secret group.

```sh
vault kv metadata delete ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/my-test-kv-secret
```
{: pre}

### Output
{: #kv-delete-metadata-cli-output}

The command to delete the metadata of a `kv` secret returns the following output:

```plaintext
Success! Data deleted (if it existed) at: ibmcloud/kv/metadata/my-test-kv-secret
```
{: screen}


## List key names of a key-value secret
{: #list-kv-cli}

Get a list of key names of a key-value secret. Do not encode sensitive information in key names. The values of the keys are not accessible by using this command.

```sh
vault kv list ibmcloud/kv
```
{: pre}

List the key names of key-value secrets that are stored in a custom secret group. 

```sh
vault kv list ibmcloud/kv/GROUP_ID
```
{: pre}

### Prerequisites
{: #list-kv-cli-prereq}

You need the [**Reader** or **Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to read the key names of key-value secrets.

### Command options
{: #list-kv-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #list-kv-cli-example}

List the key names of key-value secrets that are stored in the `default` secret group.

```sh
vault kv list ibmcloud/kv
```
{: pre}

List the key names of key-value secrets that are stored in a custom secret group. 

```sh
vault kv list ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8
```
{: pre}


### Output
{: #list-kv-cli-output}

The command to list key names of a `kv` secret returns the following output:

```plaintext
Keys
----
my-updated-kv-secret
```
{: screen}




## Patch a key-value secret
{: #patch-kv-secret-cli}

Patch a version of a key-value secret.

```sh
vault kv patch [-format=FORMAT] ibmcloud/kv/SECRET_NAME [KEY_VALUE_PAIRS]
```
{: codeblock}


Create a version of a key-value secret in a custom group.

```sh
vault kv patch [-format=FORMAT] ibmcloud/kv/GROUP_ID/SECRET_NAME [KEY_VALUE_PAIRS]
```
{: codeblock}

### Prerequisites
{: #patch-kv-secret-cli-prereq}

You need the [**Writer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to create a key-value secret.

### Command options
{: #patch-kv-secret-cli-options}

-format
:   Prints the output in the format that you specify. Valid formats are `table`, `json`, and `yaml`. The default is `table`. You can also set the output format by using the `VAULT_FORMAT` environment variable.

### Example
{: #patch-kv-secret-cli-example}

Create or update the payload of a key-value secret. 

```sh
vault kv patch ibmcloud/kv/example-kv-secret key1=value1 key2=value2 
```
{: pre}

Create or update the payload of a key-value secret in a custom group.

```sh
vault kv patch ibmcloud/kv/9426e546-83de-4da5-9631-d70c993186c8/example-kv-secret key1=value1 key2=value2 
```
{: pre}

### Output
{: #patch-kv-secret-cli-output}

The command to create a new version of a `kv` secret returns the following output:

```plaintext
Key              Value
---              -----
created_time     2022-03-08T18:32:43.610242127Z
deletion_time    n/a
destroyed        false
version          2
```
{: screen}
