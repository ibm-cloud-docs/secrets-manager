---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-04"

keywords: public isolation for Secrets Manager, compute isolation for Secrets Manager, Secrets Manager architecture, workload isolation in Secrets Manager

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

# Learning about {{site.data.keyword.secrets-manager_short}} architecture and workload isolation
{: #compute-isolation}

{{site.data.keyword.secrets-manager_full}} is a single tenant, dedicated service that is managed by both you and {{site.data.keyword.cloud_notm}}. To learn more, review the following architecture diagram to see how {{site.data.keyword.secrets-manager_short}} can meet the requirements of the sensitive workloads that you want to run in the cloud.
{: shortdesc}

## {{site.data.keyword.secrets-manager_short}} architecture
{: #architecture}

The following image shows the main {{site.data.keyword.secrets-manager_short}} components, how they interact with each other, and what type of encryption is applied to your personal information.

![This image is a visual representation of the architecture for {{site.data.keyword.secrets-manager_short}}.](../images/secrets-arch.svg){: caption="Figure 1. {{site.data.keyword.secrets-manager_short}} architecture" caption-side="bottom"}

1. A user creates an instance of {{site.data.keyword.secrets-manager_short}}. At provisioning, the user can [configure a root key from a key management service](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption) or choose the default, provider-managed encryption option. A dedicated instance of the service is created.
2. When a user, CLI, application, or DevOps tool makes a request to the service by using the {{site.data.keyword.secrets-manager_short}} UI or APIs, the request is completed through their vault formation. 
3. Service data and secrets are stored in a dedicated Cloud Object Storage bucket.

## {{site.data.keyword.secrets-manager_short}} workload isolation
{: #workload-isolation}

{{site.data.keyword.secrets-manager_short}} is a single-tenant, dedicated service instance that is currently offered only as a Lite plan. Each workload is isolated within its own namespace within the data plane of the service clusters.




