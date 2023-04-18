---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-18"

keywords: DNS provider, connect DNS provider, set up DNS provider, connect DNS, set up DNS, connect CIS, set up CIS, add DNS provider configuration

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

# Connecting DNS providers
{: #add-dns-provider}

With {{site.data.keyword.secrets-manager_full}}, you can connect to a DNS provider by adding a configuration to your instance.
{: shortdesc}

A DNS provider is the service that is used to add and manage domains for apps or services. By adding a DNS configuration, you can specify the DNS service to use for domain validation when you [order certificates](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates) through {{site.data.keyword.secrets-manager_short}}. 

You can define up to 10 DNS configurations per instance. To view a list of configurations that are available for your instance, go to the **Secrets engines > Public certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.
{: note}
{: ui}

You can define up to 10 DNS configurations per instance. To obtain a list of configurations that are available for your instance, you can use the [List configurations](/apidocs/secrets-manager#get-secret-config-element) API.
{: note}
{: api}

## Before you begin
{: #before-add-dns-provider}

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

### Supported DNS providers
{: #add-dns-provider-supported}

You can connect the following DNS providers with your {{site.data.keyword.secrets-manager_short}} service instance.

| Prerequisites |
| :------------ |
| Before you add a configuration for Cloud Internet Services (CIS), be sure that you:  \n  \n - [Create a CIS service instance](/docs/cis?topic=cis-getting-started).  \n - [Create an authorization between {{site.data.keyword.secrets-manager_short}} and CIS](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis).  \n - If your CIS instance is located in another account, obtain the CRN of the instance and [create an API key with the correct level of access](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account). |
{: caption="Table 1. Prerequisites - CIS" caption-side="top"}
{: #cis-prereqs}
{: tab-title="Cloud Internet Services"}
{: tab-group="dns-provider-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| Before you add a configuration for classic infrastructure, be sure that you:  \n  \n 1. [Obtain your classic infrastructure username](/docs/account?topic=account-classic_keys). If you are using IBMid to log in to your account, your classic infrastructure username is your `<account_id>_<email_address>`. \n 2. [Create a classic infrastructure API key](/docs/account?topic=account-classic_keys). Assign your user permissions to manage DNS in the account. For more information about managing classic infrastructure access, see [Classic infrastructure permissions](/docs/account?topic=account-mngclassicinfra#how-classic-infra-permissions-work). |
{: caption="Table 1. Prerequisites - Classic infrastructure" caption-side="top"}
{: #classic-infrastructure-prereqs}
{: tab-title="Classic infrastructure"}
{: tab-group="dns-provider-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| To use your own DNS provider, you must [create a certificate authority configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority&interface=ui) before you can [order a your certificate](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates-manual-api). |
{: caption="Table 1. Prerequisites - Manual DNS providers" caption-side="top"}
{: #manual-prereqs}
{: tab-title="Manual"}
{: tab-group="dns-provider-prereqs"}
{: class="simple-tab-table"}

## Adding a DNS provider configuration in the UI
{: #add-dns-provider-ui}
{: ui}

You can add DNS provider configurations to your service instance by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets engines** page, click the **Public certificates** tab.
4. In the DNS providers table, click **Add**.
5. Select the DNS provider that you want to use. 

   Currently, you can add configurations for Cloud Internet Services (CIS) and IBM Cloud classic infrastructure.
6. Grant service access between {{site.data.keyword.secrets-manager_short}} and your selected DNS provider.
   1. If you choose CIS, grant access by selecting from a list of authorized CIS instances or by entering an API key.

      Don't have an authorization yet? You can [create one in the IAM console](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis). Optionally, you can grant access to CIS by providing an API key and the instance CRN. You can find the CRN in the **Overview** page of your CIS service instance. For more information about creating an API key for CIS, see [Granting service access by using an API key](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account)
   2. If you choose classic infrastructure, enter the [username and API key](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-classic-infrastructure) that is associated with your account.
7. Click **Add**.



## Adding a DNS provider configuration with the API
{: #add-dns-provider-api}
{: api}

You can add DNS provider configurations to your service instance by using the {{site.data.keyword.secrets-manager_short}} API. 

### Configuring Cloud Internet Services (CIS)
{: #add-cis-config-api}
{: api}

The following example shows a query that you can use to add a Cloud Internet Services (CIS) DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance. When you call the API, replace the `cis_crn` value with the CRN of the CIS instance that contains your domains.
{: curl}

If you need to access a CIS instance that is located in another account, provide a `cis_apikey` value that contains an API key with **Manager** service access on the Internet Services (`internet-svs`) service. For more information, see [Granting service access to CIS](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account).
{: note} 


```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
    "cloud_internet_services_apikey": "5ipu_ykv0PMp2MhxQnDMn7VzrkSlBwi3BOI8uthi_EXZ",
    "cloud_internet_services_crn": "crn:v1:bluemix:public:internet-svcs:global:a/128e84fcca45c1224aae525d31ef2b52:009a0357-1460-42b4-b903-10580aba7dd8::",
    "config_type": "public_cert_configuration_dns_cloud_internet_services",
    "name": "cloud-internet-services-config"
  }' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v2/configurations"
```
{: codeblock}
{: curl}


A successful response adds the configuration to your service instance. For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.


### Configuring classic infrastructure
{: #add-classic-infra-config-api}
{: api}

The following example shows a query that you can use to add a classic infrastructure DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance. When you call the API, replace the `classic_infrastructure_username` and `classic_infastructure_password` (API key) values.
{: curl}


```sh
curl -X POST 
  --H 'Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "classic_infrastructure_password": "sRBm1jkHOH2kn9oBnK5R0ODsRBm1jkHOH2kn9oBnK5R0ODsRBm1jkHOH2kn9oBnK5R0OD",
  "classic_infrastructure_username": "1234567_JohnDoe@mail.com",
  "config_type": "public_cert_configuration_dns_classic_infrastructure",
  "name": "classic-infrastructure-config"
}' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v2/configurations"
```
{: codeblock}
{: curl}



A successful response adds the configuration to your service instance. For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.


## Adding a DNS provider configuration with Terraform
{: #add-dns-provider-terraform}
{: terraform}

You can add DNS provider configurations to your service instance by using Terraform for {{site.data.keyword.secrets-manager_short}}.

### Configuring Cloud Internet Services (CIS) with Terraform
{: #add-cis-config-terraform}
{: terraform}

The following example shows a confihuration that you can use to add a a Cloud Internet Services (CIS) DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance. 

```terraform
resource "ibm_sm_public_certificate_configuration_dns_cis" "my_dns_cis_config" {
   instance_id = local.instance_id
   region = local.region
   name = "my_DNS_CIS_config"
   cloud_internet_services_apikey = var.my_cis_apikey
   cloud_internet_services_crn = var.my_cis_crn
   }
```
{: codeblock}


### Configuring classic infrastructure with Terraform
{: #add-classic-infra-config-terraform}
{: terraform}

The following example shows a confihuration that you can use to add a classic infrastructure DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance.

```terraform
resource "ibm_sm_public_certificate_configuration_dns_classic_infrastructure" "my_dns_classic_config" {
   instance_id = local.instance_id
   region = local.region
   name = "my_DNS_config"
   classic_infrastructure_password = "username"
   classic_infrastructure_username = "password"
}
```
{: codeblock}


## Deleting a DNS provider configuration in the UI
{: #delete-dns-provider-ui}
{: ui}

If you no longer need a configuration, you can delete it by using the {{site.data.keyword.secrets-manager_short}} UI.

After you delete a configuration, the certificates that are associated with the DNS provider can no longer be rotated automatically. Do not delete configurations that are associated with certificates in your production apps or services.
{: important}

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
   
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}. 
3. In the **Secrets engines** page, click the **Public certificates** tab.
4. Use the **DNS providers section** table to view the configurations in your instance. 
5. In the row for the configuration that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.  
6. Enter the name of the configuration to confirm its deletion.
7. Click **Delete**.



## Deleting a DNS provider configuration with the API
{: #delete-dns-provider-api}
{: api}

You can delete configurations by calling the {{site.data.keyword.secrets-manager_short}} API. 

The following example shows a query that you can use to remove a DNS provider configuration from your instance. When you call the API, replace `{config_name}` with the name of the configuration that you want to delete.
{: curl}

After you delete a configuration, the certificates that are associated with the DNS provider can no longer be rotated automatically. Do not delete configurations that are associated with certificates in your production apps or services.
{: important}


```sh
curl -X DELETE 
--H "Authorization: Bearer {iam_token}" \
 "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v2/configurations/{name}"
```
{: codeblock}
{: curl}


A successful response removes the configuration from your service instance. For more information about the required and optional request parameters, see [Remove a configuration](/apidocs/secrets-manager#delete-config-element){: external}.


## Next steps
{: #manage-dns-config-next-steps}

- [Order a certificate](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates)


