---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-30"

keywords: change log for {{site.data.keyword.secrets-manager_short}} CLI, change log for {{site.data.keyword.secrets-manager_short}} API, updates to {{site.data.keyword.secrets-manager_short}} CLI, updates to {{site.data.keyword.secrets-manager_short}} API

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



# Change logs for {{site.data.keyword.secrets-manager_short}}
{: #change-log}



You're viewing the **{{site.data.keyword.secrets-manager_short}} CLI** change log. To view the API change log, switch to the **API** tab.
{: note}
{: cli}

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} CLI plug-in](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli). The change log lists changes that have been made, ordered by the date they were released. Changes to existing CLI versions are designed to be compatible with existing client applications.
{: shortdesc}
{: cli}




You're viewing the **{{site.data.keyword.secrets-manager_short}} API** change log. To view the CLI change log, switch to the **CLI** tab.
{: note}
{: api}

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} API](/apidocs/secrets-manager). The change log lists changes that have been made, ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}
{: api}

To learn about general updates and improvements to the {{site.data.keyword.secrets-manager_short}} service, see [Release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).



## Version 0.0.10
{: #v0.0.10-cli}
{: cli}

`ibmcloud secrets-manager` version 0.0.10 was released on 23 April 2021. This release includes the following updates:

- Changed the **--secret-action-one-of** flag in the [secret-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-update-command) command to **--body**.
- Added **--groups** flag to the [all-secrets](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-all-secrets-command) command.

## 13 April 2021
{: #2021-04-13-api}
{: api}

This release includes the following updates:

- Added `group={secret_group_ID}` query parameter that can be used to filter a list of secrets by secret group.


## Version 0.0.8
{: #v0.0.8-cli}
{: cli}

`ibmcloud secrets-manager` version 0.0.8 was released on 23 March 2021. This release includes the following updates:

- Removed **--metadata** flags from the [secret-create](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command), [secret-metadata-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command), [secret-group-create](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-group-create-command), and [secret-group-metadata-update](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-group-metadata-update-command) commands.
- Added **--search** and **--sort-by** flags to the [all-secrets](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-all-secrets-command) command.
- Changed the environment variable for targeting a {{site.data.keyword.secrets-manager_short}} instance to `SECRETS_MANAGER_URL`.


## 7 March 2021
{: #2021-03-07-api}
{: api}

This release includes the following updates:

- Added the `reuse_api_key` boolean parameter for IAM credential secrets.


## 10 February 2021
{: #2021-02-10-api}
{: api}

This release includes the following updates:

- Added the `search={string}` query parameter that can be used to filter a list of secrets that contain a specified string.
- Added the `sort_by={field_name}` query parameter that can be used to filter a list of secrets by a specified metadata field.

## 25 January 2021
{: #2021-01-25-api}
{: api}

This release includes the following updates:

- Changed the maximum length for secret names to 128 characters.
- Changed the maximum length for secret group names to 62 characters.


## Version 0.0.6
{: #v0.0.6-cli}
{: cli}

`ibmcloud secrets-manager` version 0.0.6 was released on 14 December 2020. This release includes the following updates:

- Plug-in now available for download.

