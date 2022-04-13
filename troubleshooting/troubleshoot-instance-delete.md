---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-23"

keywords: {{site.data.keyword.secrets-manager_short}} instance deleted, lite plan, deprecated

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


# Why was my instance of {{site.data.keyword.secrets-manager_short}} deleted?
{: #troubleshoot-instance-delete}
{: troubleshoot}


You try to access your instance of {{site.data.keyword.secrets-manager_full}}, but it was removed from your account.
{: shortdesc}


Your instance of {{site.data.keyword.secrets-manager_short}} is missing from your account.
{: tsSymptoms}


Your instance of the service might have been deleted for one of two reasons.
{: tsCauses}


On 23 March 2022, {{site.data.keyword.secrets-manager_short}} introduced Standard and Trial pricing plans. As part of that release, the current Lite plan was deprecated. For more information about the deprecation timeline, see the [release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).

Instances on the Trial plan are only available for 30 days. If, when the trial period ends, you choose not to upgrade your plan, the instance is removed from your account.


If you were working with an instance of the service on the Lite plan that was deleted, you can restore your instance for 7 days through the [reclamation process](/docs/secrets-manager?topic=secrets-manager-mng-data#restore-instance). However, to continue working with the reclaimed instance you must upgrade the instance to the Standard plan within 1 hour.
{: tsResolve}

