---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-03"

keywords: change log for Secrets Manager CLI, CLI changelog, updates to Secrets Manager CLI

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

# {{site.data.keyword.secrets-manager_short}} CLI change log
{: #cli-change-log}

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} CLI plug-in](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli) (`ibmcloud secrets-manager`). The change log lists changes that have been made, ordered by the date they were released. Changes to existing CLI versions are designed to be compatible with existing client applications.
{: shortdesc}

To learn about general updates and improvements to the {{site.data.keyword.secrets-manager_short}} service, see [Release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).







## Version 0.1.14
{: #0.1.14}

Version 0.1.14 was released on 6 December 2021. This release includes the following updates:

- Added options for setting the `SECRETS_MANAGER_URL`. You can now either define your {{site.data.keyword.secrets-manager_short}} service endpoint URL once by setting an environment variable (for example `export SECRETS_MANAGER_URL`), or you can override the value on the command-level by using the `--service-url INSTANCE` flag.
- Added support for using the {{site.data.keyword.secrets-manager_short}} CLI plug-in to interact with a service instance over a private network connection.
- Fixed an issue in the [config-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-update-command) command for IAM credential secrets. 
- Updated command descriptions for clarity.


## Version 0.1.12
{: #0.1.12}

Version 0.1.12 was released on 20 September 2021. This release includes the following updates:

- Added support for the `public_cert` secret type.
- Added the [config-element-create](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-element-create-command), [config-elements](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-elements-command), [config-element](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-element-command), [config-element-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-element-update-command), and [config-element-delete](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-element-delete-command) commands. Currently, these commands support adding configurations for `public_cert` secrets only. 
- Updated the [secret-version](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-version-command) and [secret-version-metadata](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-version-metadata-command) commands. These commands now support `pubilc_cert` and `imported_cert` secrets.


## Version 0.0.11
{: #0.0.11}

Version 0.0.11 was released on 23 June 2021. This release includes the following updates:

- Added support for the `imported_cert` secret type.
- Added the [secret-version](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-version-command) and [secret-version-metadata](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-version-metadata-command) commands. Currently, these commands support `imported_cert` secrets only.
- Changed the **--engine-config-one-of** flag in the [config-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-update-command) command to **--engine-config**.


## Version 0.0.10
{: #0.0.10}

Version 0.0.10 was released on 23 April 2021. This release includes the following updates:

- Changed the **--secret-action-one-of** flag in the [secret-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-update-command) command to **--body**.
- Added **--groups** flag to the [all-secrets](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-all-secrets-command) command.


## Version 0.0.8
{: #0.0.8}

Version 0.0.8 was released on 23 March 2021. This release includes the following updates:

- Removed **--metadata** flags from the [secret-create](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command), [secret-metadata-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command), [secret-group-create](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-group-create-command), and [secret-group-metadata-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-group-metadata-update-command) commands.
- Added **--search** and **--sort-by** flags to the [all-secrets](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-all-secrets-command) command.
- Changed the environment variable for targeting a {{site.data.keyword.secrets-manager_short}} instance to `SECRETS_MANAGER_URL`.

## Version 0.0.6
{: #0.0.6}

Version 0.0.6 was released on 14 December 2020. This release includes the following updates:

- Plug-in now available for download.



