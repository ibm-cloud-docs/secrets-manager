---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-16"

keywords: release notes for Secrets Manager, what's new, enhancements, fixes, improvements, Secrets Manager

subcollection: secrets-manager
content-type: release-note

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

# Release notes for {{site.data.keyword.secrets-manager_short}}
{: #release-notes}

Use these release notes to learn about the latest changes to {{site.data.keyword.secrets-manager_full}} that are grouped by date.
{: shortdesc}

For the latest changes to the APIs, check out the [{{site.data.keyword.secrets-manager_short}} API change log](/docs/secrets-manager?topic=secrets-manager-api-change-log).



## 12 September 2022
{: #secrets-manager-sept1222}
{: release-note}

CLI plug-in version 0.1.22
:   Version 0.1.22 of the {{site.data.keyword.secrets-manager_short}} CLI is now available for download. To learn about the latest updates, see the [CLI change log](/docs/secrets-manager?topic=secrets-manager-cli-change-log).

New custom metadata fields
:   The documentation for creating secrets and managing versions of a secret is now updated to include `custom_metadata` and `version_custom_metadata` fields. To learn more about the latest updates, see [Creating secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui) and [Managing secret versions](/docs/secrets-manager?topic=secrets-manager-version-history&interface=ui).

New {{site.data.keyword.secrets-manager_short}} event notifications
:   The following notifications for events related to Secret Locks are now available.

   - `secret_deletion_blocked`: Now available for arbitrary secrets, IAM credentials, user credentials, key value, imported certificates, private certificates, and public certificates.
   - `secret_revocation_blocked`: Now available for private certificates. 
   - `secret_rotation_blocked`:  Now available for arbitrary secrets, IAM credentials, user credentials, key value, imported certificates, private certificates, and public certificates.
   - `secret_expiration_blocked`:  Now available for arbitrary secrets, IAM credentials, and user credentials.
   - `secret_version_data_deleted`:  Now available for arbitrary secrets, IAM credentials, user credentials, key value, imported certificates, private certificates, and public certificates.

   For more information about using {{site.data.keyword.en_short}} to enable lifecycle notifications for your {{site.data.keyword.secrets-manager_short}} instance, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).


## 1 August 2022
{: #secrets-manager-aug0122}
{: release-note}

Improved authorization flow
:   The documentation for granting service access to domains is now updated to simplify the required flow. To try out the latest updates, see [Granting service access to CIS](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#authorize-domains).



## 27 July 2022
{: #secrets-manager-jul2722}
{: release-note}

CLI plug-in version 0.1.21
:   Version 0.1.21 of the {{site.data.keyword.secrets-manager_short}} CLI is now available for download. To learn about the latest updates, see the [CLI change log](/docs/secrets-manager?topic=secrets-manager-cli-change-log).


## 10 July 2022
{: #secrets-manager-jul1022}
{: release-note}

Lock secrets to prevent them from being deleted
:   You can now attach locks to secrets to indicate that they're in use by one or more consumers. When a secret is locked, it can't be modified or deleted. For more information, check out the following resources:

    - [Locking secrets](/docs/secrets-manager?topic=secrets-manager-secret-locks)
    - [Best practices for rotating and locking secrets](/docs/secrets-manager?topic=secrets-manager-best-practices-rotate-secrets)

CLI plug-in version 0.1.20
:   Version 0.1.20 of the {{site.data.keyword.secrets-manager_short}} CLI is now available for download. To learn about the latest updates, see the [CLI change log](/docs/secrets-manager?topic=secrets-manager-cli-change-log).


## 8 June 2022
{: #secrets-manager-jun822}
{: release-note}

Allow access to {{site.data.keyword.secrets-manager_short}} in a restricted account
:   Working in an account that has [IP address access restrictions](/docs/account?topic=account-ips)? To use certain features in {{site.data.keyword.secrets-manager_short}}, for example, to generate IAM credentials, an extra step is required to ensure that your account is able to accept incoming requests from the service. If your account allows access to only specific IP addresses, you can update your account settings to specify a required list of IP addresses for {{site.data.keyword.secrets-manager_short}}.

    For more information, check out [IP addresses for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-ip-addresses).

## 27 May 2022
{: #secrets-manager-may2722}
{: release-note}

Integration with {{site.data.keyword.alb_full}}
:   You can now use {{site.data.keyword.secrets-manager_short}} to centrally manage the SSL/TLS certificates that are required for load balancers to perform SSL offloading tasks. To learn more about this integration, check out the [{{site.data.keyword.alb_full}} documentation](/docs/vpc?topic=vpc-load-balancers#ssl-offloading-and-required-authorizations){: external}.


## 16 May 2022
{: #secrets-manager-may1622}
{: release-note}

Updated {{site.data.keyword.secrets-manager_short}} event notifications
:   Previously, notifications for {{site.data.keyword.secrets-manager_short}} were supported for certificates only. Now, you can be notified when other types of secrets in your instance are expired or soon to expire. 

  - `secret_about_to_expire`: Now available for arbitrary secrets, IAM credentials, and user credentials.
  - `secret_expired`: Now available for arbitrary secrets, IAM credentials, and user credentials.

    For more information about using {{site.data.keyword.en_short}} to enable lifecycle notifications for your {{site.data.keyword.secrets-manager_short}} instance, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).

CLI plug-in version 0.1.19
:   Version 1.1.19 of the {{site.data.keyword.secrets-manager_short}} CLI is now available for download. To learn about the latest updates, see the [CLI change log](/docs/secrets-manager?topic=secrets-manager-cli-change-log).


## 25 April 2022
{: #secrets-manager-apr2522}
{: release-note}

Create private SSL/TLS certificates for your applications
:   You can now use {{site.data.keyword.secrets-manager_short}} to set up a private certificate authority (CA) that you can use to issue SSL/TLS certificates to your internal applications. By configuring root and intermediate CAs for your instance, you can establish a valid chain of trust for your certificates. To find out more about this release, [check out the announcement blog](https://www.ibm.com/cloud/blog/announcements/create-private-tls-certificates-with-ibm-cloud-secrets-manager){: external}.

  - [Preparing to create private certificates](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates)
  - [Creating root certificate authorities](/docs/secrets-manager?topic=secrets-manager-root-certificate-authorities)
  - [Creating intermediate certificate authorities](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities)
  - [Adding certificate templates](/docs/secrets-manager?topic=secrets-manager-certificate-templates)
  - [Creating private certificates](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates)

New {{site.data.keyword.secrets-manager_short}} event notifications
:   The following {{site.data.keyword.secrets-manager_short}} events can now be forwarded to a connected {{site.data.keyword.en_short}} instance.

  - `secret_deleted`: Currently supported for imported certificates, private certificates, and public certificates.
  - `secret_revoked`: Currently supported for private certificates only.

    For more information about using {{site.data.keyword.en_short}} to enable lifecycle notifications for your {{site.data.keyword.secrets-manager_short}} instance, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).

CLI plug-in version 1.1.18
:   Version 1.1.18 of the {{site.data.keyword.secrets-manager_short}} CLI is now available for download. To learn about the latest updates, see the [CLI change log](/docs/secrets-manager?topic=secrets-manager-cli-change-log).


## 11 April 2022
{: #secrets-manager-apr1122}
{: release-note}

Integrations with {{site.data.keyword.containershort}} and {{site.data.keyword.openshiftshort}}
:   You can now use {{site.data.keyword.secrets-manager_short}} to centrally manage Ingress subdomain certificates and other secrets for your Kubernetes or {{site.data.keyword.openshiftshort}} clusters. To learn more about this integration, check out the [{{site.data.keyword.containershort}}](/docs/containers?topic=containers-ingress-types#manage_certs_secrets_mgr){: external} or [{{site.data.keyword.openshiftshort}} documentation](/docs/openshift?topic=openshift-ingress-roks4#manage_certs_secrets_mgr){: external}.

## 23 March 2022
{: #secrets-manager-mar2322}
{: release-note}

Standard and Trial pricing plans
:   You can now create an unlimited number of {{site.data.keyword.secrets-manager_short}} service instances by choosing the Standard pricing plan. For more information about pricing in {{site.data.keyword.secrets-manager_short}}, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-secrets-manager-new-pricing-model){: external}.

   If you're an existing {{site.data.keyword.secrets-manager_short}} Lite plan user, be sure to upgrade your instance to the Standard plan prior to 22 May 2022 if you'd like to maintain access. For more information, see the previous [release note](#secrets-manager-feb2322).
   {: important}

Osaka and Toronto availability
:   You can now create a {{site.data.keyword.secrets-manager_short}} service instance in the Osaka (`jp-osa`) and Toronto (`ca-tor`) regions.

   For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints).


## 15 March 2022
{: #secrets-manager-mar1522}
{: release-note}

Tutorial series: Set up alerts for expiring certificates
:   Looking for examples that can help you to send {{site.data.keyword.secrets-manager_short}} notifications to GitHub or Slack? Check out the new [tutorial series](/docs/secrets-manager?topic=secrets-manager-tutorial-expiring-secrets-part-1) that guides you through:

   - Enabling [{{site.data.keyword.en_short}}](/docs/event-notifications) for {{site.data.keyword.secrets-manager_short}}
   - Using [Cloud Functions](/docs/openwhisk) to create a GitHub issue when a secret is about to expire
   - Using Cloud functions to sending a notification to Slack

## 28 February 2022
{: #secrets-manager-feb2822}
{: release-note}

Sao Paulo availability
:   You can now create a {{site.data.keyword.secrets-manager_short}} service instance in the Sao Paulo (`br-sao`) region.

   For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints).

## 23 February 2022
{: #secrets-manager-feb2322}
{: release-note}

Pricing plan updates coming soon in {{site.data.keyword.secrets-manager_short}}
:   On 23 March 2022, {{site.data.keyword.secrets-manager_short}} will introduce Standard and Trial pricing plans. With the introduction of the new plans, the current Lite plan option will be deprecated. 

   **Types of plans:**

   When you provision an instance of the service after the 23 March, you can choose either a Trial or Standard plan.


   * **Trial**: To try out the service, you can provision an instance of the service to access all the features that {{site.data.keyword.secrets-manager_short}} offers for a limited time. After the trial period, all functionality is disabled but the instance remains in your account for an additional 30 days, during which you can choose to upgrade your plan. If you choose not to, the instance and its data are automatically removed from your account without any action on your part.

   You can have one instance of the service on the trial plan that is provisioned in your account at any time.
   {: note}

   * **Standard**: When you're ready to upgrade, you get unlimited access to all the features that the service has to offer. The number of instances that your teams can provision is unlimited. With the Standard plan, you are charged per secret and per instance that is provisioned. To view the most current pricing information, see the {{site.data.keyword.secrets-manager_short}} UI.


   **Important dates:**

   Be sure to keep the following dates in mind:


   * **23 March 2022**: Instances of {{site.data.keyword.secrets-manager_short}} on the Lite plan will be deprecated. As an existing user, you can continue to use the service without interruption, but you should upgrade your instances to the Standard plan as soon as you can after this date.


   * **22 May 2022**: Instances of {{site.data.keyword.secrets-manager_short}} on the Lite plan will be disabled and functionality is removed. However, the instance remains in your account for an additional 30 days, during which you can choose to upgrade your plan.


   * **21 June 2022**: If you have chosen not to upgrade your plan, the instances of the service and their data will be removed from your account.


   Users will have the option to upgrade their service starting 23 March 2022. To ensure that your service functionality is not disrupted, be sure to upgrade to a Standard plan prior to 22 May 2022.
   {: important}


## 3 February 2022
{: #secrets-manager-feb0322}
{: release-note}

Enable lifecyle notifications for your certificates
:   You can now integrate with the [{{site.data.keyword.en_short}}](/catalog/services/event-notifications){: external} service so that you can manage and route all of your {{site.data.keyword.secrets-manager_short}} alerts to your preferred destinations.

   Currently, {{site.data.keyword.secrets-manager_short}} supports notifications for certificates only. For more information, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).
   {: note}


## 31 January 2022
{: #secrets-manager-jan3122}
{: release-note}

Store key-value secrets
:   You can now create key-value secrets with the {{site.data.keyword.secrets-manager_short}} UI and API. For more information, see [Storing key-value secrets](/docs/secrets-manager?topic=secrets-manager-key-value&interface=ui).

## 22 November 2021
{: #secrets-manager-nov2221}
{: release-note}

View the version history of your secrets
:   With the {{site.data.keyword.secrets-manager_short}} UI, you can now check the version history of your secrets so that you can quickly understand when a secret was last rotated. For more information, see [Viewing your version history](/docs/secrets-manager?topic=secrets-manager-version-history).

Create IAM credentials by using a service ID in your account
:   In addition to using access groups to determine the access capabilities of an IAM credential, you can now create a secret by using an existing service ID in your account. You can choose this option if you need {{site.data.keyword.secrets-manager_short}} to dynamically generate and manage an API key only.

   For more information, see [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials).

Restore secrets to a previous version
:   Need to roll back a secret to its previous version? If you need to revert a secret, you can now restore select secret types to a previous version.

   Currently, you can restore 1 version back for IAM credentials and public certificate secrets only. For more information, see [Restoring secrets to a previous version](/docs/secrets-manager?topic=secrets-manager-version-history&interface=ui#restore-secrets).
   {: note}

Rotate IAM credentials on-demand
:   You can now rotate IAM credentials manually by using the UI or APIs. For more information, see [Rotating secrets manually](/docs/secrets-manager?topic=secrets-manager-manual-rotation).


## 20 September 2021
{: #secrets-manager-sept2021}
{: release-note}

Order domain-validated TLS certificates from Let's Encrypt
:   In addition to [importing your existing certificates](/docs/secrets-manager?topic=secrets-manager-certificates), you can now use {{site.data.keyword.secrets-manager_short}} to order domain-validated certificates! 

   In this release, the following certificate authorities (CA) and DNS providers are supported:

   - Let's Encrypt
   - {{site.data.keyword.cloud_notm}} Internet Services (CIS)
   - {{site.data.keyword.cloud_notm}} classic infrastructure (SoftLayer)

   For more information, see [Ordering domain-validated certificates from third-parties](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates) or check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/now-available-order-domain-validated-tls-certificates-with-single-tenant-ibm-cloud-secrets-manager){: external}.


## 30 August 2021
{: #secrets-manager-aug3021}
{: release-note}

Migrate certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}
:   Ready to try {{site.data.keyword.secrets-manager_short}} for certificates management? You can now take advantage of automation scripts that can help you to move certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.

   For more information, check out the [migration scripts in GitHub](https://github.com/ibm-cloud-security/certificate-manager-to-secrets-manager){: external}. 


## 20 June 2021
{: #secrets-manager-jun2021}
{: release-note}

Import and manage your existing TLS certificates
:   Looking for the ability to centralize TLS certificates into a single secrets management service? You can now use {{site.data.keyword.secrets-manager_short}} to import TLS certificates that are issued by external certificate authorities (CA). 

   For more information, check out [Importing TLS certificates](/docs/secrets-manager?topic=secrets-manager-certificates). 

Connect to {{site.data.keyword.secrets-manager_short}} from your VPC network
:   You can now connect to a {{site.data.keyword.secrets-manager_short}} service instance by using Virtual Private Endpoints (VPE) for VPC. 

   To learn how to connect your existing {{site.data.keyword.secrets-manager_short}} instance, see [Securing your connection to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-virtual-private-endpoint). For more information about setting up a virtual private endpoint gateway, check out [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

## 19 May 2021
{: #secrets-manager-may1921}
{: release-note}

Upcoming updates to supported cipher suites
:   On 29 May 2021, {{site.data.keyword.secrets-manager_short}} will deliver changes to the cipher suites that it supports for TLS connections to the service. This update is being implemented to enhance the security of IBM Cloud users and protect user data.

   - **What's changing?** Beginning 29 May 2021, {{site.data.keyword.secrets-manager_short}} API endpoints will allow only the following cipher suites:

      - `ECDHE-ECDSA-AES128-GCM-SHA256`
      - `ECDHE-ECDSA-CHACHA20-POLY1305`
      - `ECDHE-ECDSA-AES256-GCM-SHA384`
      - `ECDHE-RSA-AES128-GCM-SHA256`
      - `ECDHE-RSA-CHACHA20-POLY1305`
      - `ECDHE-RSA-AES256-GCM-SHA384`

   - **How will this change impact my environment?** This change will impact clients that are configured to use ciphers that are not included on this list. To avoid connectivity issues with {{site.data.keyword.secrets-manager_short}}, make sure that your client is configured to use only the allowed list of ciphers in TLS connections to the service. Reach out to [IBM Cloud support](https://cloud.ibm.com/unifiedsupport/cases/form) with any questions.

Manage secrets in your {{site.data.keyword.contdelivery_short}} toolchain
:   You can now configure {{site.data.keyword.secrets-manager_short}} to securely manage secrets that are part of your {{site.data.keyword.contdelivery_short}} toolchain.

   For more information, check out the [announcement blog](https://www.ibm.com/cloud/blog/manage-secrets-in-continuous-delivery-with-ibm-cloud-secrets-manager){: external}.

## 22 March 2021
{: #secrets-manager-mar2221}
{: release-note}

Manage secrets over a private network connection
:   You can now create a private instance of {{site.data.keyword.secrets-manager_short}} so that you can manage application secrets over a private network connection.

   For more information, see [Securing your connection to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).

Reuse API keys for IAM credential secrets
:   By default, IAM credential secrets are generated and deleted each time that the secret is read or accessed. By using the `reuse_api_key` parameter in the API, or the **Reuse IAM credentials** option in the UI, you can control whether the associated service ID API key can be reused until the secret expires.

   For more information, see [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials#iam-credentials-ui).

Analyze {{site.data.keyword.secrets-manager_short}} logs with {{site.data.keyword.la_full_notm}}
:   {{site.data.keyword.secrets-manager_short}} is now integrated {{site.data.keyword.la_short}} so that you can diagnose issues and analyze logs that are generated by your {{site.data.keyword.secrets-manager_short}} service instance.

   For more information, see [Viewing logs for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-logs).

London, Tokyo, and Washington DC availability
:   {{site.data.keyword.secrets-manager_short}} is now available in the London (`eu-gb`), Tokyo (`jp-tok`), and Washington DC (`us-east`) regions.

   For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints).

## 15 February 2021
{: #secrets-manager-feb1521}
{: release-note}

Now available: {{site.data.keyword.secrets-manager_short}} Java SDK
:   You can now use the {{site.data.keyword.secrets-manager_full_notm}} Java SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

   For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Java SDK repository in GitHub](https://github.com/IBM/secrets-manager-java-sdk){: external}.

## 27 January 2021
{: #secrets-manager-jan2721}
{: release-note}

Now available: {{site.data.keyword.secrets-manager_short}} Go and Python SDKs
:   You can now use the {{site.data.keyword.secrets-manager_full_notm}} Go and Python SDKs to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

   For more information, check out the SDK repositories in GitHub:
   
   - [{{site.data.keyword.secrets-manager_full_notm}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}
   - [{{site.data.keyword.secrets-manager_full_notm}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}


## 18 December 2020
{: #secrets-manager-dec1820}
{: release-note}

Announcing {{site.data.keyword.secrets-manager_short}} general availability
:   {{site.data.keyword.secrets-manager_short}} is now generally available in the {{site.data.keyword.cloud_notm}} catalog!

   To find out more about this release, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-secrets-manager-is-now-generally-available){: external}.

## 15 December 2020
{: #secrets-manager-dec1520}
{: release-note}

Sydney availability
:   You can now create a {{site.data.keyword.secrets-manager_short}} service instance in the Sydney (`au-syd`) region.

   For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints).

## 14 December 2020
{: #secrets-manager-dec1420}
{: release-note}

Now available: {{site.data.keyword.secrets-manager_short}} CLI plug-in
:   The {{site.data.keyword.secrets-manager_short}} CLI plug-in is now available for download!

   You can use the {{site.data.keyword.secrets-manager_short}} CLI to interact with the secrets that you store in your {{site.data.keyword.secrets-manager_short}} instance. To install the plug-in, log in to the IBM Cloud CLI and run `ibmcloud plugin install secrets-manager`.

   - To see CLI usage examples for creating secrets of different types, check out [Creating secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets).
   - To find out more about the CLI commands and options that are available for {{site.data.keyword.secrets-manager_short}}, see the [CLI reference](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli).

## 24 November 2020
{: #secrets-manager-nov2420}
{: release-note}

Now available: {{site.data.keyword.secrets-manager_short}} Node.js SDK
:   You can now use the {{site.data.keyword.secrets-manager_full_notm}} Node.js SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

   For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Node.js SDK repository in GitHub](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}.

## 13 November 2020
{: #secrets-manager-nov1320}
{: release-note}

Now available: {{site.data.keyword.cloud_notm}} plug-ins for Vault
:   Need to manage {{site.data.keyword.cloud_notm}} secrets by using an on-premises Vault? You can now integrate stand-alone {{site.data.keyword.cloud_notm}} plug-ins for Vault. These open source plug-ins can be used independently from each other so that you can manage {{site.data.keyword.cloud_notm}} secrets through your on-premises Vault server.

   - To set up authentication between Vault and your {{site.data.keyword.cloud_notm}} account, you can use the [{{site.data.keyword.cloud_notm}} Auth Method plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-auth-ibmcloud){: external}.
   - To dynamically create API keys for {{site.data.keyword.cloud_notm}} service IDs, you can use the [{{site.data.keyword.cloud_notm}} Secrets Backend plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-secrets-ibmcloud){: external}.

   For more information, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/hashicorp-enterprise-vault-plugins-for-ibm-cloud){: external}

## 9 November 2020
{: #secrets-manager-nov0920}
{: release-note}

Grant controlled access to secret data with the `SecretsReader` role
:   You can now choose between the Reader and SecretsReader IAM roles for better control over access to the payload of your secrets.

   - As a reader, you can browse a high-level view of secrets in your instance. Readers can't access the payload of a secret.
   - As a secrets reader, you can browse a high-level view of secrets, and you can access the payload of a secret. A secrets reader can't create secrets or modify the value of an existing secret.

   To learn more about service access roles, see [Managing IAM access for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-iam).


## 24 September 2020
{: #secrets-manager-sept2420}
{: release-note}

Introducing {{site.data.keyword.secrets-manager_short}}
:   With {{site.data.keyword.secrets-manager_full_notm}}, you can create secrets dynamically and lease them to applications while you control access from a single location. Built on open source HashiCorp Vault, {{site.data.keyword.secrets-manager_short}} helps you get the data isolation of a dedicated environment with the benefits of a public cloud.

   In this release, {{site.data.keyword.secrets-manager_short}} offers support for the following types of secrets:

   - IAM credentials, which consist of a service ID and API key that are generated dynamically on your behalf.
   - Arbitrary secrets, such as custom credentials that can be used to store any type of structured or unstructured data.
   - User credentials, such as usernames and passwords that you can use to log in to applications.

   To find out more about capabilities and use cases for {{site.data.keyword.secrets-manager_short}}, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/introducing-ibm-cloud-secrets-manager-beta){: external}.


