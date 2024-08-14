---

copyright:
  years: 2024
lastupdated: "2024-08-12"

keywords: terraform, {{site.data.keyword.secrets-manager_short}}

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


# Setting up Terraform for {{site.data.keyword.secrets-manager_short}}
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud_notm}} enables predictable and consistent creation of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multitier cloud environments that follow Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the creation, update, and deletion of your {{site.data.keyword.secrets-manager_short}} instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud_notm}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with. But you don't need to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud_notm}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud_notm}} catalog.
{: tip}

## Installing Terraform and configuring resources for {{site.data.keyword.secrets-manager_short}}
{: #install-terraform}

Before you can create an authorization by using Terraform, make sure that you completed the following steps:

* Make sure that you have the [required access](/docs/secrets-manager?topic=secrets-manager-assign-access&interface=ui) to create and work with {{site.data.keyword.secrets-manager_short}} resources.
* Install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. For more information, see the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud_notm}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.
* Create a Terraform configuration file that is named `main.tf`. In this file, you define resources by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.


1. After you finish building your configuration file, initialize the Terraform CLI. For more information, see [Initializing Working Directories](https://developer.hashicorp.com/terraform/cli/init){: external}.

   ```terraform
   terraform init
   ```
   {: pre}

2. Create a {{site.data.keyword.secrets-manager_short}} instance by using the `ibm_resource_instance` resource argument in your `main.tf` file.

   * The {{site.data.keyword.secrets-manager_short}} instance in the following example is named `secrets-manager-london` and is created with the trial plan in the `eu-gb` region. The `user@ibm.com` is assigned the Administrator role in the IAM access policy. For other supported regions, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints). Plan options include `trial` and `standard`. 

      ```terraform
       resource "ibm_resource_instance" "sm_instance" {
           name = "Secrets Manager-London"
           service = "secrets-manager"
           plan = "trial"
           location = "eu-gb"
           timeouts {
            create = "60m"
            delete = "2h"
        },
       }
      ```
      {: codeblock}
 
 
      To view a complete list of the supported attributes, see [`ibm_resource_instance`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/resource_instance){: external}.

    * Optionally, you can create a data source to retrieve information about an existing {{site.data.keyword.secrets-manager_short}} instance from {{site.data.keyword.cloud_notm}}, by running the following command. 

        ```terraform
        data "ibm_resource_instance" "sm_resource_instance" {
            name              = "Secrets Manager-London"
            location          = "eu-gb"
            service           = "secrets-manager"
        }
        ```
        {: codeblock}

   For a complete list of the supported attributes, see [`ibm_resource_instance`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/resource_instance){: external}.

3. Provision the resources from the `main.tf` file. For more information, see [Provisioning Infrastructure with Terraform](https://developer.hashicorp.com/terraform/cli/run){: external}.

   1. Run `terraform plan` to generate a Terraform execution plan to preview the proposed actions.

        ```terraform
        terraform plan
        ```
        {: pre}

   2. Run `terraform apply` to create the resources that are defined in the plan.

        ```terraform
        terraform apply
        ```
        {: pre}

4. Define local values for your {{site.data.keyword.secrets-manager_short}} instance to be used when you are creating resources.

    ```terraform
        locals {
            instance_id = data.ibm_resource_instance.sm_resource_instance.guid
            region = data.ibm_resource_instance.sm_resource_instance.location
        }
    ```
   {: pre}

5. From the {{site.data.keyword.cloud_notm}} resource list in the UI, select the {{site.data.keyword.secrets-manager_short}} instance that you created and note the instance ID.
6. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources&interface=ui#review-your-access-console).

## Managing Resource Drift
{: #resource-drift-terraform}
 
With Terraform, you can safely and predictably manage the lifecycle of your infrastructure by using declarative configuration files. One challenge that exists when you are managing infrastructure as code is drift. Drift occurs when resources are added, deleted, or modified outside of applying Terraform configuration changes. For example, when a secret expires or is rotated. To avoid drift, always use Terraform to manage resources initially created with Terraform.

The Terraform state file is a record of all resources that Terraform manages. You must not make manual changes to resources that are controlled by Terraform because by doing so, the state file becomes out of sync or "drift", from the real infrastructure. If your state and configuration do not match your infrastructure, Terraform attempts to reconcile your infrastructure, which might unintentionally destroy or re-create resources.

When you are using the {{site.data.keyword.secrets-manager_short}} Terraform provider, a drift might occur in cases such as:

- Secret expiration
- Secret auto-rotation
- External changes to Secret Manager resources that are controlled by Terraform

When you are designing your Terraform project, follow Terraform best practices for managing drift and lifecycle changes to avoid unintentional destruction or recreation of {{site.data.keyword.secrets-manager_short}} resources.
{: note}


## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.secrets-manager_short}} service instance with Terraform on {{site.data.keyword.cloud_notm}}, You can review the {{site.data.keyword.secrets-manager_short}} resources and data sources in the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/sm_secrets){: external}. You can also review how to manage your {{site.data.keyword.secrets-manager_short}} resources by following the Terraform steps that are included in the How to section. For example, you can follow the directions on how to create [arbitrary secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets) by using Terraform.


