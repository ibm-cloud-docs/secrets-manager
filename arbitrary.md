---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-22"

keywords: arbitrary secrets, arbitrary text, custom secrets

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

# Storing arbitrary secrets
{: #arbitrary-secrets}

You can use {{site.data.keyword.secrets-manager_full}} to store arbitrary secrets that are used to access protected systems that are inside or outside of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

An arbitrary secret is a type of application secret that can be used to hold structured or unstructured data, such as a key, configuration file, or any other piece of sensitive information. After you create the secret, you can use it to connect your application to a protected resource, such as a third-party app or database. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-arbitrary}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Creating arbitrary secrets in the UI
{: #arbitrary-ui}
{: ui}

To add an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Other secret type** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a file or enter the secret value that you want to associate with the secret.

    {{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you select a file to assign to an arbitrary secret, the service uses base64 encoding to store the data in your instance. To access this secret later, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can keep track of secrets that require base64 decoding.
    {: note}

8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
10. Click **Add**.

## Creating arbitrary secrets from the CLI
{: #arbitrary-cli}
{: cli}

To create an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type arbitrary` option. For example, the following command creates an arbitrary secret and stores `secret-data` as its value.

```sh
ibmcloud secrets-manager secret-create --secret-type arbitrary --resources '[{"name": "example-arbitrary-secret","description": "Extended description for my secret.","payload": "secret-data"}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Creating arbitrary secrets with the API
{: #arbitrary-api}
{: api}


You can create arbitrary secrets programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create and store an arbitrary secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `createSecret` method to create and store an arbitrary secret. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create and store an arbitrary secret. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create and store an arbitrary secret. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create and store an arbitrary secret. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/arbitrary" \
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
          "name": "example-arbitrary-secret",
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "payload": "secret-data",
          "expiration_date": "2030-12-31T00:00:00Z",
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
SecretResourceArbitrarySecretResource secretResourceModel = new SecretResourceArbitrarySecretResource.Builder()
    .name("example-arbitrary-secret")
    .description("Extended description for this secret.")
    .secretGroupId("432b91f1-ff6d-4b47-9f06-82debc236d90")
    .labels(new java.util.ArrayList<String>(java.util.Arrays.asList("testString")))
    .expirationDate(TestUtilities.createMockDateTime("2030-01-01T00:00:00Z"))
    .payload("secret-data")
    .build();
CreateSecretOptions createSecretOptions = new CreateSecretOptions.Builder()
    .secretType("arbitrary")
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
    secretType: 'arbitrary',
    'metadata': {
        'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1,
    },
    'resources': [
        {
        'name': 'example-arbitrary-secret',
        'description': 'Extended description for my secret.',
        'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
        'payload': 'secret-data',
        'expiration_date': '2030-12-31T00:00:00Z',
        'labels': ['dev', 'us-south'],
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
    'name': 'example-arbitrary-secret',
    'description': 'Extended description for this secret.',
    'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
    'payload': 'secret-data',
    'labels': ['dev', 'us-south']
}

response = secretsManager.create_secret(
    secret_type='arbitrary',
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

secretResource := &sm.SecretResourceArbitrarySecretResource{
    Name: core.StringPtr("example-arbitrary-secret"),
    Description: core.StringPtr("Extended description for this secret."),
    SecretGroupID: core.StringPtr("bc656587-8fda-4d05-9ad8-b1de1ec7e712"),
    Labels: []string{"dev","us-south"},
    ExpirationDate: core.StrfmtDateTimePtr(CreateMockDateTime()),
    Payload: core.StringPtr("secret-data"),
}

createSecretOptions := secretsManagerApi.NewCreateSecretOptions(
    "arbitrary", collectionMetadata, []sm.SecretResourceIntf{secretResource},
)

result, response, err := secretsManagerApi.CreateSecret(createSecretOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))

secretIdLink = *result.;
```
{: codeblock}
{: go}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.


