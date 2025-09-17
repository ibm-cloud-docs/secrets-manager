---

copyright:
  years: 2025
lastupdated: "2025-09-17"

keywords: Secrets Manager custom credentials, Secrets Manager third-party

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Creating custom credentials secrets
{: #custom-credentials}

The custom credentials secret type enables {{site.data.keyword.secrets-manager_full}} users to securely manage credentials for external systems (such as Artifactory or PagerDuty) through {{site.data.keyword.secrets-manager_short}} APIs and integrations. To create these secrets, you deploy an  {{site.data.keyword.codeenginefull}} job that acts as a bridge between {{site.data.keyword.secrets-manager_short}} and the external service. These jobs run on a fixed schedule and manage credentials asynchronously by using [secret tasks](/docs/secrets-manager?topic=secrets-manager-secret-tasks).
{: shortdesc}

The process for creating custom credentials is asynchronous by design. When a new secret is added, it initially enters a **pre-activation** state. If the secret is successfully created in the external credentials provider, its state automatically becomes **active** within {{site.data.keyword.secrets-manager_short}}.

The custom credentials secrets creation process is asynchronous. After a secret is added, it begins in `pre-activation` state and if created successfully in the credentials provider, its state changes to `active` in {{site.data.keyword.secrets-manager_short}}.
{: note}

## Before you begin
{: #before-custom-credentials}

Before you get started, make sure that you have:
- The required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).
- Configured your instance to create custom credentials secrets by [creating a {{site.data.keyword.codeengineshort}} job and custom credentials engine configuration](/docs/secrets-manager?topic=secrets-manager-custom-credentials-prepare).

## Creating a custom credentials secret in the console
{: #custom-credentials-secret-ui}
{: ui}

To add a secret:

1. In the **Secrets** table, click **Add**.
2. From the list of secret types, click the **Custom credentials** tile.
3. Click **Next**.
4. Add a name and description to easily identify your secret.
5. Select the [secret group](#x9968962){: term} that [you have previously created](/docs/secrets-manager?topic=secrets-manager-custom-credentials-config&interface=ui#custom-credentials-config-before-begin).
6. Optional: Add labels to help you to search for similar secrets in your instance.
7. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format.  
8. Click **Next**.
9. Select the custom credentials engine configuration to use for this secret.
10. Enter the required values under **Parameters**.
11. Click **Next**.
12. Optional: Enable least duration and automatic rotation of your secret.
13. Click **Next**.
18. Review the details of your secret. 
19. Click **Add**.

You can change the value of the parameters later. The change takes place after a new secret version is created. You cannot add or subtract new parameters without creating a new configuration.
{: note}

## Viewing and updating secret details in the console
{: #custom-credentials-view-update-ui}
{: ui}

As with [any other secret](/docs/secrets-manager?topic=secrets-manager-updating-secret-metadata), you can access your secret details by clicking the click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) > **Details**. From the details screens, you can learn about:

* [Accessing secrets](/docs/secrets-manager?topic=secrets-manager-access-secrets)
* [Deleting secrets](/docs/secrets-manager?topic=secrets-manager-delete-secrets)
* [Automatically rotating secrets](/docs/secrets-manager?topic=secrets-manager-automatic-rotation)
* [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation)
* [Updating secret version metadata](/docs/secrets-manager?topic=secrets-manager-updating-secret-version)
* [Managing secrets versions](/docs/secrets-manager?topic=secrets-manager-version-history)

## Creating a custom credentials secret from CLI
{: #custom-credentials-secret-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To create a custom credentials secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. 

```sh 
ibmcloud secrets-manager secret-create --secret-type custom_credentials --secret-name "example-custom-credential-secret" --secret-description "Description of my custom credential secret" --secret-rotation '{"auto_rotate": true,"interval": 30,"unit": "day"}' --custom-credentials-parameters '{"my_input_parameter":"my_param_value"}' --custom-credentials-configuration '{"my_custom_credential_config"}' --secret-custom-metadata '{"metadata_custom_key": "metadata_custom_value"},"version_custom_metadata": {"custom_version_key": "custom_version_value"}}'
```
{: pre}


## Creating a custom credentials secret using API
{: #custom-credentials-secret-api}
{: api}

You can create a custom credential programmatically by calling the {{site.data.keyword.secrets-manager_short}} API. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.


```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
      "name": "example-custom-credential-secret",
      "description": "Description of my custom credential secret",
      "secret_type": "custom_credentials",
      "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
      "labels": [
        "dev",
        "us-south"
      ],
      "rotation": {
        "auto_rotate": true,
        "interval": 30,
        "unit": "day"
      },
      "configuration": "my_custom_credential_config",
      "parameters": {
        "user_name": "username",
        "scope": "admin"
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

## Creating a custom credentials secret by using Terraform
{: #custom-credentials-secret-terraform}
{: terraform}

You can create custom credentials secrets programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.
The following example shows a configuration that you can use to create a custom credentials secret.

Creating custom credentials secrets is an asynchronous process that can potentially take a long time depending on the use-case, therefore when planning to use Terraform potential delays should be considered.
{: note}

```terraform
resource "ibm_sm_custom_credentials_secret" "sm_custom_credentials_secret" {
  instance_id   = ibm_resource_instance.sm_instance.guid
  region        = "us-south"
  name 			= "secret-name"
  secret_group_id = ibm_sm_secret_group.sm_secret_group.secret_group_id
  custom_metadata = {"key":"value"}
  description = "Extended description for this secret."
  labels = ["my-label"]
  configuration = "my_custom_credentials_configuration"
  parameters {
    int_values = {
        example_param_1 = 17
    }
    string_values = {
        example_param_2 = "str2"
        example_param_3 = "str3"
    }
    bool_values = {
        example_param_4 = false
    }
  }
  rotation {
      auto_rotate = true
      interval = 3
      unit = "day"
  }
  ttl = "864000"
}
```
