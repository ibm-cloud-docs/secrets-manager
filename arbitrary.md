---

copyright:
  years: 2020, 2023
lastupdated: "2023-05-08"

keywords: arbitrary secrets, arbitrary text, custom secrets

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

# Storing arbitrary secrets
{: #arbitrary-secrets}

You can use {{site.data.keyword.secrets-manager_full}} to store arbitrary secrets that are used to access protected systems that are inside or outside of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

An arbitrary secret is a type of application secret that can be used to hold structured or unstructured data, such as a key, configuration file, or any other piece of sensitive information. After you create the secret, you can use it to connect your application to a protected resource, such as a third-party app or database. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)


## Before you begin
{: #before-arbitrary}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Creating arbitrary secrets in the UI
{: #arbitrary-ui}
{: ui}

To add an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.




1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Other secret type** tile.
5. Click **Next**.
6. Add a name and description to easily identify your secret.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
10. Click **Next**.
11. Select a file or enter the secret value that you want to associate with the secret.

    {{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you select a file to assign to an arbitrary secret, the service uses base64 encoding to store the data in your instance. To access this secret later, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can track secrets that require base64 decoding.
    {: note}

12. Optional: Enable expiration  to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
12. Click **Next**.
13. Review the details of your secret.
14. Click **Add**.



## Creating arbitrary secrets from the CLI
{: #arbitrary-cli}
{: cli}

To create an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type arbitrary` option. For example, the following command creates an arbitrary secret and stores `secret-data` as its value.

{{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you need to upload a binary file, you must base64 encode the data first so that you can pass it to the {{site.data.keyword.secrets-manager_short}} CLI in a single-line string. To access this secret later in its original form, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can track secrets that require base64 decoding.
{: note}


```sh
ibmcloud secrets-manager secret-create \    
    --secret-prototype='{"custom_metadata": {"anyKey": "anyValue"}, "description": "Description of my arbitrary secret.", "expiration_date": "2023-10-05T11:49:42Z", "labels": ["dev","us-south"], "name": "example-arbitrary-secret", "secret_group_id": "default", "secret_type": "arbitrary", "payload": "secret-data", "version_custom_metadata": {"anyKey": "anyValue"}}'

```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


## Creating arbitrary secrets with the API
{: #arbitrary-api}
{: api}

You can create arbitrary secrets programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create and store an arbitrary secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

{{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you need to upload a binary file, you must base64 encode the data first so that you can pass it to the {{site.data.keyword.secrets-manager_short}} API in a single-line string. To access this secret later in its original form, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can track secrets that require base64 decoding.
{: note}


```sh
curl -X POST 
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
          "custom_metadata": {
            "metadata_custom_key": "metadata_custom_value"
          },
          "description": "Description of my arbitrary secret.",
          "expiration_date": "2023-10-05T11:49:42Z",
          "labels": [
            "dev",
            "us-south"
          ],
          "name": "example-arbitrary-secret",
          "payload": "secret-data",
          "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
          "secret_type": "arbitrary",
          "version_custom_metadata": {
          "custom_version_key": "custom_version_value"} }' \ 
      "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}


A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager#create-secret).<apiv2>


## Creating arbitrary secrets with Terraform
{: #arbitrary-terraform}
{: terraform}

You can create arbitrary secrets programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.

Follow Terraform best practices for protecting sensitive input variables such as secret credentials. For more information, see [Protect sensitive input variables](https://developer.hashicorp.com/terraform/tutorials/configuration-language/sensitive-variables).
{: note}

The following example shows a configuration that you can use to create an arbitrary secret by setting sensitive values in a `terraform.tfvars` file.

1. Define an input variable for the arbitrary secret payload in a `variables.tf` file.

```terraform
    variable "arbitrary_secret_payload" {
        description = "Arbitrary secret payload"
        type        = string
        sensitive   = true
    }
```
  {: codeblock}
  

2. Assign a value to the `arbitrary_secret_payload` variable in a `terraform.tfvars` file.

    By setting values with a `.tfvars` file, you can separate sensitive values from the rest of your variable values, and ensure that your users who work with your configuration know which values are sensitive. For security purposes, you must maintain and share the `.tfvars` file only with your users who have the appropriate access. You must also be careful not to store `.tfvars` files with sensitive values into version control such as Github, in clear text.
    {: note}

    ```terraform
    arbitrary_secret_payload = "my sensitive arbitrary payload"
    ```
    {: codeblock}


3. Create the arbitrary secret in the `main.tf` file.

    ```terraform
    resource "ibm_sm_arbitrary_secret" "sm_arbitrary_secret" {
        instance_id = local.instance_id
        region = local.region
        description = "Extended description for this arbitrary secret"
        labels = [ "tf-resource"]
        name = "test-arbitrary-secret"
        secret_group_id = ibm_sm_secret_group.sm_secret_group_test.secret_group_id
        payload = var.arbitrary_secret_payload
    } 
    ```
    {: codeblock}


</apiv2>
