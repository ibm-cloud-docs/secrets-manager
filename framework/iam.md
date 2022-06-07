---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-07"

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

---

copyright:
  years:  2017, 2022
lastupdated: "2022-05-13"

keywords: IAM access for _servicename_, permissions for _servicename_, identity and access management for _servicename_, roles for _servicename_, actions for _servicename_, assigning access for _servicename_

subcollection: _your-subcollection_

---

{{site.data.keyword.attribute-definition-list}}

_Include this file in the **How to** nav group of your toc.yaml file_

# Managing IAM access for _servicename_
{: #iam-docs-template}

Access to {{site.data.keyword.secrets-manager_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.secrets-manager_short}} service in your account must be assigned an access policy with an IAM role. Review the following roles, actions, and more to help determine the best way to assign access to {{site.data.keyword.secrets-manager_short}}. 
{: shortdesc}

The access policy that you assign users in your account determines what actions a user can perform within the context of the service or specific instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.secrets-manager_short}} as operations that are allowed to be performed on the service. Each action is mapped to an IAM platform or service role that you can assign to a user.

If a specific role and its actions don't fit the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles#custom-access-roles) and pick the actions to include.
{: tip}

IAM access policies enable access to be granted at different levels. Some of the options include the following: 

* Access across all instances of the service in your account
* Access to an individual service instance in your account 
* Access to a specific resource within an instance, _such as resource type `bucket`_ 

