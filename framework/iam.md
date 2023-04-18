---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-18"

keywords: IAM access for Secrets Manager, permissions for Secrets Manager, identity and access management for Secrets Manager, roles for Secrets Manager, actions for Secrets Manager, assigning access for Secrets Manager

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

# Managing IAM access for {{site.data.keyword.secrets-manager_short}}
{: #iam}

Access to {{site.data.keyword.secrets-manager_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.secrets-manager_short}} service in your account must be assigned an access policy with an IAM role. The policy determines which actions a user can perform within the context of {{site.data.keyword.secrets-manager_short}}.
{: shortdesc} 

IAM access policies enable access to be granted at different levels. Some of the options include the following actions: 

- Access across all {{site.data.keyword.secrets-manager_short}} service instances in your account
- Access to an individual {{site.data.keyword.secrets-manager_short}} instance in your account
- Access to a specific resource within a {{site.data.keyword.secrets-manager_short}} instance, such resource type `secret-group`

## IAM roles and actions for {{site.data.keyword.secrets-manager_short}} 
{: #iam-roles-actions}

Review the following [platform and service roles](/docs/account?topic=account-userroles#platformroles) that you can use to assign access to {{site.data.keyword.secrets-manager_short}} service instances. Each role maps to specific {{site.data.keyword.secrets-manager_short}} actions that a user can complete within an account or an individual instance.

If a specific role and its actions don't fit the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles#custom-access-roles) and pick the actions to include.
{: tip}

| Role | Description |
| ----- | :----- |
| Viewer | As a viewer, you can view {{site.data.keyword.secrets-manager_short}} service instances, but you can't modify them. |
| Operator | As an operator, you can complete platform actions that are required to configure and operate {{site.data.keyword.secrets-manager_short}} service instances, such as the ability to view a {{site.data.keyword.secrets-manager_short}} dashboard. |
| Editor | As an editor, you can create, modify, and delete {{site.data.keyword.secrets-manager_short}} service instances, but you can't assign access policies to other users. |
| Administrator | As an administrator, you can complete all platform actions for {{site.data.keyword.secrets-manager_short}}, including the ability to assign access policies to other users. |
{: caption="Table 1. Platform roles - {{site.data.keyword.secrets-manager_short}}" caption-side="top"}
{: #platform-roles-table1}
{: tab-title="Platform roles"}
{: tab-group="secrets-manager"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the platform role name and the column headers identify the specific information available about each role."}
{: class="simple-tab-table"}
{: row-headers}

| Role | Description |
| ----- | :----- |
| Reader | As a reader, you can complete read-only actions within a {{site.data.keyword.secrets-manager_short}} service instance, such as the ability to view secrets and secret groups. Readers can access only the metadata that is associated with a secret. |
| SecretsReader | As a secrets reader, you can complete read-only actions, and you can also access the secret data that is associated with a secret. A secrets reader can't create secrets or modify the value of an existing secret. |
| Writer | As a writer, you have permissions beyond the secrets reader role, including the ability to create and edit secrets. Writers can't create secret groups, manage the rotation policies of a secret, or configure secrets engines. |
| Manager | As a manager, you have permissions beyond the writer role to complete privileged actions, such as the ability to manage secret groups, configure secrets engines, and manage secrets policies. |
{: caption="Table 1. Service roles - {{site.data.keyword.secrets-manager_short}}" caption-side="top"}
{: #service-roles-table1}
{: tab-title="Service roles"}
{: tab-group="secrets-manager"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the service role name and the column headers identify the specific information available about each role."}
{: class="simple-tab-table"}
{: row-headers}


| Action | Description | Roles |
| ----- | :----- | :----- |
| `secrets-manager.dashboard.view` | View the {{site.data.keyword.secrets-manager_short}} dashboard. | Operator, Editor, Administrator |
| `secrets-manager.secret-group.create` | Create secret groups. | Manager |
| `secrets-manager.secret-group.update` | Update a secret group. | Manager |
| `secrets-manager.secret-group.delete` | Delete a secret group. | Manager |
| `secrets-manager.secret-group.read` | View the details of a secret group. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret-groups.list` | List the secret groups in your instance. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret.create` | Create a secret. | Writer, Manager |
| `secrets-manager.secret.read` | Get the value of a secret. | SecretsReader, Writer, Manager |
| `secrets-manager.secret.delete` | Delete a secret. | Manager |
| `secrets-manager.secrets.list` | List the secrets in your instance. | Reader, SecretsReader, Writer, Manager | 
| `secrets-manager.secret-locks.create` | Create secret locks.   | Writer, Manager |
| `secrets-manager.secret-locks.delete` | Delete secret locks.  | Manager         |
| `secrets-manager.secrets-locks.list`  | List secret locks.     | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret-version-locks.create` | Create secret version locks. | Manager, Writer |
| `secrets-manager.secret-version-locks.list` | List secret version locks. | Manager, Reader, Writer, SecretsReader |
| `secrets-manager.secret-version-locks.delete` | Delete secret version locks. | Manager |
| `secrets-manager.secret-metadata.update` | Update the metadata of a secret. | Writer, Manager |
| `secrets-manager.secret-metadata.read` | View the metadata of a secret. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret-action.create` | Create a secret action. | Manager, Writer |
| `secrets-manager.secret-version.create` | Create a new secret version. | Manager, Writer |
| `secrets-manager.secret-version.read` | View the details of a secret version. | Manager, Writer, SecretsReader |
| `secrets-manager.secret-version-metadata.read` | View the metadata of a secret version. | Manager, Reader, Writer, SecretsReader |
| `secrets-manager.secret-version-action.create` | Create a secret version action. | Manager, Writer |
| `secrets-manager.configuration.create` | Create a new configuration. | Manager | 
| `secrets-manager.configuration-action.create` | Create a new configuration action. | Manager |
| `secrets-manager.configurations.list` | List configurations. | Manager, Reader, Writer |
| `secrets-manager.configuration.read` | View the details of a configuration. | Manager |
| `secrets-manager.configuration.update` | Update a configuration. | Manager |
| `secrets-manager.configuration.delete` | Delete a configuration. | Manager |
| `secrets-manager.secret-versions.list` | List secret versions. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.endpoints.view` | Get service instance endpoints. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.notifications-registration.create` | Create a registration with Event Notifications. | Manager |
| `secrets-manager.notifications-registration.read` | Get Event Notifications registration details. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.notifications-registration.delete` | Delete an Event Notifications registration. | Manager |
| `secrets-manager.notifications-registration.test` | Send a test event. | Reader, SecretsReader, Writer, Manager |
{: caption="Table 1. Service actions - {{site.data.keyword.secrets-manager_short}}" caption-side="top"}
{: #actions-table1}
{: tab-title="Actions"}
{: tab-group="secrets-manager"}
{: class="simple-tab-table"}
{: summary="Use the tab buttons to change the context of the table. This table provides the available actions for the service, descriptions of each, and the roles that each action are mapped to."}


## Assigning access to {{site.data.keyword.secrets-manager_short}}
{: #iam-assign-access}

You can use the {{site.data.keyword.cloud_notm}} console, CLI, or APIs to assign different levels of access to {{site.data.keyword.secrets-manager_short}} resources in your account. You can assign access at the instance level, or you can narrow access to a secret group that contains one or more secrets. For more information, see [Assigning access to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-assign-access).

To learn about using the {{site.data.keyword.cloud_notm}} CLI to assign access, check out the [{{site.data.keyword.cloud_notm}} CLI reference](/docs/cli?topic=cli-ibmcloud_commands_iam). When you create an access policy for {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.cloud_notm}} CLI or APIs, use `secrets-manager` for the service name in the CLI command or API call.
{: tip}
