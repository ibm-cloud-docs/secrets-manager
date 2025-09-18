---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-18"

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

# Storing user credentials
{: #user-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to store a username and password that you can use to log in to and access a protected service inside or outside of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

User credentials consist of username and password values that you can use to log in to or access an external service or application. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance. In the instance, you can centrally manage its lifecycle, and control the secret's lifespan by setting an expiration date, automatic rotation policies, and more.

In user credentials, only the **password** value in the can be rotated. Once the **username** value is set, it cannot be changed.
{: note}


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
4. From the list of secret types, select the **User credentials** tile.
5. Click **Next**.
6. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
10. Click **Next**.
11. Enter the username and password that you want to associate with the secret.

    If you choose to generate a password, {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols. You can choose to further customize the generated password by configuring its length (12-256 characters), and whether to include numbers, symbols, and upper-case letters.

12. Optional: Enable expiration and rotation options to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
    2. To rotate your secret at a 30, 60, or 90-day interval, switch the **Automatic secret rotation** toggle to **Yes**.
13. Click **Next**. 
14. Review the details of your secret. 
15. Click **Add**.


## Adding user credentials from the CLI
{: #user-credentials-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. 


```sh 
ibmcloud secrets-manager secret-create \    
    --secret-prototype='{"name": "example-username-password-secret","description": "Description of my user credentials secret","secret_type": "username_password","secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa","labels": ["dev","us-south"],"username": "example-username","password": "example-password","rotation": {"auto_rotate": true,"interval": 10,"unit": "day"},"custom_metadata": {"metadata_custom_key": "metadata_custom_value"},"version_custom_metadata": {"custom_version_key": "custom_version_value"}}'

```
{: pre}

## Generating a random password from the CLI
{: #random-password-cli}
{: cli}

You can choose to further customize the generated password by configuring its length (12-256 characters), and whether to include numbers, symbols, and upper-case letters. To generate a random password using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command and include the `password_generation_policy` field.


```sh 
ibmcloud secrets-manager secret-create \    
    --secret-prototype='{"name": "example-username-password-secret","description": "Description of my user credentials secret","secret_type": "username_password","secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa","labels": ["dev","us-south"],"username": "example-username","password": "example-password","rotation": {"auto_rotate": true,"interval": 10,"unit": "day"},"password_generation_policy": {"length": 24, "include_digits": true, "include_symbols": false, "include_uppercase": true}, "custom_metadata": {"metadata_custom_key": "metadata_custom_value"},"version_custom_metadata": {"custom_version_key": "custom_version_value"}}'

```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Adding user credentials with the API
{: #user-credentials-api}
{: api}

You can store a username and password programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a username and password secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.


```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
      "name": "example-username-password-secret",
      "description": "Description of my user credentials secret",
      "secret_type": "username_password",
      "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
      "labels": [
        "dev",
        "us-south"
      ],
      "username": "example-username",
      "password": "example-password",
      "rotation": {
        "auto_rotate": true,
        "interval": 10,
        "unit": "day"
      },
      "custom_metadata": {
        "metadata_custom_key": "metadata_custom_value"
      },
      "version_custom_metadata": {
        "custom_version_key": "custom_version_value"
      }
    }'
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets" 
```
{: codeblock}
{: curl}

## Generating a random password with the API
{: #random-password-api}
{: api}

You can choose to further customize the generated password by configuring its length (12-256 characters), and whether to include numbers, symbols, and upper-case letters. When calling the {{site.data.keyword.secrets-manager_short}} API include the `password_generation_policy` field.

```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
      "name": "example-username-password-secret",
      "description": "Description of my user credentials secret",
      "secret_type": "username_password",
      "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
      "labels": [
        "dev",
        "us-south"
      ],
      "username": "example-username",
      "password": "example-password",
      "rotation": {
        "auto_rotate": true,
        "interval": 10,
        "unit": "day"
      },
      "password_generation_policy": {
        "length": 24,
        "include_digits": true,
        "include_symbols": false,
        "include_uppercase": true
      },
      "custom_metadata": {
        "metadata_custom_key": "metadata_custom_value"
      },
      "version_custom_metadata": {
        "custom_version_key": "custom_version_value"
      }
    }'
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets" 
```
{: codeblock}
{: curl}


## Adding user credentials with Terraform
{: #user-credentials-terraform}
{: terraform}

You can store a username and password programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.

You can either provide a password by setting the optional `password` argument in the Terraform configuration or choose to generate a random password. If you omit the `password` argument, {{site.data.keyword.secrets-manager_short}} generates a 32-character random password that contains uppercase letters, lowercase letters, digits, and symbols. You can choose to further customize the generated password by configuring its length (12-256 characters), and whether to include numbers, symbols, and upper-case letters.

If you configure the secret with an automatic rotation policy it is recommended to omit the `password` argument to have the initial password also generated automatically. This is to avoid a Terraform drift situation, where after automatic rotation has occurred, Terraform detects that the password had been changed outside of Terraform. 

The following example shows a query that you can use to create a username and password secret with a randomly generated password and auto-rotation enabled. This example also shows how to specify a non-default password generation policy for the secret.

```terraform
    resource "ibm_sm_username_password_secret" "test_username_password_secret" {
        instance_id = local.instance_id
        region = local.region
        secret_group_id = "default"
        name = "test-user-creds-secret"
        username = "sm_username"
        rotation {
            auto_rotate = true
            interval = 10
            unit = "day"
        }
        password_generation_policy {
            length = 24
            include_digits = true
            include_symbols = false
            include_uppercase = true
        }
    } 
```
{: codeblock}

The following example shows a query that you can use to create a username and password secret with the password provided in the Terraform configuration. Auto-rotation is disabled in this example, but you can rotate the password manually by modifying the value of the `password` argument in the configuration.

```terraform
    resource "ibm_sm_username_password_secret" "test_username_password_secret" {
        instance_id = local.instance_id
        region = local.region
        secret_group_id = "default"
        name = "test-user-creds-secret"
        username = "sm_username"
        password = "sm_password"
    } 
```
{: codeblock}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret).
