---

copyright:
  years: 2020, 2024
lastupdated: "2024-12-18"

keywords: secrets management best practice, managing secrets, secrets strategy, secrets best practices, organizing secrets, assigning access to secrets

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

# Best practices for organizing secrets and assigning access
{: #best-practices-organize-secrets}

With {{site.data.keyword.secrets-manager_full}}, you can design a strategy for organizing and assigning access to your secrets and sensitive data. Review the following suggested guidelines for implementing best practices around your secrets management.
{: shortdesc}

## Limit Access to Your Secrets
{: #limit-access-to-secrets}

To ensure the security of your organization's secrets stored in {{site.data.keyword.secrets-manager_short}}, restrict access for non-administrative users and applications.

- Review your IAM settings to confirm that permissive access to the {{site.data.keyword.secrets-manager_short}} service is not allowed.
- Grant access to secrets for non-administrative users and applications only for the duration necessary to complete their tasks, and promptly revoke access when it is no longer required.
- When assigning access permissions, adhere to the principle of least privilege. Limit access to specific {{site.data.keyword.secrets-manager_short}} instances or secret groups, and assign only the minimum service role necessary for the required operations. Additionally, you can [configure custom IAM roles](/docs/account?topic=account-custom-roles) to restrict access to a specific set of {{site.data.keyword.secrets-manager_short}} actions tailored to a workload's needs.

## Review the hierarchy of resources in your account
{: #best-practices-access-hierarchy}

As you use {{site.data.keyword.secrets-manager_short}} to design your secrets management strategy, consider the hierarchy of resources in your {{site.data.keyword.cloud_notm}} account and how they inherit the access policies that you assign to them. Determine ahead of time which users or service IDs require access to manage secrets. By defining a circle of trust, you help to reduce the risk of compromised credentials.

1. Use resource groups to organize the {{site.data.keyword.secrets-manager_short}} instances that you want a group of developers in your account to use. To create a resource group, go to **Manage > Account > Account resources > Resource groups**.

    If the resource groups in your account are organized by project environment level, for example development, test, and production, create a {{site.data.keyword.secrets-manager_short}} instance for each resource group so that you can separate your secrets according to each phase of development. For more information about using resource groups, see [What makes a good resource group strategy?](/docs/account?topic=account-account_setup#resource-group-strategy)

2. Use access groups to organize the users and services IDs that require the same level of access to a {{site.data.keyword.secrets-manager_short}} instance. To create an access group, go to **Manage > Access (IAM) > Access groups**.

    For example, you can create a single access group, such as `Admin-Group`, for the users or service IDs that require administrator access to resources in a resource group, including your {{site.data.keyword.secrets-manager_short}} instance. This way, you can assign a single policy to the access group rather than an individual policy for each user or service ID. For more information about using access groups, see [What makes a good access group strategy?](/docs/account?topic=account-account_setup#accessgroup_strategy)

## Organize secrets in your {{site.data.keyword.secrets-manager_short}} instance by using secret groups
{: #best-practices-secret-groups}

Use secret groups to narrow the scope of access to specific secrets at an instance level.

1. Create secret groups to assign even more granular access to a group of secrets, such as those that map to a specific application. To create a secret group, go to the **{{site.data.keyword.secrets-manager_short}} UI > Secret groups > Create**.

    If you're familiar with using resource groups, think of organizing secrets in a secret group similar to the way that you organize resources in resource groups. As an admin, you create secret groups to control access to secrets in your instance. For example, you can create a secret group that contains only the secrets that are used to authenticate to a specific resource or system. Then, you assign the secret group to an IAM access group so that only your selected users or service IDs are able to access those secrets. For more information about managing secret groups, see [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).

    A default secret group is created for you when you create a {{site.data.keyword.secrets-manager_short}} instance. It's important that you create your secret groups first because you can't change the assignment of secrets after you create them. If you accidentally assign a secret to the wrong secret group, or if you don't want a secret to belong to the default secret group, delete the secret and create a new one.
    {: tip}

2. Optionally, use secret groups to allow privileged access to specific resources in your account.

    Secret groups can be used to grant direct access to resources that otherwise wouldn't be possible through IAM. For example, assume that `User A` has no access to `Service A` in IAM. If you create an IAM access policy that assigns `User A` to `Secret Group A`, and `Secret Group A` contains an [IAM credential](/docs/secrets-manager?topic=secrets-manager-iam-credentials) with a service ID that gives access to `Service A`, then you grant `User A` access to `Service A`. In this scenario, `Secret Group A` becomes a gateway to `Service A`, even if a restriction exists in IAM. Keep in mind that with this scenario it is possible to grant access to a resource unintentionally. Review your configuration carefully to ensure that your secret group assignments do not override your IAM access policies accidentally.

3. Audit your secret groups regularly and remove them when they're no longer needed.

    Grant only the minimum access that is required, and delete a secret group when it is no longer needed.

    To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](/docs/secrets-manager?topic=secrets-manager-delete-secrets) that are part of the group.
    {: note}


## Track your related secrets by adding labels
{: #best-practices-labels}

Add labels so that you can further search by and categorize the secrets in your instance. When you use a consistent labeling schema, you can easily group similar secrets together.

1. Label your secrets by using a consistent schema, such as creating labels to differentiate which secrets are used for a specific purpose. To add labels by using the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** page, and then select a secret to edits its details.
