---

copyright:
  years: 2020
lastupdated: "2020-09-24"

keywords: secret groups, assign secret access, iam roles, secrets policies, organize secrets

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

# Organizing your secrets
{: #secret-groups}

When you work with {{site.data.keyword.secrets-manager_full}}, you can create groups to organize your secrets and control who on your team has access to them. Then, if you don't need them anymore, you can delete the groups.
{: shortdesc}

Similar to the way that [resource groups](#x2161955){: term} help to ensure correct policy enforcement at the platform level, you can create secret groups at the instance level to organize secrets.

![The image shows two examples of a secret group and how they're mapped to access groups. One where the reader role is assigned and one where the manager role is assigned. The content is explained fully in the surrounding text.](../images/secret-group.svg){: caption="Figure 1. Assigning access to secret groups" caption-side="bottom"}

As shown in the previous image, users with *Reader* access to a secret group can see that the group exists and understand which secrets are assigned to it. Users with *Writer* access can view and edit the secret group and secrets themselves. By design, the default secret group inherits all of the same permissions that are set for the instance.

You can choose to group your secrets by phase of development, specific to the type of roles that people on your team have, or in any way that might help you. Each secret can be mapped to one group only and the mapping occurs at the time of secret creation. 

To learn about the suggested guidelines for using secret groups, check out [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).
{: tip}

## Before you begin
{: #before-secret-groups}

Before you begin, be sure that you have the required level of access. To create and manage secret groups, you need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam).


## Creating secret groups
{: #create-secret-groups}

You can create secret groups by using the {{site.data.keyword.secrets-manager_short}} console or the APIs.

### Creating secret groups with the console
{: #create-group-ui}

You can create secret groups by using the console. You can also create a secret group during the process of adding or creating a secret.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secret groups** page.
4. Click **Add**.
5. Enter a name and description for your group.
6. Click **Create**.
7. Optional: Assign your secret group an [IAM policy](/docs/secrets-manager?topic=secrets-manager-assign-access).


### Creating secret groups with the API
{: #create-group-api}

You can create secret groups by using the {{site.data.keyword.secrets-manager_short}} APIs.

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secret_groups" \
  -H "Authorization: Bearer {IAM_token}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{ 
    "metadata": { 
      "collection_type": "application/vnd.ibm.secrets-manager.secret.group+json", 
      "collection_total": 1 
      }, 
      "resources": [ 
        { 
          "type": "application/vnd.ibm.secrets-manager.secret.group+json",
          "name": "example-secret-group", 
          "description": "Extended description for my secret group."
        } 
      ] 
    }' 
```
{: pre}


## Deleting secret groups
{: #delete-groups}

If you no longer need to use a group, you can delete it by using the console or the APIs.

To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](/docs/secrets-manager?topic=secrets-manager-delete-secrets) that are part of the group.
{: note}


### Deleting secret groups with the console
{: #delete-group-ui}

You can delete secret groups by using the console.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the navigation, click **Secret groups**.
4. In the row for the secret group that you want to delete, click the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg).
5. Click **Delete group**.
6. Click **Delete**.

### Deleting secret groups with the API
{: #delete-group-api}

You can delete secret groups by using the {{site.data.keyword.secrets-manager_short}} APIs.

```bash
curl -X DELETE "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/secret_groups/{id}" \
  -H "Authorization: Bearer {IAM_token}" 
  -H "Accept: application/json" 
```
{: pre}

