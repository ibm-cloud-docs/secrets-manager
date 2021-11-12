---

copyright:
  years: 2020, 2021
lastupdated: "2021-11-12"

keywords: IAM credentials, dynamic, IAM API key, IAM secret engine, IAM secrets engine

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

# Creating IAM credentials
{: #iam-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to dynamically generate IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication.
{: shortdesc}

IAM credentials are [dynamic secrets](#x9968958){: term} that you can use to access an {{site.data.keyword.cloud_notm}} resource. A set of IAM credentials consists of a service ID and an API key that is generated each time that the protected resource is read or accessed. You can define a time-to-live (TTL) or a lease duration for your IAM credential at its creation so that you shorten the amount of time that the secret exists.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-iam-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

IAM credentials require an extra configuration step before you can start to create or manage them in the service. For more information, see [Configuring the IAM credentials engine](/docs/secrets-manager?topic=secrets-manager-configure-iam-engine).
{: note}


## Creating IAM credentials in the UI
{: #iam-credentials-ui}
{: ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **IAM credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Click **Select access group** to determine the scope of access for your IAM credential.

    By selecting an access group from your {{site.data.keyword.cloud_notm}} account, you determine the scope of access to assign to the service ID that is dynamically generated and associated with your new IAM credential. This step ensures that your IAM credentials are scoped with the preferred level of permissions in your {{site.data.keyword.cloud_notm}} account. You can assign up to 10 access groups.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.
10. Optional: Determine whether IAM credentials can be reused for your secret.

    By default, IAM credentials are generated and deleted each time that a secret is read or accessed. By setting **Reuse IAM credentials** to **On**, your secret retains its current service ID and API key values, so that you can reuse the same credentials on each read while the secret remains valid. After the secret reaches the end of its lease, the credentials are revoked automatically.
13. Click **Add**.

## Creating IAM credentials from the CLI
{: #iam-credentials-cli}
{: cli}

To create a dynamic service ID and API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type iam_credentials` option. For example, the following command creates an IAM secret with a lease duration of 12 hours.

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --resources '[{"name":"example-IAM-credentials","description":"Extended description for my secret.","access_groups":["e7e1a364-c5b9-4027-b4fe-083454499a20"],"secret_group_id":"432b91f1-ff6d-4b47-9f06-82debc236d90","ttl":"12h","labels":["dev","us-south"]}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Creating IAM credentials with the API
{: #iam-credentials-api}
{: api}


You can create IAM credentials programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a dynamic service ID and API key. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `createSecret` method to create a dynamic service ID and API key. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create a dynamic service ID and API key. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create a dynamic service ID and API key. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create a dynamic service ID and API key. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/iam_credentials" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
        "collection_total": 1
        },
        "resources": [
        {
          "name": "example-IAM-credentials",
          "description": "Extended description for my secret.",
          "access_groups": [
            "AccessGroupId-e7e1a364-c5b9-4027-b4fe-083454499a20"
          ],
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "reuse_api_key": <true|false>,
          "ttl": "12h",
          "labels": [
            "dev",
            "us-south"
          ]
        }
        ]
    }'
```
{: codeblock}
{: curl}

```java
CollectionMetadata collectionMetadataModel = new CollectionMetadata.Builder()
    .collectionType("application/vnd.ibm.secrets-manager.secret+json")
    .collectionTotal(Long.valueOf("1"))
    .build();
SecretResourceIAMSecretResource secretResourceModel = new SecretResourceIAMSecretResource.Builder()
    .name("example-IAM-credentials")
    .description("Extended description for this secret.")
    .accessGroups(new java.util.ArrayList<String>(java.util.Arrays.asList("AccessGroupId-e7e1a364-c5b9-4027-b4fe-083454499a20")))
    .secretGroupId("432b91f1-ff6d-4b47-9f06-82debc236d90")
    .labels(new java.util.ArrayList<String>(java.util.Arrays.asList("dev","us-south")))
    .ttl("12h")
    .build();
CreateSecretOptions createSecretOptions = new CreateSecretOptions.Builder()
    .secretType("iam_credentials")
    .metadata(collectionMetadataModel)
    .resources(new java.util.ArrayList<SecretResource>(java.util.Arrays.asList(secretResourceModel)))
    .build();

Response<CreateSecret> response = sm.createSecret(createSecretOptions).execute();
CreateSecret createSecret = response.getResult();

System.out.println(createSecret);
```
{: codeblock}
{: java}

```javascript
const params = {
    secretType: 'iam_credentials',
    'metadata': {
        'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1,
    },
    'resources': [
        {
        'name': 'example-IAM-credentials',
        'description': 'Extended description for my secret.',
        'access_groups': [
        'AccessGroupId-e7e1a364-c5b9-4027-b4fe-083454499a20'
        ],
        'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
        'ttl': '12h',
        'labels': [
        'dev',
        'us-south'
        ]
    },
    ],
};

secretsManagerApi.createRules(params)
    .then(res => {
        console.log('Create secret:\n', JSON.stringify(result.resources, null, 2));
    })
    .catch(err => {
        console.warn(err)
    });
```
{: codeblock}
{: javascript}

```python
collection_metadata = {
    'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1
}

secret_resource = {
    'name': 'example-IAM-credentials',
    'description': 'Extended description for this secret.',
    'access_groups': [
        'AccessGroupId-e7e1a364-c5b9-4027-b4fe-083454499a20'
    ],
    'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
    'ttl': '12h',
    'labels': [
        'dev',
        'us-south'
    ]
}

response = secretsManager.create_secret(
    secret_type='iam_credentials',
    metadata=collection_metadata,
    resources=[secret_resource]
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
collectionMetadata := &sm.CollectionMetadata{
    CollectionType: core.StringPtr("application/vnd.ibm.secrets-manager.secret+json"),
    CollectionTotal: core.Int64Ptr(int64(1)),
}

secretResource := &sm.SecretResourceIAMSecretResource{
    Name: core.StringPtr("example-IAM-credentials"),
    Description: core.StringPtr("Extended description for this secret."),
    AccessGroups: []string{"AccessGroupId-e7e1a364-c5b9-4027-b4fe-083454499a20"},
    SecretGroupID: core.StringPtr("432b91f1-ff6d-4b47-9f06-82debc236d90"),
    TTL: []string{"12h"},
    Labels: []string{"dev","us-south"},
}

createSecretOptions := secretsManagerApi.NewCreateSecretOptions(
    "iam_credentials", collectionMetadata, []sm.SecretResourceIntf{secretResource},
)

result, response, err := secretsManagerApi.CreateSecret(createSecretOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

When you set the `reuse_api_key` parameter true, the credentials that are generated for the secret can be reused until the secret expires. For more information, check out the [API reference](/apidocs/secrets-manager#create-secret).
{: tip}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager#create-secret).


