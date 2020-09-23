---

copyright:
  years: 2020
lastupdated: "2020-09-23"

keywords: IAM access for Secrets Manager, permissions for Secrets Manager, identity and access management for Secrets Manager, roles for Secrets Manager, actions for Secrets Manager, assigning access for Secrets Manager

subcollection: secrets-manager

---

{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:notoc: .notoc}
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

# Managing access for {{site.data.keyword.secrets-manager_short}}
{: #iam}

Access to {{site.data.keyword.secrets-manager_short}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.secrets-manager_short}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following: 

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance such as specific secret groups

After you define the scope of the access policy, you assign a role, which determines the user's level of access. Review the following tables that outline what actions each role allows within the {{site.data.keyword.secrets-manager_short}} service.

## Platform roles
{: #secret-manager-platform-roles}

Use platform roles to grant access to {{site.data.keyword.secrets-manager_short}} at the platform level, such as the ability to create or delete instances or assign user access to {{site.data.keyword.secrets-manager_short}} in your IBM Cloud account.

The following table shows how platform roles map to {{site.data.keyword.secrets-manager_short}} actions.

| Role | Description of actions | 
|----------------|------------------------|
| Viewer         | View instances of {{site.data.keyword.secrets-manager_short}}. |
| Operator       | View instances of {{site.data.keyword.secrets-manager_short}}. |
| Editor         | Create, update, or delete instances of {{site.data.keyword.secrets-manager_short}}. | 
| Administrator  | Invite new users and manage access policies. |
{: row-headers}
{: caption="Table 1. IAM platform roles and actions" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the platform management role. The second column is a description of the actions in the service that the platform management role permits."}

## Service roles
{: #secret-manager-service-roles}

Use service roles to grant access to {{site.data.keyword.secrets-manager_short}} at the instance level, such as the ability to view, create, or delete secrets and secret groups. 

The following table shows how service roles map to {{site.data.keyword.secrets-manager_short}} actions.

| Role | Description of actions | 
|----------------|------------------------|
| Reader         | List the available secrets in your instance. <br> View the metadata of a secret. </br> List the secret groups that are available in your instance. </br>View the details of a secret group. </br> View the service endpoints. </br> List the available versions of a secret. |
| Writer         | All of the actions that the Reader role allows. </br> Create and rotate secrets. </br> View the details of a secret. </br> Update the metadata of a secret. </br> |
| Manager        | All of the actions that the Writer role allows. </br> Delete the Cloud IAM API key that is associated with a secret. </br> Delete a secret. </br> Set and view secret policies. </br> Create, update, and delete a secret group. </br> Set and view your secret engine configuration.| 
{: row-headers}
{: caption="Table 1. IAM platform roles and actions" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the platform management role. The second column is a description of the actions in the service that the platform management role permits."}

For information about assigning user roles in the console, see [Managing access to resources](/docs/account?topic=account-assign-access-resources).


