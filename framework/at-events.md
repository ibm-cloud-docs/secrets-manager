---

copyright:
  years: 2020, 2025
lastupdated: "2025-04-30"

keywords: activity tracking events for Secrets Manager, events, Secrets Manager actions

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for {{site.data.keyword.secrets-manager_short}}
{: #at_events}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.secrets-manager_full}}, generate activity tracking events. 
{: shortdesc}

Audit devices that you can enable with Vault, such as the [`syslog` audit device](https://developer.hashicorp.com/vault/docs/audit/syslog){: external}, are not supported by {{site.data.keyword.secrets-manager_short}}.
{: note}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.logs_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-about-cl).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed to an {{site.data.keyword.logs_full_notm}} instance.

## Locations where activity tracking events are generated
{: #at-locations}

{{site.data.keyword.secrets-manager_short}} sends activity tracking events to {{site.data.keyword.logs_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #logs-table-1}
{: tab-title="Americas"}
{: tab-group="logs"}
{: class="simple-tab-table"}
{: row-headers}

| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | Chennai (`in-che`) |
|---------------------|------------------|------------------|--------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [No]{: tag-red} |
{: caption="Regions where activity tracking events are sent in Asia Pacific locations" caption-side="top"}
{: #logs-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="logs"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #atracker-table-3}
{: tab-title="Europe"}
{: tab-group="logs"}
{: class="simple-tab-table"}
{: row-headers}

## Viewing activity tracking events for {{site.data.keyword.secrets-manager_short}}
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch)

## Analyzing events
{: #at-analyze}

Successful events that are generated by an instance of the {{site.data.keyword.secrets-manager_short}} service contain various fields that can help you to identify the initiator, the target resource, and the outcome of each completed action in your instance.

Due to the sensitivity of secrets, when an event is generated as a result of an API call to the {{site.data.keyword.secrets-manager_short}} service, the generated event does not include the actual contents of a secret. Sensitive data, such as an API key or password, is replaced with identifying information about the secret only, or it is omitted from generated events altogether.
{: note}

You can create views and alerts from all of your {{site.data.keyword.secrets-manager_short}} instances, or from a specific instance.  
To target a specific instance, replace `host:secrets-manager` with `app:{INSTANCE_CRN}`.

### Query for finding all create secret actions:
{: #query-all-at}

Run the following query to find all create secret actions.

```sh
host:secrets-manager action:secrets-manager.secret.create
```
{: codeblock}

The action value can be replaced with any other applicable action.
{: note}


### Query for finding unauthorized access attempts
{: #query-specific-at}

To see unauthorized access attempts, run the following query.

```sh
host:secrets-manager reason.reasonType:Unauthorized
```
{: codeblock}


## Understanding generated events
{: #gen-events}

The following events are generated for each category.

### Events for secrets
{: #at-actions-secrets}

The following table lists the secret actions that generate an event.

| Action                                           | Description                                                    |
| ------------------------------------------------ | -------------------------------------------------------------- |
| `secrets-manager.secret.create`                  | Create a secret.                                               |
| `secrets-manager.secrets.list`                   | List secrets.                                                  |
| `secrets-manager.secret.read`                    | Get a secret.                                                  |
| `secrets-manager.secret.delete`                  | Delete a secret.                                               |
| `secrets-manager.secret-metadata.read`           | View the metadata of a secret.                                 |
| `secrets-manager.secret-metadata.update`         | Update the metadata of a secret.                               |
| `secrets-manager.secret-action.create`	         | Create a secret action                                         |
| `secrets-manager.secret-versions.list`           | List versions of a secret                                      |
| `secrets-manager.secret-version.create`	         | Create a new secret version                                    |
| `secrets-manager.secret-version.read`	           | Get a secret version                                           |
| `secrets-manager.secret-version-metadata.update` |	Update the metadata of a secret version                       |
| `secrets-manager.secret-version-metadata.read`   |	Get the metadata of a secret version                          |
| `secrets-manager.secret-version-data.delete`     |	Delete the data of a secret version                           |
| `secrets-manager.secret-version-action.create`   |	Create a version action                                       |
{: caption="List of secret events" caption-side="top"}

### Events for secret groups
{: #at-actions-secret-groups}

The following table lists the secret group actions that generate an event.

| Action                                | Description                         |
| ------------------------------------- | ----------------------------------- |
| `secrets-manager.secret-group.create` | Create a secret group.              |
| `secrets-manager.secret-groups.list`  | List secret groups.                 |
| `secrets-manager.secret-group.read`   | View the details of a secret group. |
| `secrets-manager.secret-group.update` | Update a secret group.              |
| `secrets-manager.secret-group.delete` | Delete a secret group.              |
{: caption="List of secret group events" caption-side="top"}

### Events for secret locks
{: #at-actions-secret-locks}

The following table lists the secret lock actions that generate an event.

| Action                                         | Description                         |
| ---------------------------------------------- | ----------------------------------- |
| `secrets-manager.secret-locks.create`          | Create a secret lock.               |
| `secrets-manager.secret-locks.list`            | List secrets and their locks        | 
| `secrets-manager.secret-locks.delete`          | Delete a secret lock.               |
| `secrets-manager.secrets-locks.list`           | List secret locks.                  | 
| `secrets-manager.secret-version-locks.create`  | Create secret version locks.        |
| `secrets-manager.secret-version-locks.list`    | List secret version locks.          |
| `secrets-manager.secret-version-locks.delete`  | Delete secret version locks.        | 
{: caption="List of secret lock events" caption-side="top"}

### Events for instance operations
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
{: caption="List of instance operation events" caption-side="top"}
