---

copyright:
  years: 2020, 2024
lastupdated: "2024-09-19"

keywords: IAM credentials, dynamic, IAM API key IAM credentials engine

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
{:terraform: .ph data-hd-interface="terraform"}
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

In {{site.data.keyword.secrets-manager_short}}, the IAM credentials engine serves as the backend for the `iam_credentials` secret type. Before you can create IAM credentials, you must configure the IAM credentials engine for your service instance. You can enable your instance by creating an [IAM service authorization](/docs/account?topic=account-serviceauth), or by entering an [API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account.

When using an API key for the engine configuration, the access token that is generated lasts for 60 minutes and will continue to work even if the API key is deleted from IAM. This can be modified by changing the token expiration in IAM settings. [Learn more](/docs/account?topic=account-token-limit). Note that if the entity that is related to the API key is deleted, the token is invalidated immediately.
{: note}

When using IAM service authorization for the engine configuration, you can create and manage IAM credential secrets in the same {{site.data.keyword.cloud_notm}} account as the {{site.data.keyword.secrets-manager_short}} instance, as well as from other accounts.
{: tip}

## Before you begin
{: #before-configure-iam-engine}

To configure the IAM credentials engine, be sure that you're assigned the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) on the {{site.data.keyword.secrets-manager_short}} instance. 

If configuring the IAM credentials engine with an API key, you need a [service ID API key](/docs/account?topic=account-serviceidapikeys) with the following access:

- [**Editor** platform role](/docs/account?topic=account-account-services#access-groups-account-management) on the IAM Access Groups Service.
- [**Operator** platform role](/docs/account?topic=account-account-services#identity-service-account-management) on the IAM Identity Service.
- [**Service ID creator** service role](/docs/account?topic=account-account-services#identity-service-account-management) on the IAM Identity Service.

The service ID creator service role is only required when you disable the creation of service IDs in your IAM settings.
{: important}

If the account in which you want to generate IAM credentials allows access from specific IP addresses, you must also update the IP address settings in the account to allow incoming requests from {{site.data.keyword.secrets-manager_short}}. For more information, see [Managing access with context-based restrictions](/docs/secrets-manager?topic=secrets-manager-access-control-cbr).
{: important}


## Setting up the IAM credentials engine in the UI
{: #configure-iam-engine-ui}
{: ui}

### Using IAM service authorization
{: #ui-s2s}

Complete the following steps to configure the IAM credentials engine using IAM service authorization.

1. In the **Secrets engines** page, click the **IAM credentials** tab.
2. Click **Authorize**.
3. Click **Configure**.

{{site.data.keyword.secrets-manager_short}} adds the following two authorization policies on your behalf, for the current instance.
- **Operator** for **IAM Identity Service service**
- **Groups Service Member Manage** for **IAM Access Groups Service service**
{: note}

### Using API key
{: #ui-apikey}

Complete the following steps to configure the IAM credentials engine using a Service ID's API key.

1. In the **Secrets engines** page, click the **IAM credentials** tab.
2. Click **Configure IAM credentials engine with API key**.
3. Enter an API key that has access to create and manage other API keys in your account.
4. Click **Configure**.

If you configured your {{site.data.keyword.secrets-manager_short}} instance by using an API key, the key will take priority over an IAM service credential configuration. To use the service credential flow. you can disable and delete the API key configuration. Be sure to delete the API key when it's no longer needed.
{: note}

## Setting up the IAM credentials engine from the CLI
{: #configure-iam-engine-cli}
{: cli}

### Using IAM service authorization
{: #cli-s2s}

Complete the following steps to configure the IAM credentials engine by using an IAM service authorization.

1. Add the required policies.

      ```sh
      ibmcloud iam authorization-policy-create secrets-manager iam-identity "Service ID Creator, Operator" --source-service-instance-id SOURCE_SERVICE_INSTANCE_GUID --source-service-account ACCOUNT_GUID 
      ibmcloud iam authorization-policy-create secrets-manager iam-groups "Groups Service Member Manage" --source-service-account SOURCE_SERVICE_INSTANCE_GUID --source-service-instance-id ACCOUNT_GUID
      ```
      {: pre}
      
      - Replace **SOURCE_SERVICE_INSTANCE_GUID** with the {{site.data.keyword.secrets-manager_short}} instance service ID. If not provided, the authorization applies to all {{site.data.keyword.secrets-manager_short}} instances
      - Replace **ACCOUNT_GUID** with the {{site.data.keyword.cloud_notm}} account's ID where you want to create IAM credential secrets. If you are managing secrets in the same account as the {{site.data.keyword.secrets-manager_short}} instance, you do not need to provide this.

### Using API key
{: #cli-apikey}

Complete the following steps to configure the IAM credentials engine by using a Service ID's API key.

1. In a terminal window, log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

    ```sh
    ibmcloud login
    ```
    {: pre}

2. Select the account, region, and resource group where your {{site.data.keyword.secrets-manager_short}} service instance is located.
3. Create a service ID and set it as an environment variable.

    ```sh
    export SERVICE_ID=`ibmcloud iam service-id-create <service_ID_name> --description "<description>" --output json | jq -r ".id"`; echo $SERVICE_ID
    ```
    {: pre}

4. Manage access for the service ID.

    Assign the service ID permissions to create and manage other service IDs.

    ```sh
    ibmcloud iam service-policy-create $SERVICE_ID --roles Operator --service-name "iam-identity"
    ```
    {: pre}

    Assign the service ID permissions to view and update access groups in your account.

    ```sh
    ibmcloud iam access-group-policy-create $SERVICE_ID --roles Editor --service-name "iam-groups"
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

5. Use the API key to configure the IAM credentials engine for your instance.

    To configure a secrets engine from the {{site.data.keyword.cloud_notm}} CLI, run the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command.

    ```sh
    ibmcloud secrets-manager configuration-create --configuration-prototype='{"config_type": "iam_credentials_configuration","name": "iam-configuration","api_key": "'$IBM_CLOUD_API_KEY'"}'
    ```
    {: pre}


    Using a Windows™ command prompt (`cmd.exe`) or PowerShell? If you encounter errors with passing JSON content on the command line, you might need to adjust the strings for quotation-escaping requirements that are specific to your operating system. For more information, see [Using quotation marks with strings in the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-quote-strings).
    {: tip}


## Configuring the IAM credentials engine with the API
{: #configure-iam-engine-api}
{: api}

For step-by-step instructions to create an {{site.data.keyword.cloud_notm}} API key with the correct level of access, switch to the **UI** or **CLI** steps.
{: tip}

### Using IAM service authorization
{: #api-s2s}

The following example shows a query that you can use to configure the IAM credentials engine for your instance when using IAM service authorization. Learn more in {{site.data.keyword.cloud_notm}} IAM's [API Docs](/apidocs/iam-policy-management#create-policy).

```sh
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' -d '{
  "type": "authorization",
  "subjects": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$SOURCE_ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "secrets-manager"
        },
        {
          "name": "serviceInstance",
          "value": "$INSTANCE_GUID"
        }
      ]
    }
  ],
  "roles":[
        {
          "role_id": "crn:v1:bluemix:public:iam-identity::::serviceRole:ServiceIdCreator"
        },
        {
          "role_id": "crn:v1:bluemix:public:iam::::role:Operator"
        }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$TARGET_ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "iam-identity"
        }
      ]
    }
  ]
}'

curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' -d '{
  "type": "authorization",
  "subjects": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$SOURCE_ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "secrets-manager"
        },
        {
          "name": "serviceInstance",
          "value": "$INSTANCE_GUID"
        }
      ]
    }
  ],
  "roles":[
        {
          "role_id": "crn:v1:bluemix:public:iam-groups::::serviceRole:MemberManage"
        }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name":"$TARGET_ACCOUNT_ID",
          "value": "791f5fb10986423e97aa8512f18b7e65"
        },
        {
          "name": "serviceName",
          "value": "iam-groups"
        }
      ]
    }
  ]
}'
```
{: codeblock}
{: curl}

The **serviceInstance** attribute is optional. If omitted the authorization applies to all {{site.data.keyword.secrets-manager_short}} instances.
The **SOURCE_ACCOOUNT_ID** and **TARGET_ACCOUNT_ID** are required wether creating the authorization for the current account or a specific account.
{: note}

### Using API key
{: #api-apikey}

The following example shows a query that you can use to configure the IAM credentials engine for your instance using an API key. When you call the API, replace the API key variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{ 
    "api_key": "{iam_apikey}", "config_type": "iam_credentials_configuration", 
    "name": "iam-configuration" 
    }' \ 
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations"
```
{: codeblock}
{: curl}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.


