---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: 412 Precondition Failed, IAM credentials, locked IAM credentials

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

# Why can't I read a locked IAM credentials secret?
{: #locked-iam-credentials}
{: troubleshoot} 
{: support}

You try to read or access an IAM credentials secret that you manage in {{site.data.keyword.secrets-manager_full}}, but you get a `412 Precondition Failed` response.
{: shortdesc}

You have an IAM credentials secret that you want to regenerate for your application. But when you use the {{site.data.keyword.secrets-manager_short}} APIs, SDKs, or CLI to get the secret, you see the following `412 Precondition Failed` error:
{: tsSymptoms}

```
The requested action can't be completed because the secret version is locked.
```
{: screen}

A lock on a secret prevents it from being modified or deleted from your instance. IAM credentials are [dynamic secrets](#x9968958){: term}. By default, each request to read an IAM credential (for example, a `GET` request) generates a new service ID API key, deletes the old credentials, and returns the new credentials. Locking the secret overrides this default behavior and returns a `412 Precondition Failed` error to indicate that the secret data is locked. A locked IAM credential can't be read, because doing so modifies its secret data.
{: tsCauses}

To regenerate your IAM credentials, you can remove all of the locks that are associated with your secret, and try again. To delete locks from the {{site.data.keyword.secrets-manager_short}} UI, go to **Secrets > _secret name_ > Locks > Delete**. 
{: tsResolve}




