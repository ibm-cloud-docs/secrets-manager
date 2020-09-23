---

copyright:
  years: 2020
lastupdated: "2020-09-23"

keywords: assign access for Secrets Manager, secret group access, assign access for all secrets, grant access, add users

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

# Assigning access
{: #assign-access}

You can enable different levels of access to {{site.data.keyword.secrets-manager_full}} resources in your {{site.data.keyword.cloud_notm}} account by creating and modifying {{site.data.keyword.cloud_notm}} IAM access policies.
{: shortdesc}

As an account admin, determine an access policy type for users, service IDs, and access groups based on your internal access control requirements. For example, if you want to grant user access to {{site.data.keyword.secrets-manager_short}} at the smallest scope available, you can [assign access to a secret group](#assign-access-secret-group) in an instance.

To learn more about suggested guidelines for assigning access to secrets, check out [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).
{: tip}

## Before you begin
{: #before-access}

Before you get started, be sure that you have [**Administrator** platform access](/docs/account?topic=account-assign-access-resources#assign_new_access) to manage access policies for others in your account.

## Assigning access to a {{site.data.keyword.secrets-manager_short}} instance
{: #assign-access-instance}

You can grant access to the secrets within a {{site.data.keyword.secrets-manager_short}} instance by using the {{site.data.keyword.cloud_notm}} console.

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > Access groups**.
   
    You can use access groups to organize the users and services IDs that require the same level of access to a {{site.data.keyword.secrets-manager_short}} instance. If you need to create a new access group, click **Create** and provide a name and description. 
2. Click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for the access group that you want to manage.
3. Click **Assign access**.
4. Select **IAM services**. 
5. From the list of services, select **{{site.data.keyword.secrets-manager_short}}**.
6. From the list of options, select a region and a {{site.data.keyword.secrets-manager_short}} service instance.

    If you choose not to provide a specific instance, access is assigned for all instances of the service within the region that you selected. If you choose not to select a region, access is granted for all instances of the service in your account.
7. Choose a combination of [platform and service access roles](/docs/secrets-manager?topic=secrets-manager-iam) to assign access for access group.
8. Click **Add**.
9. Review your selections and click **Assign**.

  Now you can add users and service IDs to the access group so that you can assign access to {{site.data.keyword.secrets-manager_short}} with a single access policy. For more information, see [Setting up access groups](/docs/account?topic=account-groups).

## Assigning access to a secret group
{: #assign-access-secret-group}

You can further narrow the scope of access to secrets in your instance by creating and managing [secret groups](#x9968962){: term}.

[After you create a secret group for your instance](/docs/secrets-manager?topic=secrets-manager-secret-groups#create-secret-groups), you can use the **Access (IAM)** section of the console to manage its access.

1. Retrieve the ID of the secret group that you want to manage.

   1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List** to view a list of your resources.
   2. Select your instance of {{site.data.keyword.secrets-manager_short}}.
   3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secret groups** page.
   4. Copy the ID of the secret group that you want to manage.
   5. From the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg), click **Manage access** to go to the IAM section of the console.
   
2. Manage access for your secret group by updating its access policy.
   
   1. Go to **Manage > Access (IAM) > Access Groups**.
   2. Click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for the access group that you want to manage.
   3. Click **Assign access**.
   4. Select **IAM services**.
   5. From the list of services, select **{{site.data.keyword.secrets-manager_short}}**.
   6. From the list of options, select a region and a {{site.data.keyword.secrets-manager_short}} instance.
   7. In the **Resource Type** field, enter `secret-group`.
   8. In the **Resource** field, enter the ID that was assigned to your secret group by the {{site.data.keyword.secrets-manager_short}} service.
   9. Choose a combination of [access roles](/docs/secrets-manager?topic=secrets-manager-iam) to assign.
   10. Click **Add**.
   11. Review your selections and click **Assign**.

