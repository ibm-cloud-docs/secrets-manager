---

copyright:
  years: 2021
lastupdated: "2021-01-20"

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# Managing IAM access for {{site.data.keyword.secrets-manager_short}}
{: #iam}



Access to {{site.data.keyword.secrets-manager_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.secrets-manager_short}} service in your account must be assigned an access policy with an IAM role. Review the following roles, actions, and more to help determine the best way to assign access to {{site.data.keyword.secrets-manager_short}}.
{: shortdesc} 

The access policy that you assign users in your account determines what actions a user can perform within the context of the service or specific instance that you select. The allowable actions are customized and defined by {{site.data.keyword.secrets-manager_short}} as operations that are allowed to be performed on the service. Each action is mapped to an IAM platform or service role that you can assign to a user.

If a specific role and its actions don't fit the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles#custom-access-roles) and pick the actions to include.
{: tip}

IAM access policies enable access to be granted at different levels. Some of the options include the following: 

- Access across all instances of the service in your account
- Access to an individual service instance in your account
- Access to a specific resource within an instance, such as specific secret groups

Review the following tables that outline what types of tasks each role allows for when you're working with the {{site.data.keyword.secrets-manager_short}} service.

When you assign policies to users for this service, use `secrets-manager` for the service name in the CLI command or API call.
{: important}

The following table describes the types of tasks that can be completed with each platform management role assigned. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access to the service, create or delete instances, and bind instances to applications.

For information about the exact actions that are mapped to each role, see [{{site.data.keyword.secrets-manager_short}}](/docs/account?topic=account-iam-service-roles-actions#secrets-manager).
{: tip}



| Platform role | Description of actions | 
|----------------|------------------------|
| Viewer         | As a viewer, you can view instances of {{site.data.keyword.secrets-manager_short}}, but you can't modify them. |
| Operator       | As an operator, you can perform platform actions that are required to configure and operate {{site.data.keyword.secrets-manager_short}} service instances, such as viewing the service's dashboard. |
| Editor         | As an editor, you can create, modify, and delete instances of {{site.data.keyword.secrets-manager_short}}, but you can't assign access policies for others. | 
| Administrator  | As an administrator, you can perform all platform actions based on the resource this role is being assigned, including the ability to assign access policies for others. |
{: row-headers}
{: caption="Table 1. IAM platform roles" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the platform management role. The second column is a description of the actions in the service that the platform management role permits."}


The following table describes the types tasks that can be completed when each service access role is assigned. Service access roles enable users access to {{site.data.keyword.secrets-manager_short}} and the ability to call the {{site.data.keyword.secrets-manager_short}} API.

| Service role | Description of actions | 
|---------------------|------------------------|
| Reader             | As a reader, you can perform read-only actions within {{site.data.keyword.secrets-manager_short}}, such as viewing service-specific resources. Readers can't access the payload of a secret.  |
| SecretsReader | As a secrets reader, you can perform read-only actions, and you can access the payload of a secret. A secrets reader can't create secrets or modify the value of an existing secret. |
| Writer            | As a writer, you have permissions beyond the secrets reader role, including the ability to create and edit service-specific resources. Writers can't create secret groups, manage the rotation policies of a secret, or configure secret engines.           |
| Manager             | As a manager, you have permissions beyond the writer role to complete privileged actions, such as managing secret groups, configuring secret engines, and managing secret policies.         | 
{: row-headers}
{: caption="Table 2. IAM service access role descriptions" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the service access role. The second column is a description of the actions in the service that the service access role permits."}

For more information about assigning IAM access, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).

