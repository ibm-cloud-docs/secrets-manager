---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-09"

keywords: activity tracker events for Secrets Manager, events, Secrets Manager actions

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

# Auditing events for {{site.data.keyword.secrets-manager_short}}
{: #at-events}

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records service and user initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard.

For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started).

Audit devices that you can enable with Vault, such as the [`syslog` audit device](https://www.vaultproject.io/docs/audit/syslog){: external}, are not supported by {{site.data.keyword.secrets-manager_short}}.
{: note}

## Events for secrets
{: #at-actions-secrets}

The following table lists the secret actions that generate an event.

| Action                                      | Description                                                    |
| ------------------------------------------- | -------------------------------------------------------------- |
| `secrets-manager.secret.create`             | Create a secret.                                               |
| `secrets-manager.secrets.list`              | List secrets.                                                  |
| `secrets-manager.secret.read`               | Get a secret.                                                  |
| `secrets-manager.secret.rotate`             | Rotate a secret.                                               |
| `secrets-manager.secret-credentials.delete` | Delete the Cloud IAM API key that is associated with a secret. |
| `secrets-manager.secret.delete`             | Delete a secret.                                               |
| `secrets-manager.secret-metadata.read`      | View the metadata of a secret.                                 |
| `secrets-manager.secret-metadata.update`    | Update the metadata of a secret.                               |
| `secrets-manager.secret-policies.set`       | Set secret policies.                                           |
| `secrets-manager.secret-policies.get`       | Get secret policies.                                           |
{: caption="Table 1. List of secret events" caption-side="top"}


## Events for secret groups
{: #at-actions-secret-groups}

The following table lists the secret group actions that generate an event.

| Action                                | Description                         |
| ------------------------------------- | ----------------------------------- |
| `secrets-manager.secret-group.create` | Create a secret group.              |
| `secrets-manager.secret-groups.list`  | List secret groups.                 |
| `secrets-manager.secret-group.read`   | View the details of a secret group. |
| `secrets-manager.secret-group.update` | Update a secret group.              |
| `secrets-manager.secret-group.delete` | Delete a secret group.              |
{: caption="Table 2. List of secret group events" caption-side="top"}


## Events for instance operations
{: #at-actions-instance-operations}

The following table lists the instance operation actions that generate an event.

| Action                                     | Description                      |
| ------------------------------------------ | -------------------------------- |
| `secrets-manager.instance.login`           | Log in to Vault.                 |
| `secrets-manager.secret-engine-config.set` | Set secret engine configuration. |
| `secrets-manager.secret-engine-config.get` | Get secret engine configuration. |
| `secrets-manager.endpoints.get`            | Get service instance endpoints.  |
{: caption="Table 3. List of instance operation events" caption-side="top"}


## Viewing events
{: #at-ui}





Events that are generated by an instance of the {{site.data.keyword.secrets-manager_short}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-launch#launch_cloud_ui).

