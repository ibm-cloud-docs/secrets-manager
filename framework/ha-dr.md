---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-28"

keywords: HA for {{site.data.keyword.secrets-manager_short}}, DR for {{site.data.keyword.secrets-manager_short}}, high availability for {{site.data.keyword.secrets-manager_short}}, disaster recovery for {{site.data.keyword.secrets-manager_short}}, failover for {{site.data.keyword.secrets-manager_short}}

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

# Understanding high availability and disaster recovery for {{site.data.keyword.secrets-manager_short}}
{: #ha-dr}

{{site.data.keyword.secrets-manager_full}} is a highly available, regional service that runs in the Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Sydney (`au-syd`), Sao Paulo (`br-sao`), Tokyo (`jp-tok`), and Washington DC (`us-east`) regions.
{: shortdesc}

In each supported region, the service exists in multiple availability zones with no single point of failure. All of the data that is associated with your instance of the service, including your secrets, is backed up across regions.

However, because {{site.data.keyword.secrets-manager_short}} is a regional service, there is no automatic cross-regional failover or cross-regional disaster recovery. If all of the availability zones in a region fail, {{site.data.keyword.secrets-manager_short}} becomes unavailable in that location. When the region is available again, data and traffic is restored without any need for action from you.

See [How do I ensure zero downtime?](/docs/overview?topic=overview-zero-downtime) to learn more about the high availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. You can also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

