---

copyright:
  years: 2020, 2023
lastupdated: "2023-02-20"

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

{{site.data.keyword.secrets-manager_full}} is a highly available, regional service that runs in the Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Osaka (`jp-osa`), Sydney (`au-syd`), Sao Paulo (`br-sao`), Tokyo (`jp-tok`), Toronto (`ca-tor`), and Washington DC (`us-east`) regions.
{: shortdesc}

In each supported region, the service exists in multiple availability zones with no single point of failure. All the data that is associated with your instance of the service, including your secrets, is backed up across regions.

However, because {{site.data.keyword.secrets-manager_short}} is a regional service, cross-regional failover, and cross-regional disaster recovery are not automatic. If all the availability zones in a region fail, {{site.data.keyword.secrets-manager_short}} becomes unavailable in that location. When the region is available again, data and traffic is restored without any need for action from you.

See [How do I ensure zero downtime?](/docs/overview?topic=overview-zero-downtime) to learn more about the high availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. You can also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

## Manually backing up secrets
{: #manual-backup}

To manually back up your secrets across regions, you must first have an instance of {{site.data.keyword.secrets-manager_short}} in another region. Then, use the following steps to ensure cross-region availability.

1. List and download secrets from your instance by using the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager) or CLI.

   If you have existing configurations on secrets engines in your instance, you can also retrieve the information programmatically so that it can be re-created in a new instance. For more information, see the [Get the configuration of a secret type](/apidocs/secrets-manager#get-config) API.

2. Add your downloaded secrets to the newly created instance.

## Automatically backing up secrets
{: #auto-backup}

Creating an automatic backup of your secrets is possible by automating the manual flow, which can be done in various ways. Check out some of the following examples to see whether one of them might work for you.

- Create a script that periodically downloads all of your secrets and then imports them into your backup instance.
- [Create a destination and subscription in {{site.data.keyword.en_short}}](/docs/event-notifications) that points to an IBM Cloud Functions action. Configure the action to listen for lifecycle events such as `secret_created` and `secret_rotated`. Then, when the action receives the event, the action downloads the secret from one instance and adds it to the backup instance.

   Currently, {{site.data.keyword.secrets-manager_short}} supports notifications for certificates only. To learn about the various available lifecycle event types, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).
   {: note}

## Recovering data from {{site.data.keyword.keymanagementserviceshort}} backup instance
{: #byok-backup}

Did you experience a failure with your {{site.data.keyword.keymanagementservicelong_notm}} instance? If you backed up your data to a new {{site.data.keyword.keymanagementserviceshort}} instance by using the same root keys, you can update your Secrets Manager instance with data from your backup {{site.data.keyword.keymanagementserviceshort}} instance. To do so, open a case and include the following information.

* Your Secrets Manager instance's CRN
* Your backup {{site.data.keyword.keymanagementserviceshort}} instance's CRN
* The key ID
* The original {{site.data.keyword.keymanagementserviceshort}} instance's CRN and key ID, if available

You must enable service-to-service authorization between your backup {{site.data.keyword.keymanagementserviceshort}} instance and your Secrets Manager instance.
{: important}



