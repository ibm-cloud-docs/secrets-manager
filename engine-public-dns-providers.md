---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-19"

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

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam). To configure your DNS provider manually, be sure that you [Create a certificate authority configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority&interface=ui).

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
{: tab-title="Custom"}
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



### Ordering public certificates with a manual DNS provider by using the API
{: #manual-dns-provider-create-api}
{: api}

 To create a public certificate by using a manual DNS provider, complete the following steps.

1. Create a certificate authority (CA) configuration by following the steps that are defined in [Adding a CA configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority&interface=ui).
2. Create a new public certificate by specifying `manual` as your DNS configuration.

   ```sh
   curl -X POST 'https://{instance_id}.us-south.secrets-manager.appdomain.cloud/api/v1/secrets/public_cert' \
   -H 'accept: application/json' \
   -H 'Authorization: Bearer $IAM_token' \
   -H 'Content-Type: application/json' \
   -d '{
   "metadata": {
      "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
      "collection_total": 1
   },
   "resources": [
      {
         "name": "my-public-certificate",
         "description": "Description for ordered certificate.",
         "ca": "ca_config_name",
         "dns": "manual",
         "common_name": "domain1.com",
         "alt_names": [
         "domain2.com",
         "domain3.com"
         ],
         "bundle_certs": false,
         "key_algorithm": "RSA2048",
         "rotation": {
         "auto_rotate": false,
         "rotate_keys": false
         }
      }
   ]
   }'
   ```
   {: codeblock}
   {: curl}

   Example response:

   ```json
   "metadata": {
   "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
   "collection_total": 1
   },
   "resources": [
      {
         "alt_names": [
         "domain2",
         "domain3"
         ],
         "common_name": "domain1",
         "created_by": "User",
         "creation_date": "2022-09-13T06:21:33Z",
         "crn": "secret crn",
         "description": "Description for ordered certificate.",
         "downloaded": false,
         "id": "38747ae6-8c69-d745-5276-cdf3157b9021",
         "issuance_info": {
         "auto_rotated": false,
         "bundle_certs": false,
         "ca": "ca_config_name",
         "challenges": [
            {
               "domain": "domain1",
               "expiration": "2022-09-20T06:21:36Z",
               "status": "pending",
               "txt_record_name": "_acme-challenge.domain1.",
               "txt_record_value": "TA6J7fFYrwP3Jg-S_IAQSj2Ydqfw4Ycm4sMwlzuCcxk"
            },
            {
               "domain": "domain2",
               "expiration": "2022-09-20T06:21:36Z",
               "status": "pending",
               "txt_record_name": "_acme-challenge.domain2.",
               "txt_record_value": "qSDrCkFAViX4xANKuEPcMNairWm1PUtROm6kp9bmSS0"
            },
            {
               "domain": "domain3",
               "expiration": "2022-09-20T06:21:36Z",
               "status": "pending",
               "txt_record_name": "_acme-challenge.domain3.",
               "txt_record_value": "8dcgan91fW6aK3aIhPAVZRkHpbYEoMcCNPpVh1n4tSA"
            }
         ],
         "dns": "manual",
         "ordered_on": "2022-09-13T06:21:33Z",
         "state": 0,
         "state_description": "Pre-activation"
         },
         "key_algorithm": "RSA2048",
         "labels": [],
         "last_update_date": "2022-09-13T06:21:33Z",
         "locks_total": 0,
         "name": "my-public-certificate",
         "rotation": {
         "auto_rotate": false,
         "rotate_keys": false
         },
         "secret_type": "public_cert",
         "state": 0,
         "state_description": "Pre-activation",
         "versions": [],
         "versions_total": 1
      }
   ]
   }
   ```
   {: codeblock}

3. Complete the challenges that are marked as `pending` before they expire by adding the TXT records that are specified in the challenge to your domain in your DNS provider account to verify your ownership of the domain.

   If you order a certificate for subdomains, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
   {: note}

4. Validate that the TXT records that you added are propagated. Depending on your DNS provider, it can take some time to complete.

5. After the records are propagated, call the {{site.data.keyword.secrets-manager_short}} [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) API to request Let's Encrypt to validate the challenges to your domain and create a public certificate. 

   ```sh
   curl -X POST --location --header "Authorization: Bearer {iam_token}"   --header "Accept: application/json"   --header "Content-Type: application/json"   "{base_url}/api/v1/secrets/{secret_type}/{id}?action=validate_dns_challenge"
   ```
   {: codeblock}
   {: curl}

   If you need to update your certificate later, you can use the same [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) API but with the action `rotate`. However, you can't automatically rotate manual DNS provider certificates in {{site.data.keyword.secrets-manager_short}}.
   {: note}

6. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.


Want to automate the creation of your public certificates? If your domains are configured through a DNS provider, you can create a script to complete the challenges. Some DNS providers offer an API that checks whether the new records are fully transmitted. If your DNS provider doesn't offer this option, you can configure your client to wait for a specified amount of time, sometimes up to an hour. In {{site.data.keyword.secrets-manager_short}}, you can check the status of the certificate issuance by obtaining your certificate metadata. When the `IssuanceInfo.State` field that is returned changes to `active`, the certificate is issued. 
{: tip}


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
curl -X DELETE 'https://{instance_id}.us-south.secrets-manager.appdomain.cloud/api/v1/config/public_cert/dns_providers/{config_name}' \
  -H 'Authorization: Bearer $IAM_token'
```
{: codeblock}
{: curl}

A successful response removes the configuration from your service instance. For more information about the required and optional request parameters, see [Remove a configuration](/apidocs/secrets-manager#delete-config-element){: external}.


## Next steps
{: #manage-dns-config-next-steps}

- [Order a certificate](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates)


