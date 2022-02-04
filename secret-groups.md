---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-04"

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

# Organizing your secrets
{: #secret-groups}

When you work with {{site.data.keyword.secrets-manager_full}}, you can create groups to organize your secrets and control who on your team has access to them. Then, if you don't need them anymore, you can delete the groups.
{: shortdesc}

Similar to the way that [resource groups](#x2161955){: term} help to ensure correct policy enforcement at the platform level, you can create secret groups at the instance level to organize secrets.

![The image shows two examples of a secret group and how they're mapped to access groups. One where the reader role is assigned and one where the manager role is assigned. The content is explained fully in the surrounding text.](/images/secret-group.svg){: caption="Figure 1. Assigning access to secret groups" caption-side="bottom"}

As shown in the previous image, users with *Reader* access to a secret group can see that the group exists and understand which secrets are assigned to it. Users with *Writer* access can view and edit the secret group and secrets themselves. By design, the default secret group inherits all of the same permissions that are set for the instance.

You can choose to group your secrets by phase of development, specific to the type of roles that people on your team have, or in any way that might help you. Each secret can be mapped to one group only and the mapping occurs at the time of secret creation.

To learn about the suggested guidelines for using secret groups, check out [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).
{: tip}

## Before you begin
{: #before-secret-groups}

Before you begin, be sure that you have the required level of access. To create and manage secret groups, you need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam).


## Creating secret groups
{: #create-secret-groups}

You can create secret groups by using the {{site.data.keyword.secrets-manager_short}} console or the API.

### Creating secret groups in the UI
{: #create-group-ui}
{: ui}

You can create secret groups by using the console. You can also create a secret group during the process of adding or creating a secret.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the navigation, click **Secret groups > Create**.
4. Add a name and description to easily identify your group.
5. Click **Create**.
6. Optional: Assign your secret group an [IAM policy](/docs/secrets-manager?topic=secrets-manager-assign-access).


### Creating secret groups with the API
{: #create-group-api}
{: api}

You can create secret groups by using the {{site.data.keyword.secrets-manager_short}} API.

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secret_groups" \
    -H "Authorization: Bearer $IAM_TOKEN" \
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
{: codeblock}
{: curl}

```java
CollectionMetadata collectionMetadataModel = new CollectionMetadata.Builder()
    .collectionType("application/vnd.ibm.secrets-manager.secret.group+json")
    .collectionTotal(Long.valueOf("1"))
    .build();
SecretGroupResource secretGroupResourceModel = new SecretGroupResource.Builder()
    .name("example-secret-group")
    .description("Extended description for this group.")
    .build();
CreateSecretGroupOptions createSecretGroupOptions = new CreateSecretGroupOptions.Builder()
    .metadata(collectionMetadataModel)
    .resources(new java.util.ArrayList<SecretGroupResource>(java.util.Arrays.asList(secretGroupResourceModel)))
    .build();

Response<SecretGroupDef> response = sm.createSecretGroup(createSecretGroupOptions).execute();
SecretGroupDef secretGroupDef = response.getResult();

System.out.println(secretGroupDef);
```
{: codeblock}
{: java}

```javascript
const params = {
    'metadata': {
        'collection_type': 'application/vnd.ibm.secrets-manager.secret.group+json',
    'collection_total': 1,
    },
    'resources': [
        {
        'name': 'example-secret-group',
        'description': 'Extended description for my secret group.'
    },
    ],
};

secretsManagerApi.createSecretGroup(params)
    .then(res => {
        console.log('Create secret group:\n', JSON.stringify(result.resources, null, 2));
    })
    .catch(err => {
        console.warn(err)
    });
```
{: codeblock}
{: javascript}

```python
collection_metadata = {
    'collection_type': 'application/vnd.ibm.secrets-manager.secret.group+json',
    'collection_total': 1
}

secret_group_resource = {
    'name': 'example-secret-group',
    'description': 'Extended description for this group.'
}

response = secretsManager.create_secret_group(
    metadata={'collection_type':'application/vnd.ibm.secrets-manager.secret.group+json','collection_total':1},
    resources=[secret_group_resource]
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
collectionMetadata := &sm.CollectionMetadata{
    CollectionType: core.StringPtr("application/vnd.ibm.secrets-manager.secret.group+json"),
    CollectionTotal: core.Int64Ptr(int64(1)),
}

secretGroupResource := &sm.SecretGroupResource{
    Name: core.StringPtr("example-secret-group"),
    Description: core.StringPtr("Extended description for this group."),
}

createSecretGroupOptions := secretsManagerApi.NewCreateSecretGroupOptions(
    collectionMetadata, []sm.SecretGroupResource{*secretGroupResource},
)

result, response, err := secretsManagerApi.CreateSecretGroup(createSecretGroupOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))

secretGroupIdLink = *result.;
```
{: codeblock}
{: go}


## Deleting secret groups
{: #delete-groups}

If you no longer need to use a group, you can delete it by using the console or the API.

To delete a secret group, it must be empty. If you need to remove a secret group that contains secrets, you must first [delete the secrets](/docs/secrets-manager?topic=secrets-manager-delete-secrets) that are part of the group.
{: note}


### Deleting secret groups in the UI
{: #delete-group-ui}
{: ui}

You can delete secret groups by using the console.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the navigation, click **Secret groups**.
4. In the row for the secret group that you want to delete, click the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg).
5. Click **Delete group**.
6. Click **Delete**.

### Deleting secret groups with the API
{: #delete-group-api}
{: api}

You can delete secret groups by using the {{site.data.keyword.secrets-manager_short}} API.

The following example request deletes a secret group. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}

If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `deleteSecretGrouop` method to delete a secret group. The following code shows an example call.
{: java}

If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `deleteSecretGroup(params)` method to delete a secret group. The following code shows an example call.
{: javascript}

If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `delete_secret_group(params)` method to delete a secret group. The following code shows an example call.
{: python}

If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `DeleteSecretGroup` method to delete a secret group. The following code shows an example call.
{: go}

```bash
curl -X DELETE "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secret_groups/{id}" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json"
```
{: codeblock}
{: curl}

```java
DeleteSecretGroupOptions deleteSecretGroupOptions = new DeleteSecretGroupOptions.Builder()
    .id(secretGroupIdLink)
    .build();

service.deleteSecretGroup(deleteSecretGroupOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
    id: secretGroupId,
};

secretsManagerApi.deleteSecretGroup(params)
    .then(res => {
        console.log('Secret group deleted');
    })
    .catch(err => {
        console.warn(err)
    });
```
{: codeblock}
{: javascript}

```python
response = secretsManager.delete_secret_group(
    id=secret_group_id_link
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
deleteSecretGroupOptions := secretsManagerApi.NewDeleteSecretGroupOptions(
    secretGroupIdLink,
)

response, err := secretsManagerApi.DeleteSecretGroup(deleteSecretGroupOptions)
if err != nil {
    panic(err)
}
```
{: codeblock}
{: go}