## Configuring the IAM credentials engine with Terraform
{: #configure-iam-engine-terraform}
{: terraform}

The following example shows a query that you can use to configure a secrets engine for your instance when using an IAM service authorization.

```terraform
    resource "ibm_sm_iam_credentials_configuration" "iam_credentials_configuration" {
        instance_id = local.instance_id
        region = local.region
        name = "iam_credentials_config"
        api_key = var.ibmcloud_api_key
    }
```
{: codeblock}


The following example shows a configuration that you can use to configure the IAM credentials engine by using a Service ID's API key.

```terraform
    resource "ibm_sm_iam_credentials_configuration" "iam_credentials_configuration" {
        instance_id = local.instance_id
        region = local.region
        name = "iam_credentials_config"
        api_key = var.ibmcloud_api_key
    }
```
{: codeblock}

## Retrieving an IAM credentials engine's configuration value in the UI
{: #get-iam-engine-value-ui}
{: ui}

You can retrieve an engine's configuration by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the **IAM credentials** secret engine, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your engine configuration.
2. To view the configuration value, click **View configuration**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}


## Retrieving an engine's configuration value using CLI
{: #get-iam-engine-value-cli}
{: cli}

You can retrieve an engine's configuration by using the {{site.data.keyword.secrets-manager_short}} CLI. In the following example command, replace the engine configuration name with your configuration's name.

```sh
ibmcloud secrets-manager configuration --name EXAMPLE_CONFIG --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)


## Retrieving an engine's configuration value using API
{: #get-iam-engine-value-api}
{: api}

You can retrieve an engine's configuration by using the {{site.data.keyword.secrets-manager_short}} API. In the following example request, replace the engine configuration name with your configuration's name.

```sh
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
--header "Accept: application/json" \
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

A successful response returns the value of the engine configuration, along with other metadata. For more information about the required and optional request parameters, see [Get configuration](/apidocs/secrets-manager/secrets-manager-v2#get-configuration){: external}.


## Next steps
{: #configure-iam-engine-next-steps}

Now you can use {{site.data.keyword.secrets-manager_short}} generate IAM credentials. In the {{site.data.keyword.secrets-manager_short}} UI, click **Secrets > Add > IAM credentials** to start creating secrets.

- [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials)
