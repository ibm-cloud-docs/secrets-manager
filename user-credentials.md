---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-20"

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

User credentials consist of username and password values that you can use to log in to or access an external service or application. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance. In the instance, you can centrally manage its lifecycle, and control the secret's lifespan by setting an expiration date, automatic rotation policies, and more.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-user-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Adding user credentials in the UI
{: #user-credentials-ui}
{: ui}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**. 
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
10. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format.   
11. Click **Add**.

## Adding user credentials from the CLI
{: #user-credentials-cli}
{: cli}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type username_password` option.

```sh
ibmcloud secrets-manager secret-create --secret-type username_password --resources '[{"name": "example-username-password-secret","description": "Extended description for my secret.","username": "user123","password": "cloudy-rainy-coffee-book"}]' --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

<apiv2>

```sh 
ibmcloud secrets-manager secret-create \    
  --secret-type=username_password \    
  --resources='[{"name": "example-username-password-secret", "description": "Extended description for my secret.", "username": "user123","password": "cloudy-rainy-coffee-book", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}]'
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Adding user credentials with the API
{: #user-credentials-api}
{: api}

You can store a username and password programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a username and password secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.


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
          ],
          "expiration_date": "2030-01-01T00:00:00Z",
          "custom_metadata": {
            "collection_nickname" : "test_collection"
            "collection_special_id" : "test12345"
          },
          "version_custom_metadata": {
            "version_special_id" : "test6789"
          }
        }
        ]
    }'
```
{: codeblock}
{: curl}


A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager#create-secret).
