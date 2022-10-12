---

copyright:
  years: 2020, 2022
lastupdated: "2022-10-12"

keywords: set up public certificates, public certificates engine, set up CIS, set up CA, set up Let's Encrypt

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

# Preparing to order public certificates
{: #prepare-order-certificates}

You can enable your {{site.data.keyword.secrets-manager_full}} service instance to order certificates by configuring the public certificates engine.
{: shortdesc}

In {{site.data.keyword.secrets-manager_short}}, the public certificates engine serves as the back end for the `public_cert` secret type. Public certificates are domain-validated TLS certificates that you can order and manage in the service. Before you can order a certificate, you must enable your service instance by connecting supported certificate authorities (CA) and DNS providers.

[Ordering a certificate](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates) through {{site.data.keyword.secrets-manager_short}} is an asynchronous process that can take a few minutes to complete.
{: note}



## Supported certificate authorities
{: #connect-certificate-authority}

A certificate authority (CA) is an entity that issues digital certificates. You can connect the following certificate authorities with your {{site.data.keyword.secrets-manager_short}} service instance.

| Authority | Description |
| --------- | ----------- | 
| [Let's Encrypt](https://letsencrypt.org/){: external} | Let’s Encrypt is a free, automated, ACME-based certificate authority that provides domain validated certificates valid for 90 days. It is a service that is provided by the Internet Security Research Group (ISRG). |
{: caption="Table 1. Certificate authority options" caption-side="top"}



### Creating a Let's Encrypt ACME account
{: #create-acme-account}

To connect with Let's Encrypt, {{site.data.keyword.secrets-manager_short}} uses the [Automatic Certificate Management Environment (ACME)](https://datatracker.ietf.org/doc/html/rfc8555){: external} protocol. The ACME protocol makes it possible to automatically obtain browser trusted certificates from a certificate authority without human intervention.

You can grant service access to Let's Encrypt by registering an ACME account and providing your account credentials. If you have a working ACME client or account for Let's Encrypt, you can use your existing private key. If you don't have an account yet, you can create one by using the [ACME account creation tool](https://github.com/ibm-cloud-security/acme-account-creation-tool){: external}.

Certificate authorities can apply a charge when you are ordering or renewing a certificate. Additionally, various rate limits apply. {{site.data.keyword.secrets-manager_short}} does not control costs or rate limits that are associated with ordering certificates. For more information about rate limits to keep in mind as you order Let's Encrypt certificates, check out the [Let's Encrypt documentation](https://letsencrypt.org/docs/rate-limits/){: external}.
{: note} 


## Supported DNS providers
{: #connect-dns-provider}

A DNS provider is the service that is used to manage the domains that you own. You can connect the following DNS providers with your {{site.data.keyword.secrets-manager_short}} service instance.

| DNS provider | Description |
| --------- | ----------- | 
| [{{site.data.keyword.cis_full_notm}}](https://{DomainName}/catalog/services/internet-services) | {{site.data.keyword.cis_full}} (CIS), powered by Cloudflare, provides a fast, highly performant, reliable, and secure internet service for customers who are running their business on {{site.data.keyword.cloud_notm}}. |
| [{{site.data.keyword.cloud_notm}} classic infrastructure](https://{DomainName}/catalog/infrastructure/domain_registration)  | [{{site.data.keyword.cloud}} Domain Name Registration](/docs/dns), available as part of {{site.data.keyword.cloud_notm}} classic infrastructure (SoftLayer), offers a central location from which to view and manage domains. |
| [Manual DNS providers](/docs/secrets-manager?topic=secrets-manager-add-dns-provider&interface=api#manual-dns-provider-create-api) | If your DNS provier is not IBM Cloud Internet Services or IBM Cloud Domain Name Registration, you can connect your {{site.data.keyword.secrets-manager_short}} to your DNS provider manually.|
{: caption="Table 2. DNS provider options" caption-side="top"}

### Granting service access to CIS
{: #authorize-cis}

If you manage your domains in {{site.data.keyword.cis_short}}, you must assign access to {{site.data.keyword.secrets-manager_short}} so that it can validate the ownership. To authorize {{site.data.keyword.secrets-manager_short}} to manage a {{site.data.keyword.cis_short_notm}} instance and its domains, you can [create an authorization between the services](/docs/account?topic=account-serviceauth)if your instances are located in the same account.

If you're working with a CIS instance that is located in another account, you can use an API key to manage access. For more information, see [Granting service access by using an API key](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-cis-another-account).
{: note}

#### Granting service access to CIS
{: #authorize-domains}

You can grant {{site.data.keyword.secrets-manager_short}} the ability to access your CIS instance and all of its domains by creating a service authorization between the services. Both your {{site.data.keyword.secrets-manager_short}} and CIS instance must be in the same account.

To create a service authorization, you can use the **Access (IAM)** section of the console.

![The figure shows a simplified IAM dashboard with numbered steps for creating an authorization between Secrets Manager and Cloud Internet Services. The steps are described in the following text.](images/create-service-auth.svg){: caption="Figure 1. Creating a service authorization between Secrets Manager and CIS" caption-side="bottom"}

1. In the console, click **Manage > Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source account for the authorization.
4. Select a source and target service for the authorization.
   
    1. From the **Source service** list, select {{site.data.keyword.secrets-manager_short}}.
    2. From the **Target service** list, select Internet Services.
5. Specify a service instance for both the source and the target.
6. Select the **Manager** role. With these permissions, your {{site.data.keyword.secrets-manager_short}} instance can manage the {{site.data.keyword.cis_short_notm}} instance and its domains.
7. Optional: To grant access to a specific domain, select **Resources based on selected attributes** and provide the **Domain ID** for the CIS instance.

   For production environments, it is recommended that you assign access only to the specific domains.
   {: note}
   
8. Click **Authorize**
9. Complete the steps to [add a certificate authority configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority) to your {{site.data.keyword.secrets-manager_short}} instance. 


#### Granting service access for another account
{: #authorize-cis-another-account}

If the CIS instance that you'd like to access is located in another account, you can create an authorization between the services by providing an API key. You need the Cloud Resource Name (CRN) of the CIS instance that contains your domains, and an API key with the correct level of access to your instance. The API key must grant {{site.data.keyword.secrets-manager_short}} the ability to view the CIS instance, access its domains, and manage TXT records.

If the CIS instance is located in an account that allows access to only specific IP addresses, you must also update the IP address restrictions in the account to allow incoming requests from {{site.data.keyword.secrets-manager_short}}. For more information, see [IP addresses for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-ip-addresses).
{: important}

To assign access, you can use the **Access (IAM)** section of the console.

1. Log in to the account in which your CIS instance is located.
2. Click **Manage > Access (IAM)**, and select **Service IDs**.
3. [Create a service ID API key](/docs/account?topic=account-serviceidapikeys) or select an existing one.
4. Assign the required access to view the CIS instance, access its domains, and manage TXT records.
   
   1. In the row of the service ID, click the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> Assign access**.
   2. Click the **Access policy** tile.
   3. From the list of services, select **Internet Services** and click **Next**.
   4. Select **Resources based on selected attributes**.
   5. In the **Service instance** field, select your CIS instance.
   6. In the Roles and actions section, select the **Manager** role. If you want to grant the service ID the ability to access the CIS instance from the Resource list in the {{site.data.keyword.cloud_notm}} console, you can also assign the **Viewer** platform role.
   7. Click **Review > Add > Assign** to complete the access assignment.
5.  Complete the steps to [add a DNS configuration](/docs/secrets-manager?topic=secrets-manager-add-dns-provider) to your {{site.data.keyword.secrets-manager_short}} instance. 


### Granting service access to classic infrastructure
{: #authorize-classic-infrastructure}

If you manage domains by using classic infrastructure, you must grant service access to its DNS service so that {{site.data.keyword.secrets-manager_short}} can validate the ownership of your domains. You need your classic infrastructure account credentials before you can grant access.

To obtain your classic infrastructure username and API key, you can use the **Access (IAM)** section of the console.

You can view and access your classic infrastructure credentials from the **Access (IAM)** section of the console only if you are a classic infrastructure user. If you do not have classic infrastructure access, the VPN username and classic infrastructure API key fields do not display on the page. For more information, see [Managing classic infrastructure access](/docs/account?topic=account-mngclassicinfra).
{: important}

![The figure shows a simplified IAM dashboard with numbered steps for viewing your classic infrastructure username and API key. The steps are described in the following text.](images/classic-infra-creds.svg){: caption="Figure 2. Viewing your classic infrastructure username and API key" caption-side="bottom"}

1. In the console, go to **Manage > Access (IAM) > Users**, then select the user's name.
2. In the VPN password section, copy the **Username** value.

   In most cases, your classic infrastructure username is your `<account_id>_<email_address>`. This username is also your VPN username for the account.

3. In the API keys section, [create a classic infrastructure API key](/docs/account?topic=account-classic_keys) or find your existing key.
4. Click the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> Details** to copy the API key value.
5. Assign your user permissions to manage DNS in the account.

   For more information about managing classic infrastructure access, see [Classic infrastructure permissions](/docs/account?topic=account-mngclassicinfra#how-classic-infra-permissions-work).

   1. Click the **Classic infrastructure** tab to manage your classic infrastructure permissions.
   2. In the Services section, ensure that the **Manage DNS** permission is selected.
6. Complete the steps to [add a DNS configuration](/docs/secrets-manager?topic=secrets-manager-add-dns-provider) to your {{site.data.keyword.secrets-manager_short}} instance.


## Next steps
{: #prepare-order-certificates-next-steps}

Now you're ready to add engine configurations to your instance.

First, [add a certificate authority configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority) then, [add a DNS provider configuration](/docs/secrets-manager?topic=secrets-manager-add-dns-provider).

