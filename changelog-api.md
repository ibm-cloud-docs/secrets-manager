---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-11"

keywords: change log for [{sm-short}] APIs, updates to [{sm-short}] APIs

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

# API change log
{: #api-change-log}

In this change log, you can learn about the latest changes, improvements, and updates for the {{site.data.keyword.secrets-manager_short}} API. The change log lists changes that have been made, ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

To learn more about the {{site.data.keyword.secrets-manager_short}} API, check out the [API reference](/apidocs/secrets-manager).



## 7 March 2021
{: #2021-03-07-api}

This release includes the following updates:

- Added the `reuse_api_key` boolean parameter for IAM credential secrets.

## 10 February 2021
{: #2021-02-10-api}

This release includes the following updates:

- Added the `search={string}` query parameter that can be used to filter a list of secrets that contain a specified string.
- Added the `sort_by={field_name}` query parameter that can be used to filter a list of secrets by a specified metadata field.

## 25 January 2021
{: #2021-01-25-api}

This release includes the following updates:

- Changed the maximum length for secret names to 128 characters.
- Changed the maximum length for secret group names to 62 characters.
