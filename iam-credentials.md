---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-20"

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

# IAM credentials
{: #iam-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to dynamically generate IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication.
{: shortdesc}

IAM credentials are [dynamic secrets](#x9968958){: term} that you can use to access an {{site.data.keyword.cloud_notm}} resource. A set of IAM credentials consists of a service ID and an API key that is generated each time that the protected resource is read or accessed. You can define a time-to-live (TTL) or a lease duration for your IAM credential at its creation so that you shorten the amount of time that the secret exists.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-iam-credentials}

To create IAM credentials, you need the [**Editor** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam) on the {{site.data.keyword.secrets-manager_short}} instance. If you're setting up IAM credentials for the first time by using the {{site.data.keyword.secrets-manager_short}} UI, be sure that you also have the required permissions to configure the IAM secrets engine:

- [**Editor** platform role](/docs/account?topic=account-account-services#access-groups-account-management) on the IAM Access Groups Service.
- [**Operator** platform role](/docs/account?topic=account-account-services#identity-service-account-management) on the IAM Identity Service.
- [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) on the {{site.data.keyword.secrets-manager_short}} instance.


## Configuring the IAM secrets engine in the UI
{: #configure-iam-secrets-engine-ui}
{: ui}

Before you can create dynamic IAM credentials, you must configure the IAM [secrets engine](#x9968967){: term} for your service instance. You can configure your instance by creating or entering an [{{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account.

To configure your instance to start creating IAM credentials, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Settings** page, go to the **IAM secrets engine** section.
4. Click **New**.
5. In the **Configure IAM secrets engine** side pane, choose an option for setting up your instance.

    To create an API key, choose the **Create an API key** option. {{site.data.keyword.secrets-manager_short}} assigns the new service ID and API key the IAM access that's required to manage and create other IAM credentials dynamically within your instance. You can also enter an existing {{site.data.keyword.cloud_notm}} API by choosing the **Use an existing API key** option.

    The required level of access might differ depending on the option that you choose. If you choose the **Use an existing API key** option, the key's associated service ID must have _Editor_ platform access on the IAM Access Groups Service and _Operator_ platform access on the IAM Identity Service. If you choose the **Create an API key** option, be sure that you're assigned the _Administrator_ platform roles on both the IAM Access Groups Service and IAM Identity Service. 
    {: note}
6. Click **Configure**.

   Now, your {{site.data.keyword.secrets-manager_short}} instance is enabled for IAM credential secrets.




## Configuring the IAM secrets engine from the CLI
{: #configure-iam-secrets-engine-cli}
{: cli}

Before you can create dynamic IAM credentials, you must configure the IAM secrets engine for your service instance. Start by entering an [{{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account.

To allow your {{site.data.keyword.cloud_notm}} API key to create and manage other API keys dynamically, its associated service ID must have _Editor_ platform access for the IAM Access Groups Service, and _Operator_ platform access for the IAM Identity Service.
{: note}

1. In a terminal window, log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
    {: note}

2. Select the account, region, and resource group where your {{site.data.keyword.secrets-manager_short}} service instance is located.
3. Create a service ID and set it as an environment variable.

    ```sh
    export SERVICE_ID=`ibmcloud iam service-id-create <service_ID_name> --description "<description>" --output json | jq -r ".id"`; echo $SERVICE_ID
    ```
    {: pre}

4. Manage access for the service ID.

    Assign the service ID permissions to create and manage other service IDs.

    ```sh
    ibmcloud iam service-policy-create $SERVICE_ID --roles Operator --service-name "IAM Identity Service"
    ```
    {: pre}

    Assign the service ID permissions to view and update access groups in your account.

    ```sh
    ibmcloud iam access-group-policy-create $SERVICE_ID --roles Editor --service-name "IAM Access Groups"
    ```
    {: pre}

    Add the service ID to an access group in your account.

    ```sh
    ibmcloud iam access-group-service-id-add <access_group_name> $SERVICE_ID
    ```
    {: pre}

    Create an {{site.data.keyword.cloud_notm}} API key for your service ID.

    ```sh
    export IBM_CLOUD_API_KEY=`ibmcloud iam service-api-key-create <API_key_name> $SERVICE_ID --description "<description>" --output json | jq -r ".apikey"`
    ```
    {: pre}

5. Use the API key to configure the IAM secrets engine for your instance.

    To configure a secrets engine from the {{site.data.keyword.cloud_notm}} CLI, run the [**`ibmcloud secrets-manager config-update`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-config-update-command) command.

    ```sh
    ibmcloud secrets-manager config-update --secret-type iam_credentials --engine-config '{"api_key": "'"$API_KEY"'"}'
    ```
    {: pre}

    Using a Windows™ command prompt (`cmd.exe`) or PowerShell? If you encounter errors with passing JSON content on the command line, you might need to adjust the strings for quotation-escaping requirements that are specific to your operating system. For more information, see [Using quotation marks with strings in the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-quote-strings).
    {: tip}

## Configuring the IAM secrets engine with the API
{: #configure-iam-secrets-engine-api}
{: api}


Before you can create dynamic IAM credentials, you must configure the IAM secrets engine for your service instance. You can configure a secrets engines programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

First, you need an [{{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account. To allow your {{site.data.keyword.cloud_notm}} API key to create and manage other API keys dynamically, its associated service ID must have _Editor_ platform access for the IAM Access Groups Service, and _Operator_ platform access for the IAM Identity Service.

For step-by-step instructions to create an {{site.data.keyword.cloud_notm}} API key with the correct level of access, switch to the **UI** or **CLI** steps.
{: tip}

The following example shows a query that you can use to configure a secrets engine for your instance. When you call the API, replace the API key variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `putConfig` method to configure a secrets engine. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `putConfig(params)` method to configure a secrets engine. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `put_config(params)` method to configure a secrets engine. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `PutConfig` method to configure a secrets engine. The following code shows an example call.
{: go}

```sh
curl -X PUT "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/config/iam_credentials" \
  -H "Authorization: Bearer $IAM_TOKEN"
  -H "Accept: application/json"
  -H "Content-Type: application/json"
  -d '{
    "api_key": "$IBM_CLOUD_API_KEY"
  }'
```
{: codeblock}
{: curl}

```java
EngineConfigOneOfIAMSecretEngineRootConfig engineConfigOneOfModel = new EngineConfigOneOfIAMSecretEngineRootConfig.Builder()
  .apiKey("API_KEY")
  .build();
PutConfigOptions putConfigOptions = new PutConfigOptions.Builder()
  .secretType("iam_credentials")
  .engineConfigOneOf(engineConfigOneOfModel)
  .build();

service.putConfig(putConfigOptions).execute();
```
{: codeblock}
{: java}

```javascript

```
{: codeblock}
{: javascript}

```python
engine_config = {
    'api_key': 'API_KEY'
}

response = secretsManager.put_config(
    secret_type='iam_credentials',
    engine_config_one_of=engine_config
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
engineConfig := &sm.EngineConfigOneOfIAMSecretEngineRootConfig{
    ApiKey: core.StringPtr("API_KEY"),
}

putConfigOptions := secretsManagerApi.NewPutConfigOptions(
    "iam_credentials", engineConfig,
)

response, err := secretsManagerApi.PutConfig(putConfigOptions)
if err != nil {
    panic(err)
}
```
{: codeblock}
{: go}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

## Creating IAM credentials in the UI
{: #iam-credentials-ui}
{: ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **IAM credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Click **Select access group** to determine the scope of access for your IAM credential.

    By selecting an access group from your {{site.data.keyword.cloud_notm}} account, you determine the scope of access to assign to the service ID that is dynamically generated and associated with your new IAM credential. This step ensures that your IAM credentials are scoped with the wanted level of permissions in your {{site.data.keyword.cloud_notm}} account. You can assign up to 10 access groups.
8. (Optional) Add labels to help you to search for similar secrets in your instance.
9. Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.
10. (Optional) Determine whether IAM credentials can be reused for your secret.

    By default, IAM credentials are generated and deleted each time that a secret is read or accessed. By setting **Reuse IAM credentials** to **On**, your secret retains its current service ID and API key values, so that you can reuse the same credentials on each read while the secret remains valid. After the secret reaches the end of its lease, the credentials are revoked automatically.
11. Click **Add**.

## Creating IAM credentials from the CLI
{: #iam-credentials-cli}
{: cli}

To create a dynamic service ID and API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type iam_credentials` option. For example, the following command creates an IAM secret with a lease duration of 12 hours.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --resources '[{"name":"example-IAM-credentials","description":"Extended description for my secret.","access_groups":["e7e1a364-c5b9-4027-b4fe-083454499a20"],"secret_group_id":"432b91f1-ff6d-4b47-9f06-82debc236d90","ttl":"12h","labels":["dev","us-south"]}]'
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

When you set the `reuse_api_key` parameter true, the credentials that are generated for the secret can be reused until the secret expires. For more information, check out the [API reference](/apidocs/secrets-manager#create-secret).
{: tip}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.
