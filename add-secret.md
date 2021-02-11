---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-11"

keywords: create secrets, add secrets, store secrets, single tenant secret storage, manage secrets

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

# Creating secrets
{: #store-secrets}

With {{site.data.keyword.secrets-manager_full}}, you can create or add secrets that you can centrally manage from the {{site.data.keyword.cloud_notm}} console. Your secrets are stored in a dedicated, single-tenant instance of the service.
{: shortdesc}

## Before you begin
{: #before-store-secrets}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Creating user credentials
{: #store-user-credentials}

You can use {{site.data.keyword.secrets-manager_short}} to store a username and password that you can use to log in to and access a protected service inside or outside of {{site.data.keyword.cloud_notm}}.

### Creating user credentials in the UI
{: #user-credentials-ui}
{: ui}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **User credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Enter the username and password that you want to associate with the secret.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9.  Optional: Enable expiration and rotation options to control the lifespan of the secret.
     1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
     2. To rotate your secret at a 30, 60, or 90-day interval, switch the rotate toggle to **Yes**.

10. Click **Add**.

### Creating user credentials from the CLI
{: #user-credentials-cli}
{: cli}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type username_password` option. For example, the following command creates and stores a secret called `example-username-password-secret`.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --metadata '{"collection_type": "application/vnd.ibm.secrets-manager.secret+json", "collection_total": 1}' --resources '[{"name": "example-username-password-secret","description": "Extended description for my secret.","username": "user123","password": "cloudy-rainy-coffee-book"}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Creating user credentials with the API
{: #user-credentials-api}
{: api}


You can store a username and password by calling the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager#create-secret){: external}.

The following example shows a query that you can use to create a username and password secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create a username and password secret. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create a username and password secret. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create a username and password secret. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/username_password" \
  -H "Authorization: Bearer {IAM_token}" \
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

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

## Creating IAM credentials
{: #store-iam-credentials}

You can use the {{site.data.keyword.secrets-manager_short}} to create dynamic IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication. When you create your credentials, {{site.data.keyword.secrets-manager_short}} creates a service ID and an API key. Your credentials are dynamically generated only when you or your application needs to access a protected {{site.data.keyword.cloud_notm}} resource. After the credentials reach the end of their lease, {{site.data.keyword.secrets-manager_short}} revokes them automatically on your behalf.

Before you create IAM credentials in {{site.data.keyword.secrets-manager_short}}, you need to configure an IAM secrets engine to enable a connection between your {{site.data.keyword.secrets-manager_short}} instance and IAM. For more information, check out [Enabling the IAM secrets engine](/docs/secrets-manager?topic=secrets-manager-secret-engines#configure-iam-engine).
{: note}

### Creating IAM credentials in the UI
{: #iam-credentials-ui}
{: ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **IAM credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the secret group that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Click **Select access group** to determine the scope of access for your IAM credential.

    By selecting an access group from your {{site.data.keyword.cloud_notm}} account, you determine the scope of access to assign to the service ID that is dynamically generated and associated with your new IAM credential. This step ensures that your IAM credentials are scoped with the wanted level of permissions in your {{site.data.keyword.cloud_notm}} account. You can assign up to 10 access groups.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.
10. Click **Add**.

### Creating IAM credentials from the CLI
{: #iam-credentials-cli}
{: cli}

To create a dynamic service ID and API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type iam_credentials` option. For example, the following command creates a secret called `example-iam-credentials` that has a lease duration of 12 hours.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --metadata '{"collection_type": "application/vnd.ibm.secrets-manager.secret+json", "collection_total": 1}' --resources '[{"name":"example-IAM-credentials","description":"Extended description for my secret.","access_groups":["e7e1a364-c5b9-4027-b4fe-083454499a20"],"secret_group_id":"432b91f1-ff6d-4b47-9f06-82debc236d90","ttl":"12h","labels":["dev","us-south"]}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Creating IAM credentials with the API
{: #iam-credentials-api}
{: api}


You can create IAM credentials by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a dynamic service ID and API key. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create a dynamic service ID and API key. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create a dynamic service ID and API key. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create a dynamic service ID and API key. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/iam_credentials" \
  -H "Authorization: Bearer {IAM_token}" \
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
      'secret_group_id: '432b91f1-ff6d-4b47-9f06-82debc236d90',
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
    'secret_group_id: '432b91f1-ff6d-4b47-9f06-82debc236d90',
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

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

## Creating arbitrary secrets
{: #store-arbitrary-secrets}

You can use {{site.data.keyword.secrets-manager_short}} to store arbitrary or custom secrets, such as API keys, that you can use inside or outside of {{site.data.keyword.cloud_notm}}.

### Creating arbitrary secrets in the UI
{: #arbitrary-ui}
{: ui}

To add an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Other secret type** tile.
5. Add a name and description to easily identify your secret.
6. Select the secret group that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a file or enter the secret value that you want to associate with the secret.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
     1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
10. Click **Add**.

### Creating arbitrary secrets from the CLI
{: #arbitrary-cli}
{: cli}

To create an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type iam_credentials` option. For example, the following command creates a secret called `example-arbitrary-secret`.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --metadata '{"collection_type": "application/vnd.ibm.secrets-manager.secret+json", "collection_total": 1}' --resources '[{"name": "example-arbitrary-secret","description": "Extended description for my secret.","payload": "secret-data"}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Creating arbitrary secrets with the API
{: #arbitrary-api}
{: api}


You can create arbitrary secrets by calling the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager#create-secret){: external}.

The following example shows a query that you can use to create and store an arbitrary secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to create and store an arbitrary secret. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to create and store an arbitrary secret. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to create and store an arbitrary secret. The following code shows an example call.
{: go}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/arbitrary" \
  -H "Authorization: Bearer {IAM_token}" \
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
          "payload: "secret-data",
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

