---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-23"

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
| Before you add a configuration for Cloud Internet Services (CIS), be sure that you:  \n  \n - [Create a CIS service instance](/docs/cis?topic=cis-getting-started).  \n  \n - [Create an authorization between {{site.data.keyword.secrets-manager_short}} and CIS](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis).  \n  \n - If your CIS instance is located in another account, obtain the CRN of the instance and [create an API key with the correct level of access](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account). |
{: caption="Table 1. Prerequisites - CIS" caption-side="top"}
{: #cis-prereqs}
{: tab-title="Cloud Internet Services"}
{: tab-group="dns-provider-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| Before you add a configuration for classic infrastructure, be sure that you:  \n  \n - Obtain your classic infrastructure username.  \n If you are using IBMid to log in to your account, your classic infrastructure username is your `<account_id>_<email_address>`. In the console, this username is also your VPN username for the account. To find your classic infrastructure username, go to **Manage > Access (IAM) > Users > _name_ > VPN password**.  \n  \n - [Create a classic infrastructure API key](/docs/account?topic=account-classic_keys).  \n Assign your user permissions to manage DNS in the account. For more information about managing classic infrastructure access, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission). |
{: caption="Table 1. Prerequisites - Classic infrastructure" caption-side="top"}
{: #classic-infrastructure-prereqs}
{: tab-title="Classic infrastructure"}
{: tab-group="dns-provider-prereqs"}
{: class="simple-tab-table"}


## Adding a DNS provider configuration in the UI
{: #add-dns-provider-ui}
{: ui}

You can add DNS provider configurations to your service instance by using the {{site.data.keyword.secrets-manager_short}} UI. 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
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

The following example shows a query that you can use to add a Cloud Internet Services (CIS) DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance. When you call the API, replace the `cis_crn` value with the CRN of the CIS instance that contains your domains.
{: curl}

If you need to access a CIS instance that is located in another account, provide a `cis_apikey` value that contains an API key with **Manager** service access on the Internet Services (`internet-svs`) service. For more information, see [Granting service access to CIS](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account).
{: note} 



```sh
curl -X POST 'https://{instance_id}.us-south.secrets-manager.appdomain.cloud/api/v1/config/public_cert/dns_providers' \
-H 'Authorization: Bearer $IAM_token' \
-H 'Content-Type: application/json' \
-d '{
    "type": "cis",
    "name": "my-cloud-internet-services",
    "config": {
        "cis_crn": "crn:v1:bluemix:public:internet-svcs:global:a/<account-id>:<instance-id>::",
        "cis_apikey": "<api_key>"
    }
}'
```
{: codeblock}
{: curl}



A successful response adds the configuration to your service instance. For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.

### Configuring classic infrastructure
{: #add-classic-infra-config-api}

The following example shows a query that you can use to add a classic infrastructure DNS configuration to your {{site.data.keyword.secrets-manager_short}} instance. When you call the API, replace the `classic_infrastructure_username` and `classic_infastructure_password` (API key) values.
{: curl}



```sh
curl -X POST 'https://{instance_id}.us-south.secrets-manager.appdomain.cloud/api/v1/config/public_cert/dns_providers' \
-H 'Authorization: Bearer $IAM_token' \
-H 'Content-Type: application/json' \
-d '{
    "type": "classic_infrastructure",
    "name": "my-classic-infra-config",
    "config": {
        "classic_infrastructure_username": "123456_user@email.com",
        "classic_infrastructure_password": "<classic_infrastructure_api_key>"
    }
}'
```
{: codeblock}
{: curl}



A successful response adds the configuration to your service instance. For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.

## Deleting a DNS provider configuration in the UI
{: #delete-dns-provider-ui}
{: ui}

If you no longer need a configuration, you can delete it by using the {{site.data.keyword.secrets-manager_short}} UI.

After you delete a configuration, the certificates that are associated with the DNS provider can no longer be rotated automatically. Do not delete configurations that are associated with certificates in your production apps or services.
{: important}

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
   
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
curl -X DELETE 'https://{instance_id}.us-south.secrets-manager.appdomain.cloud/api/v1/config/public_cert/dns_providers/{config_name}' \
  -H 'Authorization: Bearer $IAM_token'
```
{: codeblock}
{: curl}



A successful response removes the configuration from your service instance. For more information about the required and optional request parameters, see [Remove a configuration](/apidocs/secrets-manager#delete-config-element){: external}.


## Next steps
{: #manage-dns-config-next-steps}

- [Add a certificate authority configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority)
- [Order a certificate](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates)


