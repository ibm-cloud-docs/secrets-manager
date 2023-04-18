---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-18"

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
{:terraform: .ph data-hd-interface="terraform"}
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

# Auditing events for {{site.data.keyword.secrets-manager_short}}
{: #at-events}

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records service and user initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard.

For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

Audit devices that you can enable with Vault, such as the [`syslog` audit device](https://developer.hashicorp.com/vault/docs/audit/syslog){: external}, are not supported by {{site.data.keyword.secrets-manager_short}}.
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
| `secrets-manager.secret-credentials.delete` | Delete the {{site.data.keyword.cloud_notm}} API key that is associated with a secret. |
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


## Events for secret locks
{: #at-actions-secret-locks}

The following table lists the secret lock actions that generate an event.

| Action                                | Description                         |
| ------------------------------------- | ----------------------------------- |
| `secrets-manager.secret-locks.create`  | Create a secret lock.               |
| `secrets-manager.secret-locks.list`   | List secret locks.                  | 
| `secrets-manager.secret-locks.delete`  | Delete a secret lock.               |
| `secrets-manager.secrets-locks.list`          | List secret locks. | 
| `secrets-manager.secret-version-locks.create` | Create secret version locks. |
| `secrets-manager.secret-version-locks.list` | List secret version locks. |
| `secrets-manager.secret-version-locks.delete` | Delete secret version locks. | 
{: caption="Table 2. List of secret lock events" caption-side="top"}


## Events for instance operations
{: #at-actions-instance-operations}

The following table lists the instance operation actions that generate an event.

## Events for instance operations
{: #at-configuration-instance-operations}

The following table lists the instance operation actions that generate an event.

| Action                                     | Description                      |
| ------------------------------------------ | -------------------------------- |
| `secrets-manager.instance.login`           | Log in to Vault.                 |
| `secrets-manager.configuration.create` | Create a new configuration. |
| `secrets-manager.configuration-action.create` | Create a new configuration action. |
| `secrets-manager.configurations.list` | List configurations. |
| `secrets-manager.configuration.read` | View the details of a configuration. | 
| `secrets-manager.configuration.update` | Update a configuration. |
| `secrets-manager.configuration.delete` | Delete a configuration. |
| `secrets-manager.endpoints.view`            | Get service instance endpoints.  |
| `secrets-manager.notifications-registration.create` | Create a registration with Event Notifications. | Manager |
| `secrets-manager.notifications-registration.read` | Get Event Notifications registration details. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.notifications-registration.delete` | Delete an Event Notifications registration. | Manager |
| `secrets-manager.notifications-registration.test` | Send a test event. | Reader, SecretsReader, Writer, Manager |
{: caption="Table 3. List of instance operation events" caption-side="top"}


## Viewing events
{: #at-ui}

Events that are generated by an instance of the {{site.data.keyword.secrets-manager_short}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the UI through the IBM Cloud UI](/docs/activity-tracker?topic=activity-tracker-launch#launch_cloud_ui).

## Analyzing events
{: #at-analyze}

Successful events that are generated by an instance of the {{site.data.keyword.secrets-manager_short}} service contain various fields that can help you to identify the initiator, the target resource, and the outcome of each completed action in your instance. For more information about the components that make up an event, see the [{{site.data.keyword.at_full_notm}} documentation](/docs/activity-tracker?topic=activity-tracker-event).

Due to the sensitivity of secrets, when an {{site.data.keyword.at_full_notm}} event is generated as a result of an API call to the {{site.data.keyword.secrets-manager_short}} service, the generated event does not include the actual contents of a secret. Sensitive data, such as an API key or password, is replaced with identifying information about the secret only, or it is omitted from generated events altogether.
{: note}


