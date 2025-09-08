---

copyright:
  years: 2025
lastupdated: "2025-09-08"

keywords: Secrets Manager custom credentials, Secrets Manager third-party

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Creating a custom credentials engine configuration
{: #custom-credentials-config}

After you have created your  {{site.data.keyword.codeenginefull}} project and job, you can create your custom credentials engine configuration. The engine configuration references your {{site.data.keyword.codeengineshort}} project and the specific {{site.data.keyword.codeengineshort}} job in the project that you'd like to use for a custom credential secret.

Custom credentials have various limitations. You can read about them in the [Known issues and limitations page](/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits#custom-creds-limits).

## Before you begin
{: #custom-credentials-config-before-begin}

Make sure to complete these prerequisites:

* Create a {{site.data.keyword.codeengineshort}} project and job.
* Create an [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials) secret that allows {{site.data.keyword.codeengineshort}} access to {{site.data.keyword.secrets-manager_short}}. The credential and IAM authorization allow {{site.data.keyword.codeengineshort}} to return information back to {{site.data.keyword.secrets-manager_short}} and make necessary updates to {{site.data.keyword.secrets-manager_short}} relevant to your custom credentials secret. The IAM credentials secret must:
    * Have an auto rotation policy set.
    * Be associated with a service ID or access group that has the following permissions:
      - `secrets-manager.secret-task.update` on the secret group where the custom credential is created.
      - If applicable, `secrets-manager.secret.read` on any secret group that contains any reference secrets that are needed for the {{site.data.keyword.codeengineshort}} job to complete its actions.
    * Alternatively, configure a trusted profile for authentication. Refer to the [{{site.data.keyword.codeengineshort}} documentation](/docs/codeengine?topic=codeengine-trusted-profiles) for setup details.
* Create an IAM service authorization between {{site.data.keyword.secrets-manager_short}} as the **Source** and {{site.data.keyword.codeengineshort}} project as the **Target**.

## Creating your custom credentials engine configuration in the console
{: #custom-credentials-config-ui}
{: ui}

Navigate to the **Custom credentials** screen inside the **Secret engines** navigation, then create the configuration for your custom credentials by entering the relevant information in the console.

1. In the **Secrets engines** page, click the **Custom credentials** tab.
2. Click **Add configuration**

   1. Provide a configuration name.
   2. Select the region where your {{site.data.keyword.codeengineshort}} project was created.
   3. Select the project and job that was created for this custom credentials secret. If the project and job are in another account, enter the respective project ID and job name.
   4. When creating the configuration you are prompted to create an IAM service authorization between {{site.data.keyword.secrets-manager_short}} as **Source** and {{site.data.keyword.codeengineshort}} as **Target**, if not previously created.
   5. Optionally select the IAM Credentials secret you have previously created or create one in-context.
3. Click **Add**.

## Creating your custom credentials engine configuration using the API
{: #custom-credentials-config-api}
{: api}

You can create a custom credentials engine configuration programmatically by calling the {{site.data.keyword.secrets-manager_short}} API. When you call the API, replace the `api_key_ref` and `code_engine` variables, and the IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. You can optionally supply a `task_timeout` parameter to customize the task timeout.

```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
      "name": "my_config",
      "config_type": "custom_credentials_configuration",
      "api_key_ref": "a2f9c2e4-a3a8-c508-2bed-fcb7c26843ca",
      "code_engine": {
        "job_name": "code-engine-job-name",
        "project_id": "12345678-5120-4a18-832a-4ba122496633",
        "region": "us-south"
      },
      "task_timeout": "50m"
    }'
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations" 
```
{: codeblock}
{: curl}

## Creating your custom credentials engine configuration from CLI
{: #custom-credentials-config-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To create a custom credentials engine configuration by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command. You can optionally use the `--configuration-task-timeout` flag to customize the task timeout.

```sh
ibmcloud secrets-manager configuration-create --config-type=custom_credentials_configuration --name=my-custom-credentials-config --custom-credentials-apikey-ref IAM_credentials_secret_ID --custom-credentials-code-engine '{"project_id":"12345678-5120-4a18-832a-4ba122496633", "region":"us-south", "job_name":"code-engine-job-name"}'
```

## Creating your custom credentials engine configuration using Terraform
{: #custom-credentials-config-terraform}
{: terraform}

You can create a custom credentials engine configuration by using Terraform for {{site.data.keyword.secrets-manager_short}}.

```terraform
resource "ibm_sm_custom_credentials_configuration" "sm_custom_credentials_configuration_instance" {
	instance_id = ibm_resource_instance.sm_instance.guid
	region = "us-south"
	name = "example-custom-credentials-config"
	api_key_ref = ibm_sm_iam_credentials_secret.my_secret_for_custom_credentials.secret_id
	code_engine {
	    project_id = ibm_code_engine_project.my_code_engine_project.project_id
	    job_name = "my_code_engine_job"
	    region = "us-south"
	}
	task_timeout = "10m"
}
```

## Next steps
{: #custom-credentials-config-next-steps}

Once you have created your custom credentials engine configuration, you can now create a [custom credentials secret](/docs/secrets-manager?topic=secrets-manager-custom-credentials).
