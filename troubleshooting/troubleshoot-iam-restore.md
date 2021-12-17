---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-17"

keywords: can't restore IAM credentials, reuse credentials is off, unable to restore

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
{:release-note: data-hd-content-type='release-note'}


# Why can't I restore the previous version of an IAM credentials secret?
{: #troubleshoot-restore-iam}
{: troubleshoot}
{: support}

You try to restore the previous version of an IAM credentials secret in {{site.data.keyword.secrets-manager_full}}, but you're unable to do so.
{: shortdesc}

You want to restore the previous service ID API key that was associated with an IAM credentials secret. When you try to [restore the secret version](/docs/secrets-manager?topic=secrets-manager-version-history) by using the {{site.data.keyword.secrets-manager_short}} UI or API, you get one of the following errors:
{: tsSymptoms}

```plaintext
Rotating IAM credentials is not supported when reuse credentials is off.
```
{: screen}

```
Version previous is not active.
```
{: screen}

You might receive these errors due to the following reasons:
{: tsCauses}

- The secret was initially created with the [**Reuse IAM credentials until lease expires** option](/docs/secrets-manager?topic=secrets-manager-iam-credentials#iam-credentials-reuse-ui) set to **Off** (or, the `reuse_api_key` property set to `false`). This means that the secret can hold only a short-lived ephemeral value that is created and and then deleted after each read operation, so its previous version can't be restored.
- The service ID API key reached its defined time-to-live (TTL) or lease duration, and it can no longer be restored.
