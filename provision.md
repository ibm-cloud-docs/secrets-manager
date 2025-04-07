---

copyright:
  years: 2020, 2025
lastupdated: "2025-04-07"

keywords: provsion Secrets Manager, create Secrets Manager instance, dedicated instance, lite plan

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

# Creating a {{site.data.keyword.secrets-manager_short}} service instance
{: #create-instance}

Get started with {{site.data.keyword.secrets-manager_full}} by creating a service instance in {{site.data.keyword.cloud_notm}} console, {{site.data.keyword.cloud_notm}} CLI, or API.
{: shortdesc}



Provisioning {{site.data.keyword.secrets-manager_short}} in your {{site.data.keyword.cloud_notm}} account can take 5 - 15 minutes to complete as the service creates a single tenant, dedicated instance.
{: note}


## Creating a {{site.data.keyword.secrets-manager_short}} instance in the UI
{: #create-instance-ui}
{: ui}

To create an instance of {{site.data.keyword.secrets-manager_short}} from the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. In the console, go to the [{{site.data.keyword.secrets-manager_short}} offering details page](/catalog/services/secrets-manager){: external}.
2. In the **Create** tab, select the region that represents the geographic area where you want provision your instance.
3. Review and select a pricing plan.

    You can create only one Trial instance of {{site.data.keyword.secrets-manager_short}} per account. Before you can create a new Trial instance, you must delete the existing Trial instance and its reclamation.
    {: note}

4. Provide a name for your instance.
5. Select a resource group.
6. Optional: Add tags to help you to organize the instance in your account.
7. Determine an option for enabling customer-managed encryption for your instance.

    You can enhance the security of your secrets at rest by integrating with a key management service. For more information about customer-managed encryption, check out [Protecting your sensitive data in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption).
8. Determine an option for connecting to {{site.data.keyword.secrets-manager_short}}.

    Select either `private-only` or `public-and-private`. For more information about setting up your account to support the private connectivity option, see [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).
9. Click **Create** to create an instance of {{site.data.keyword.secrets-manager_short}} in the account, region, and resource group that you selected.

To update your service plan after you create an instance, see [Updating your service plan](/docs/account?topic=account-changing).
{: tip}


## Creating a {{site.data.keyword.secrets-manager_short}} instance from the CLI
{: #create-instance-cli}
{: cli}

To create an instance of {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.cloud_notm}} CLI, complete the following steps.


1. Log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
    {: note}

2. Select the account, region, and resource group where you want to create a {{site.data.keyword.secrets-manager_short}} service instance.

    ```sh
    ibmcloud target -r <region> -g <resource_group_name>
    ```
    {: pre}

3. Create an instance of {{site.data.keyword.secrets-manager_short}} within that account and resource group.

    ```sh
    ibmcloud resource service-instance-create <instance_name> secrets-manager <plan>
    ```
    {: pre}

    | Variable | Description |
    |:---------|:------------|
    | Instance name (`name`) | A unique alias for your service instance. |
    | Pricing plan (`plan`) | The pricing plan that you want to use, provided as a plan ID. Use `869c191a-3c2a-4faf-98be-18d48f95ba1f` for `trial` or `7713c3a8-3be8-4a9a-81bb-ee822fcaac3d` for `standard`. |
    |  Endpoints | If you need to provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [private endpoints only](/docs/secrets-manager?topic=secrets-manager-service-connection), you can append `-p '{"allowed_network": "private-only"}'` to your command. Alternatively, to have both public and private endpoints, append `-p '{"allowed_network": "public-and-private"}'` to your command. |
    | Encryption | To provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [customer-managed encryption](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption), append `-p '{"kms_key": "<root_key_crn>"}'`. Replace `<root_key_crn>` with the CRN value for the root key that you want to integrate. |
    {: caption="Description of the information that is required to provision the  {{site.data.keyword.secrets-manager_short}} service using CLI" caption-side="top"}

    You can create only one Trial instance of {{site.data.keyword.secrets-manager_short}} per account. Before you can create a new Trial instance, you must delete the existing Trial instance and its reclamation.
    {: note}


4. Optional: Verify that the service instance was created successfully.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

To update your service plan after you create an instance, see [Updating your service plan](/docs/account?topic=account-changing).
{: tip}


## Creating a {{site.data.keyword.secrets-manager_short}} instance from API
{: #create-instance-api}
{: api}

To create an instance of {{site.data.keyword.secrets-manager_short}} from API, complete the following steps.

For additional programming languages support, see the [Resource Controller API Docs](/apidocs/resource-controller/resource-controller#create-resource-instance).
{: tip}


1. Obtain an IBM Cloud IAM access token.
2. Run a curl command to provision an instance of {{site.data.keyword.secrets-manager_short}}.

    ```sh
    curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances -H "Authorization: Bearer <IAM token>" -H 'Content-Type: application/json' -d '{
       "name": "<instance_name>",
       "target": "<region>",
       "resource_group": "<resource_group_id>",
       "resource_plan_id": "<plan>",
       "parameters": {"allowed_network": "private-only","kms_key": "<root_key_crn>"}
    }'
    ```
    {: pre}

    | Variable | Description |
    |:---------|:------------|
    | Instance name (`name`) | A unique alias for your service instance. |
    | Target (`region`) | The region the instance should be provisioned in. Supported regions: |
    | Pricing plan (`plan`) | The pricing plan that you want to use, provided as a plan ID. Use `869c191a-3c2a-4faf-98be-18d48f95ba1f` for `trial` or `7713c3a8-3be8-4a9a-81bb-ee822fcaac3d` for `standard`. |
    |  Endpoints | If you need to provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [private endpoints only](/docs/secrets-manager?topic=secrets-manager-service-connection), use the `allowed_network` parameter with the `private-only` value. Alternatively, to have both public and private endpoints, use `public-and-private` as the value. |
    | Encryption | To provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [customer-managed encryption](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption), keep the `kms_key` parameter, and replace `<root_key_crn>` with the CRN value for the root key that you want to integrate. |
    {: caption="Description of the information that is required to provision the  {{site.data.keyword.secrets-manager_short}} service using API" caption-side="top"}

    You can create only one Trial instance of {{site.data.keyword.secrets-manager_short}} per account. Before you can create a new Trial instance, you must delete the existing Trial instance and its reclamation.
    {: note}

To update your service plan after you create an instance, see [Updating your service plan](/docs/account?topic=account-changing).
{: tip}

## Creating a {{site.data.keyword.secrets-manager_short}} instance using Terraform
{: #create-instance-terraform}
{: terraform}

 To create an instance of {{site.data.keyword.secrets-manager_short}} using Terraform, include the following parameters in your `ibm_resource_instance` resource for {{site.data.keyword.secrets-manager_short}}.

 - **`service`**: `secrets-manager`
 - **`plan`**: either `Standard` or `Trial`

Include the following inside `parameters` for further customization.
 - **`allowed_network`**: Either `private-only` or `public-and-private`. If not included, default is `private-only`
 - **`kms_key`**: Root key CRN from either Key Protect or Hyper Protect Crypto Services instance. If not included, default is root key that is managed by {{site.data.keyword.secrets-manager_short}}

## Upgrading a {{site.data.keyword.secrets-manager_short}} instance to the Standard plan
{: #upgrade-instance-standard}

When your Trial instance expires, you lose access to your secrets, and integrations. To preserve your data, and prevent any disruptions in your workflow, you must upgrade to the Standard plan before your Trial plan expires. Follow the steps to [update your pricing plan](/docs/account?topic=account-changing). You can use the UI, API, and CLI to complete this process.
