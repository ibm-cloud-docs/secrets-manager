---

copyright:
  years: 2020
lastupdated: "2020-12-07"

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
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}

# Understanding high availability and disaster recovery for {{site.data.keyword.secrets-manager_short}}
{: #ha-dr}

{{site.data.keyword.secrets-manager_full}} is a highly available, regional service that runs in the Dallas (`us-south`) and Frankfurt (`eu-de`) regions.
{: shortdesc}

In each supported region, the service exists in multiple availability zones with no single point of failure. All customer data is backed up across regions. 

However, because {{site.data.keyword.secrets-manager_short}} is a regional service, there is no automatic cross-regional failover or cross-regional disaster recovery. If all of the availability zones in a region fail, {{site.data.keyword.secrets-manager_short}} becomes unavailable in that location. When the region is available again, customer data and traffic is restored without any need for action from you.

See [How do I ensure zero downtime?](/docs/overview?topic=overview-zero-downtime) to learn more about the high availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. You can also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).  

