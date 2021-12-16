---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-16"

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

# Configuring the IAM credentials engine
{: #configure-iam-engine}

You can set up IAM credentials for your {{site.data.keyword.secrets-manager_full}} service instance by configuring the IAM credentials engine.
{: shortdesc}

In {{site.data.keyword.secrets-manager_short}}, the IAM credentials engine serves as the backend for the `iam_credentials` secret type. Before you can create IAM credentials, you must configure the IAM credentials engine for your service instance. You can enable your instance by entering an [API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account.

## Before you begin
{: #before-configure-iam-engine}

If you're setting up IAM credentials for the first time, be sure that you're assigned the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) on the {{site.data.keyword.secrets-manager_short}} instance. To configure the IAM secrets engine, you need a [service ID API key](/docs/account?topic=account-serviceidapikeys) with the following access:

- [**Editor** platform role](/docs/account?topic=account-account-services#access-groups-account-management) on the IAM Access Groups Service.
- [**Operator** platform role](/docs/account?topic=account-account-services#identity-service-account-management) on the IAM Identity Service.

## Setting up the IAM credentials engine in the UI
{: #configure-iam-engine-ui}
{: ui}

You can add an IAM credentials engine configuration by using the {{site.data.keyword.secrets-manager_short}} UI. To configure your instance to start creating IAM credentials, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets engines** page, click the **IAM credentials** tab.
4. Click **Configure**.
5. Enter an API key that has access to create and manage other API keys in your account.

    The service ID that is associated with your API key must have _Editor_ platform access on the IAM Access Groups Service and _Operator_ platform access on the IAM Identity Service.
    {: note}

6. Click **Configure**.

    Now, your {{site.data.keyword.secrets-manager_short}} instance is enabled for IAM credential secrets.

## Setting up the IAM credentials engine from the CLI
{: #configure-iam-engine-cli}
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

## Setting up IAM credentials with the API
{: #configure-iam-engine-api}
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
    engine_config=engine_config
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

## Next steps
{: #configure-iam-engine-next-steps}

Now you can use {{site.data.keyword.secrets-manager_short}} to dynamically generate IAM credentials for your apps. In the {{site.data.keyword.secrets-manager_short}} UI, click **Secrets > Add > IAM credentials** to start creating secrets.

- [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials)


