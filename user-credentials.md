---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: username, password, user credentials, store password

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

# Storing user credentials
{: #user-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to store a username and password that you can use to log in to and access a protected service inside or outside of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

User credentials consist of username and password values that you can use to log in to or access an external service or application. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle, control the secret's lifespan by setting an expiration date, automatic rotation policies, and more.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-user-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Adding user credentials in the UI
{: #user-credentials-ui}
{: ui}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **User credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Enter the username and password that you want to associate with the secret.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
    2. To rotate your secret at a 30, 60, or 90-day interval, switch the rotate toggle to **Yes**.

10. Click **Add**.

## Adding user credentials from the CLI
{: #user-credentials-cli}
{: cli}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type username_password` option.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --resources '[{"name": "example-username-password-secret","description": "Extended description for my secret.","username": "user123","password": "cloudy-rainy-coffee-book"}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Adding user credentials with the API
{: #user-credentials-api}
{: api}

You can store a username and password programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a username and password secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `createSecret` method to create a username and password secret. The following code shows an example call.
{: java}

If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create a username and password secret. The following code shows an example call.
{: javascript}

If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create a username and password secret. The following code shows an example call.
{: python}

If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create a username and password secret. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/username_password" \
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
          "name": "example-username-password-secret",
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "username": "user123",
          "password": "cloudy-rainy-coffee-book",
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
SecretResourceUsernamePasswordSecretResource secretResourceModel = new SecretResourceUsernamePasswordSecretResource.Builder()
    .name("example-username-password-secret")
    .description("Extended description for this secret.")
    .secretGroupId("432b91f1-ff6d-4b47-9f06-82debc236d90")
    .labels(new java.util.ArrayList<String>(java.util.Arrays.asList("dev","us-south")))
    .expirationDate(TestUtilities.createMockDateTime("2030-01-01T00:00:00Z"))
    .username("user123")
    .password("cloudy-rainy-coffee-book")
    .build();
CreateSecretOptions createSecretOptions = new CreateSecretOptions.Builder()
    .secretType("username_password")
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
    secretType: 'username_password',
    'metadata': {
        'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1,
    },
    'resources': [
        {
        'name': 'example-username-password-secret',
        'description': 'Extended description for my secret.',
        'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
        'username': 'user123',
        'password': 'cloudy-rainy-coffee-book',
        'labels': ['dev', 'us-south'],
        'expiration_date': '2030-12-31T00:00:00Z',
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
    'name': 'example-username-password-secret',
    'description': 'Extended description for my secret.',
    'secret_group_id': '432b91f1-ff6d-4b47-9f06-82debc236d90',
    'username': 'user123',
    'password': 'cloudy-rainy-coffee-book',
    'labels': ['dev', 'us-south'],
    'expiration_date': '2030-12-31T00:00:00Z'
}

response = secretsManager.create_secret(
    secret_type='username_password',
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

secretResource := &sm.SecretResourceUsernamePasswordSecretResource{
    Name: core.StringPtr("example-username-password-secret"),
    Description: core.StringPtr("Extended description for this secret."),
    SecretGroupID: core.StringPtr("bc656587-8fda-4d05-9ad8-b1de1ec7e712"),
    Labels: []string{"dev","south"},
    Username: core.StringPtr("user123"),
    Password: core.StringPtr("cloud-rainy-coffee-book"),
    ExpirationDate: core.StrfmtDateTimePtr(CreateMockDateTime()),
}

createSecretOptions := secretsManagerApi.NewCreateSecretOptions(
    "username_password", collectionMetadata, []sm.SecretResourceIntf{secretResource},
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

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager#create-secret)..
