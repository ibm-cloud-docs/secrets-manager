---

copyright:
  years: 2020, 2023
lastupdated: "2023-02-07"

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

You can get started with {{site.data.keyword.secrets-manager_full}} by creating a service instance in {{site.data.keyword.cloud_notm}} console or with the {{site.data.keyword.cloud_notm}} CLI.
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

    You can create only one Trial instance of {{site.data.keyword.secrets-manager_short}} per account. To proceed, you must select the Standard plan or delete an existing Trial instance. If you delete the Trial plan instance, you must also delete its reclamation by using the IBM Cloud CLI. 
    {: note}

4. Provide a name for your instance.
5. Select a resource group.
6. Optional: Add tags to help you to organize the instance in your account.
7. Determine an option for enabling customer-managed encryption for your instance.

    You can enhance the security of your secrets at rest by integrating with a key management service. For more information about customer-managed encryption, check out [Protecting your sensitive data in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption).
8. Determine an option for connecting to {{site.data.keyword.secrets-manager_short}}.

    To further protect your connection to {{site.data.keyword.secrets-manager_short}}, you can choose to provision an instance that uses a private service endpoint. For more information about setting up your account to support the private connectivity option, see [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).
9. Click **Create** to create an instance of {{site.data.keyword.secrets-manager_short}} in the account, region, and resource group that you selected.

To update your service plan after you create an instance, see [Updating your service plan](/docs/billing-usage?topic=billing-usage-changing).
{: tip}


## Creating a {{site.data.keyword.secrets-manager_short}} instance from the CLI
{: #create-instance-cli}
{: cli}

You can also create an instance of {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.cloud_notm}} CLI.

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
    | Pricing plan (`plan`) | The pricing plan that you want to use, provided as a plan ID. To obtain the plan ID, run `ibmcloud catalog service secrets-manager`. For more information about plan IDs, see [Updating your service plan](/docs/billing-usage?topic=billing-usage-changing). |
    | Private endpoints | If you need to provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [private endpoints only](/docs/secrets-manager?topic=secrets-manager-service-connection), you can append `-p '{"allowed_network": "private-only"}'` to your command. |
    | Encryption | To provision an instance of {{site.data.keyword.secrets-manager_short}} that uses [customer-managed encryption](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption), append `-p '{"kms_key": "<root_key_crn>"}'`. Replace `<root_key_crn>` with the CRN value for the root key that you want to integrate. |
    {: caption="Table 1. Descripition of the information that is required to provision the  {{site.data.keyword.secrets-manager_short}} service" caption-side="top"}

    You can create only one Trial instance of {{site.data.keyword.secrets-manager_short}} per account. To proceed, you must select the Standard plan or delete an existing Trial instance. If you delete the Trial plan instance, you must also delete its reclamation by using the IBM Cloud CLI. 
    {: note}


4. Optional: Verify that the service instance was created successfully.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

To update your service plan after you create an instance, see [Updating your service plan](/docs/billing-usage?topic=billing-usage-changing).
{: tip}
