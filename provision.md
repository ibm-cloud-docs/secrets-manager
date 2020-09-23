---

copyright:
  years: 2020
lastupdated: "2020-09-23"

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
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}

# Creating a {{site.data.keyword.secrets-manager_short}} service instance
{: #create-instance}

You can get started with {{site.data.keyword.secrets-manager_full}} by creating a service instance in {{site.data.keyword.cloud_notm}} console or with the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

When you provision {{site.data.keyword.secrets-manager_short}} in your {{site.data.keyword.cloud_notm}} account, the service creates a single tenant, dedicated instance.

You can create only one Lite plan instance of {{site.data.keyword.secrets-manager_short}} per {{site.data.keyword.cloud_notm}} account. The Lite plan includes access to all service capabilities for free.
{: note}

## Creating a {{site.data.keyword.secrets-manager_short}} instance in the console
{: #create-instance-ui}

To create an instance of {{site.data.keyword.secrets-manager_short}} from the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. Log in to your {{site.data.keyword.cloud_notm}} account.
2. Go to the [{{site.data.keyword.secrets-manager_short}} offering details page](/catalog/services/secrets-manager){: external}.
3. In the Create tab, select the region that represents the geographic area where you want provision your instance.
4. Review and select a pricing plan.
5. Provide a name for your instance.
6. Select a resource group.
7. Optional: Add tags to help you to organize the instance in your account.
8. Determine an option for enabling customer-managed encryption for your instance.

  You can enhance the security of your secrets at rest by integrating with {{site.data.keyword.keymanagementserviceshort}}. For more information about customer-managed encryption, check out [Protecting your sensitive data in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption).
9. Click **Create** to create an instance of {{site.data.keyword.secrets-manager_short}} in the account, region, and resource group that you selected.

  Provisioning your dedicated {{site.data.keyword.secrets-manager_short}} instance can take 4 - 5 minutes to complete. 

## Creating a {{site.data.keyword.secrets-manager_short}} instance with the CLI
{: #create-instance-cli}

You can also create an instance of {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.cloud_notm}} CLI. 

1. Log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

     If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
     {: note}

2. Select the account, region, and resource group where you want to create a {{site.data.keyword.secrets-manager_short}} service instance.

    You can use the following command to set your target region and resource group.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. Create an instance of {{site.data.keyword.secrets-manager_short}} within that account and resource group.

    ```sh
    ibmcloud resource service-instance-create <instance_name> secrets-manager lite
    ```
    {: pre}

    Replace `<instance_name>` with a unique alias for your service instance.

4. Optional: Verify that the service instance was created successfully.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}