Review the following tables that outline what types of tasks each role allows for when you're working with the {{site.data.keyword.secrets-manager_short}} service. Platform management roles enable users to-perform tasks on service resources at the platform level, for example, assign user access to the service, create or delete instances, and bind instances to applications. Service access roles enable users access to {{site.data.keyword.secrets-manager_short}} and the ability to call the {{site.data.keyword.secrets-manager_short}}'s API. For information about the exact actions that are mapped to each role, see [{{site.data.keyword.secrets-manager_short}}](/docs/account?topic=account-iam-service-roles-actions#secrets-manager).


| Platform role |  Description of actions | 
|---------------|-------------------------|
| Viewer | As a viewer, you can view {{site.data.keyword.secrets-manager_short}} service instances, but you can't modify them. |
| Operator | As an operator, you can complete platform actions that are required to configure and operate {{site.data.keyword.secrets-manager_short}} service instances, such as the ability to view a {{site.data.keyword.secrets-manager_short}} dashboard. |
| Editor | As an editor, you can create, modify, and delete {{site.data.keyword.secrets-manager_short}} service instances, but you can't assign access policies to other users. |
| Administrator | As an administrator, you can complete all platform actions for {{site.data.keyword.secrets-manager_short}}, including the ability to assign access policies to other users. |
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 1. IAM platform roles" caption-side="bottom"}
{: #iamrolesplatform}
{: tab-title="Platform roles"}
{: tab-group="IAM"}

| Service role |  Description of actions | 
|--------------|------------------------|
| Reader | As a reader, you can complete read-only actions within a {{site.data.keyword.secrets-manager_short}} service instance, such as the ability to view secrets and secret groups. Readers can access only the metadata that is associated with a secret. |
| SecretsReader | As a secrets reader, you can complete read-only actions, and you can also access the secret data that is associated with a secret. A secrets reader can't create secrets or modify the value of an existing secret. |
| Writer | As a writer, you have permissions beyond the secrets reader role, including the ability to create and edit secrets. Writers can't create secret groups, manage the rotation policies of a secret, or configure secrets engines. |
| Manager | As a manager, you have permissions beyond the writer role to complete privileged actions, such as the ability to manage secret groups, configure secrets engines, and manage secrets policies. |
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 1. IAM service access roles" caption-side="bottom"}
{: #iamrolesservice}
{: tab-title="Service roles"}
{: tab-group="IAM"}

## Assigning access to {{site.data.keyword.secrets-manager_short}} in the console
{: #assign-access-console}
{: ui}

There are two common ways to assign access in the console:

* Access policies per user. You can manage access policies per user from the **Manage** > **Access (IAM)** > **Users** page in the console. For information about the steps to assign IAM access, see [Managing access to resources](https://cloud.ibm.com/docs/account?topic=account-assign-access-resources&interface=ui#access-resources-console).
* Access groups. Access groups are used to streamline access management by assigning access to a group once, then you can add or remove users as needed from the group to control their access. You manage access groups and their access from the **Manage** > **Access (IAM)** > **Access groups** page in the console. For more information, see [Assigning access to a group in the console](/docs/account?topic=account-groups&interface=ui#access_ag).


## Assigning access to {{site.data.keyword.secrets-manager_short}} in the CLI
{: #assign-access-cli}
{: cli}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access ro resources by using the CLI](/docs/account?topic=account-assign-access-resources&interface=cli#access-resources-cli). The following example shows a command for assigning the `SecretsReader` role for {{site.data.keyword.secrets-manager_short}}:

Use `secrets-manager` for the service name. Also, use quotations around role names that are more than one word like the example here.
{: tip}


 

```bash
ibmcloud iam user-policy-create USER@EXAMPLE.COM --service-name secrets-manager --roles "SecretsReader"
```
{: pre}

## Assigning access to {{site.data.keyword.secrets-manager_short}} by using the API
{: #assign-access-api}
{: api}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the API](/docs/account?topic=account-assign-access-resources&interface=api) or the [Create a policy API docs](/apidocs/iam-policy-management#create-policy). Role cloud resource names (CRN) in the following table are used to assign access with the API.


| Role name | Role CRN | 
|---------------|-----------------|
| Viewer                 | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Viewer`        |
| Operator               | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Operator`      | 
| Editor                 | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Editor`        | 
| Administrator          | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Administrator` | 
| Reader         | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Reader`        |
| SecretsReader | `crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader` |
| Writer         | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Writer`        | 
| Manager        | `crn:v1:bluemix:public:secrets-manager::::serviceRole:Manager`       |  
{: caption="Table 2. Role ID values for API use" caption-side="bottom"}

 

The following example is for assigning the `SecretsReader` role for Secrets Manager:

Use `secrets-manager` for the service name, and refer to the Role ID values table to ensure that you're using the correct value for the CRN.
{: tip}


```curl 
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "type": "access",
  "description": "SecretsReader role for Secrets Manager",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "IBMid-123453user"
        }
      ]
    }'
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "secrets-manager"
        }
      ]
    }
  ]
}
```
{: curl}
{: codeblock}

```java
SubjectAttribute subjectAttribute = new SubjectAttribute.Builder()
      .name("iam_id")
      .value("IBMid-123453user")
      .build();

PolicySubject policySubjects = new PolicySubject.Builder()
      .addAttributes(subjectAttribute)
      .build();

PolicyRole policyRoles = new PolicyRole.Builder()
      .roleId("crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader")
      .build();

ResourceAttribute accountIdResourceAttribute = new ResourceAttribute.Builder()
      .name("accountId")
      .value("ACCOUNT_ID")
      .operator("stringEquals")
      .build();

ResourceAttribute serviceNameResourceAttribute = new ResourceAttribute.Builder()
      .name("serviceName")
      .value("secrets-manager")
      .operator("stringEquals")
      .build();

PolicyResource policyResources = new PolicyResource.Builder()
      .addAttributes(accountIdResourceAttribute)
      .addAttributes(serviceNameResourceAttribute)
      .build();

CreatePolicyOptions options = new CreatePolicyOptions.Builder()
      .type("access")
      .subjects(Arrays.asList(policySubjects))
      .roles(Arrays.asList(policyRoles))
      .resources(Arrays.asList(policyResources))
      .build();

Response<Policy> response = service.createPolicy(options).execute();
Policy policy = response.getResult();

System.out.println(policy);
```
{: java}
{: codeblock}
   
```javascript
const policySubjects = [
  {
    attributes: [
      {
        name: 'iam_id',
        value: 'IBMid-123453user',
      },
    ],
  },
];
const policyRoles = [
  {
    role_id: 'crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader',
  },
];
const accountIdResourceAttribute = {
  name: 'accountId',
  value: 'ACCOUNT_ID',
  operator: 'stringEquals',
};
const serviceNameResourceAttribute = {
  name: 'serviceName',
  value: 'secrets-manager',
  operator: 'stringEquals',
};
const policyResources = [
  {
    attributes: [accountIdResourceAttribute, serviceNameResourceAttribute]
  },
];
const params = {
  type: 'access',
  subjects: policySubjects,
  roles: policyRoles,
  resources: policyResources,
};

iamPolicyManagementService.createPolicy(params)
  .then(res => {
    examplePolicyId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: javascript}
{: codeblock}
   
```python
policy_subjects = PolicySubject(
  attributes=[SubjectAttribute(name='iam_id', value='IBMid-123453user')])
policy_roles = PolicyRole(
  role_id='crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader')
account_id_resource_attribute = ResourceAttribute(
  name='accountId', value='ACCOUNT_ID')
service_name_resource_attribute = ResourceAttribute(
  name='serviceName', value='secrets-manager')
policy_resources = PolicyResource(
  attributes=[account_id_resource_attribute,
        service_name_resource_attribute])

policy = iam_policy_management_service.create_policy(
  type='access',
  subjects=[policy_subjects],
  roles=[policy_roles],
  resources=[policy_resources]
).get_result()

print(json.dumps(policy, indent=2))
```
{: python}
{: codeblock}
   
```go
subjectAttribute := &iampolicymanagementv1.SubjectAttribute{
  Name:  core.StringPtr("iam_id"),
  Value: core.StringPtr("IBMid-123453user"),
}
policySubjects := &iampolicymanagementv1.PolicySubject{
  Attributes: []iampolicymanagementv1.SubjectAttribute{*subjectAttribute},
}
policyRoles := &iampolicymanagementv1.PolicyRole{
  RoleID: core.StringPtr("crn:v1:bluemix:public:secrets-manager::::serviceRole:SecretsReader"),
}
accountIDResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("accountId"),
  Value:    core.StringPtr("ACCOUNT_ID"),
  Operator: core.StringPtr("stringEquals"),
}
serviceNameResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("serviceName"),
  Value:    core.StringPtr("secrets-manager"),
  Operator: core.StringPtr("stringEquals"),
}
policyResources := &iampolicymanagementv1.PolicyResource{
  Attributes: []iampolicymanagementv1.ResourceAttribute{
    *accountIDResourceAttribute, *serviceNameResourceAttribute}
}

options := iamPolicyManagementService.NewCreatePolicyOptions(
  "access",
  []iampolicymanagementv1.PolicySubject{*policySubjects},
  []iampolicymanagementv1.PolicyRole{*policyRoles},
  []iampolicymanagementv1.PolicyResource{*policyResources},
)

policy, response, err := iamPolicyManagementService.CreatePolicy(options)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policy, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}





Access to {{site.data.keyword.secrets-manager_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.secrets-manager_short}} service in your account must be assigned an access policy with an IAM role. The policy determines which actions a user can perform within the context of {{site.data.keyword.secrets-manager_short}}.
{: shortdesc} 

IAM access policies enable access to be granted at different levels. Some of the options include the following actions: 

- Access across all {{site.data.keyword.secrets-manager_short}} service instances in your account
- Access to an individual {{site.data.keyword.secrets-manager_short}} instance in your account
- Access to a specific resource within a {{site.data.keyword.secrets-manager_short}} instance, such as a secret group

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
| `secrets-manager.dashboard.view` | View the {{site.data.keyword.secrets-manager_short}} dashboard. | Viewer, Operator, Editor, Administrator |
| `secrets-manager.secret-group.create` | Create secret groups. | Manager |
| `secrets-manager.secret-group.update` | Update a secret group. | Manager |
| `secrets-manager.secret-group.delete` | Delete a secret group. | Manager |
| `secrets-manager.secret-group.read` | View the details of a secret group. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret-groups.list` | List the secret groups in your instance. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret.create` | Create a secret. | Writer, Manager |
| `secrets-manager.secret.import` | Import a secret. | Writer, Manager |
| `secrets-manager.secret.read` | Get the value of a secret. | SecretsReader, Writer, Manager |
| `secrets-manager.secret.delete` | Delete a secret. | Manager |
| `secrets-manager.secrets.list` | List the secrets in your instance. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret.rotate` | Rotate a secret. | Writer, Manager |
| `secrets-manager.secret-metadata.update` | Update a secret. | Writer, Manager |
| `secrets-manager.secret-metadata.read` | View the metadata of a secret. | Reader, SecretsReader, Writer, Manager |
| `secrets-manager.secret-policies.set` | Set secret policies. | Manager |
| `secrets-manager.secret-policies.get` | Get secret policies. | Manager |
| `secrets-manager.secret-engine-config.set` | Set secrets engine configuration. | Manager |
| `secrets-manager.secret-engine-config.get` | Get secrets engine configuration. | Manager |
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

## Assigning IAM access to {{site.data.keyword.secrets-manager_short}}
{: #iam-assign-access}

You can use the {{site.data.keyword.cloud_notm}} console, CLI, or APIs to assign different levels of access to {{site.data.keyword.secrets-manager_short}} resources in your account. You can assign access at the instance level, or you can narrow access to a secret group that contains one or more secrets. For more information, see [Assigning access to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-assign-access).

To learn about using the {{site.data.keyword.cloud_notm}} CLI to assign access, check out the [{{site.data.keyword.cloud_notm}} CLI reference](/docs/cli?topic=cli-ibmcloud_commands_iam). When you create an access policy for {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.cloud_notm}} CLI or APIs, use `secrets-manager` for the service name in the CLI command or API call.
{: tip}
