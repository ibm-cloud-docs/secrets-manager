---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-20"

keywords: unable to configure IAM secrets engine, can't create API key, access required for IAM secrets engine

subcollection: secrets-manager

content-type: troubleshoot

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


# Why can't I configure the IAM secrets engine?
{: #troubleshoot-IAM-secrets-engine}
{: troubleshoot}
{: support}

You try to configure the IAM secrets engine in a {{site.data.keyword.secrets-manager_full}} instance, but you're unable to do so.
{:shortdesc}

In the {{site.data.keyword.secrets-manager_short}} UI, you go to the **Settings** page to configure the IAM secrets engine for a {{site.data.keyword.secrets-manager_short}} instance. You receive the following error message when you try to create an API key:
{: tsSymptoms}

```
Access required
You're not authorized to complete this action. To verify your permissions, contact your administrator.
```
{: screen}

You verify with an account owner that you already have [**Manager** service access](/docs/secrets-manager?topic=secrets-manager-iam#iam-roles-actions) to {{site.data.keyword.secrets-manager_short}}, but you're still unable to configure the IAM secrets engine for the instance.

You need extra permissions to [create service ID API keys](/docs/account?topic=account-account-services#identity-service-account-management) in the account.
{: tsCauses}

To resolve the issue, verify with the account owner that you're assigned the following IAM permissions:
{: tsResolve}

- _Administrator_ platform access on the IAM Access Groups Service.
- _Administrator_ platform access on the IAM Identity Service.
- _Manager_ service access on the {{site.data.keyword.secrets-manager_short}} instance.

If the problem persists, contact {{site.data.keyword.cloud_notm}} support.



