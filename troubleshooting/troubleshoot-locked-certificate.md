---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: locked certificate expired, locked certificate destroyed, certificate with lock expired, certificate with lock destroyed

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

# Why did my locked certificate move to the Destroyed state?
{: #locked-certificates}
{: troubleshoot} 
{: support}

You have an SSL/TLS certificate that you manage in {{site.data.keyword.secrets-manager_full}}, but it still expires even though it is locked.
{: shortdesc}

You want to prevent an SSL/TLS certificate in your {{site.data.keyword.secrets-manager_short}} service instance from expiring, so you attach one or more locks to it. But, when your certificate reaches its expiration date, it still moves to the **Destroyed** state. 
{: tsSymptoms}


A lock on an SSL/TLS certificate can prevent you or an authorized user from deleting the certificate from your instance, for example during a security audit. But, a lock can't prevent a certificate from reaching its defined expiration date.
{: tsCauses}

The validity period of an X.509 certificate can't be changed or modified, even if the certificate is associated with one or more locks in {{site.data.keyword.secrets-manager_short}}. When your certificate passes its defined expiration date, it is no longer valid. In {{site.data.keyword.secrets-manager_short}}, a secret that is no longer valid moves to the **Destroyed** state.

To avoid downtime in your applications that results from an expired certificate, be sure to [set up {{site.data.keyword.en_short}}](/docs/secrets-manager?topic=secrets-manager-event-notifications) to alert you when certificates are about to expire. Then, rotate your certificates and deploy the new versions to your SSL/TLS termination points. For suggested guidelines around periodic rotation of certificates, see [Best practices for rotating and locking secrets](/docs/secrets-manager?topic=secrets-manager-best-practices-rotate-secrets#best-practices-lock-secrets).
{: tsResolve}




