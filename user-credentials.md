---

copyright:
  years: 2020, 2023
lastupdated: "2023-05-12"

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
10. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
11. Click **Next**.
12. Enter the username and password that you want to associate with the secret.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
    2. To rotate your secret at a 30, 60, or 90-day interval, switch the **Automatic secret rotation** toggle to **Yes**.
10. Click **Next**. 
11. Review the details of your secret. 
12. Click **Add**.


## Adding user credentials from the CLI
{: #user-credentials-cli}
{: cli}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. 


```sh 
ibmcloud secrets-manager secret-create \    
    --secret-prototype='{"name": "example-username-password-secret","description": "Description of my user credentials secret","secret_type": "username_password","secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa","labels": ["dev","us-south"],"username": "example-username","password": "example-password","rotation": {"auto_rotate": true,"interval": 10,"unit": "day"},"custom_metadata": {"metadata_custom_key": "metadata_custom_value"},"version_custom_metadata": {"custom_version_key": "custom_version_value"}}'

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


## Adding user credentials with Terraform
{: #user-credentials-terraform}
{: terraform}

You can store a username and password programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.

By default, Terraform detects any difference in the settings of an infrastructure object and creates a plan to update the remote object to match the configuration.

You can use the Terraform meta-argument `ignore_changes` when you create a resource with references to data that might change in the future, but `ignore_changes` does not affect the resource after you create it. With the `ignore_changes` meta-argument, you can specify resource attributes that Terraform ignores when it updates the associated remote object. When secrets are set to auto-rotation, {{site.data.keyword.secrets-manager_short}} generates a new version of the secret with a computed new value of the password property.

The following example shows a query that you can use to create a username and password secret with auto-rotation, by using the Terraform lifecycle meta-argument `ignore_changes` for the `password` field. 

1. Create the user credentials by running the following command.

```terraform
    resource "ibm_sm_username_password_secret" "test_username_password_secret" {
        instance_id = local.instance_id
        region = local.region
        secret_group_id = "default"
        name = "test-user-creds-secret"
        username = "sm_username"
        password = "sm_password"
        rotation {
            auto_rotate = true
            interval = 10
            unit = "day"
        }
        lifecycle {
            ignore_changes = [
                password,
            ]
        }
    } 
```
{: codeblock}


2. After 10 days, the secret is auto-rotated in {{site.data.keyword.secrets-manager_short}}. When you check the Terraform plan, it shows that the `password` field was changed outside of Terraform, as displayed in the following example.

```terraform
    terraform plan
    data.ibm_resource_instance.sm_resource_instance: Reading...
    data.ibm_resource_instance.sm_resource_instance: Read complete after 6s [id=crn:v1:bluemix:public:secrets-manager:eu-gb:a/1efe69ceedd307f4de6f5fb6296a34b1:73d9bf0e-7b82-453d-a9bf-bfdceceb55bd::]
    ibm_sm_username_password_secret.test_username_password_secret: Refreshing state... [id=1a87fb02-9025-8b41-77b5-6bf76d52ba72]

    Note: Objects have changed outside of Terraform

    Terraform detected the following changes made outside of Terraform since the last "terraform apply" which may have affected this plan:

    # ibm_sm_username_password_secret.sm_username_password_secret has changed
    ~ resource "ibm_sm_username_password_secret" "test_username_password_secret" {
        id                 = "1a87fb02-9025-8b41-77b5-6bf76d52ba72"
        name               = "test-user-creds-secret"
        ~ password           = (sensitive value)
        # (15 unchanged attributes hidden)

        # (1 unchanged block hidden)
    }
    Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to undo
    Hi or respond to these changes.
```
{: codeblock}


3. Applying the Terraform plan syncs the user credentials `password` field in Terraform with the new version value.


A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret).
