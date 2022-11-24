---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-24"

keywords: migrate from {{site.data.keyword.cloudcerts_short}}, migrate to Secrets Manager, migrate certificates

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


# Migrating certificates from {{site.data.keyword.cloudcerts_short}}
{: #migrate-from-certificate-manager}

With {{site.data.keyword.secrets-manager_full}}, you can centralize your application secrets in a single service, including the SSL/TLS certificates that you might already store and manage in {{site.data.keyword.cloudcerts_long_notm}}. Learn about suggested guidelines for moving your certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.
{: shortdesc}


As of 10 February 2022, {{site.data.keyword.cloudcerts_long_notm}} is deprecated. The strategic alternative for managing certificates in {{site.data.keyword.cloud_notm}} is {{site.data.keyword.secrets-manager_short}}. For more information, see the [deprecation announcement](/docs/certificate-manager?topic=certificate-manager-release-notes#certificate-manager-feb1022).
{: note}

## Migration timeline
{: #migrate-time}

If you are still actively working with {{site.data.keyword.cloudcerts_short}}, be sure to start your migration as soon as possible. As you're evaluating what migration entails, keep the following dates in mind.


### 23 September 2022: Auto-provisioning ends for Kubernetes Service
{: #migrate-end-auto-provision}

{{site.data.keyword.cloudcerts_short}} is no longer to be provisioned automatically for each new cluster, but all managed default Ingress secrets continue to be written directly to the cluster. Note: if you are working with Kubernetes Service, check out the [How to migrate certificates](https://www.ibm.com/cloud/blog/how-to-migrate-certificates-from-ibm-certificate-manager-to-ibm-cloud-secrets-manager){: external} blog.


### 30 September 2022: Manual provisioning is disabled
{: #migrate-end-provision}

{{site.data.keyword.cloudcerts_short}} will be removed from the catalog and you will be unable to provision new instances of the service. If you have an existing instance of the service, it will continue to operate as normal.

If you're working with the VPC Load Balancer integration, you must update your listener configuration prior to this date to ensure that your traffic flow is not stopped.


### 31 December 2022: End of support
{: #migrate-end-support}

Any remaining instances of {{site.data.keyword.cloudcerts_short}} will be automatically deleted.
If you have any user-provided Ingress secrets that are stored in {{site.data.keyword.cloudcerts_short}}, they will no longer be valid. 



## Comparison between {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}}
{: #migrate-differences}

Both {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} provide a secure repository for storing and managing certificates. All the features that are available for the {{site.data.keyword.cloudcerts_short}} service are supported by {{site.data.keyword.secrets-manager_short}}. The following table compares and contrasts some common characteristics between the services.

Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} is a paid service that requires a credit card-equipped account. [Review the {{site.data.keyword.secrets-manager_short}} pricing plans](/catalog/services/secrets-manager){: external} to understand how migrating to the service might impact your billing. For more information, contact your {{site.data.keyword.cloud_notm}} sales representative.
{: important}

| Characteristics | {{site.data.keyword.secrets-manager_short}} | {{site.data.keyword.cloudcerts_short}} |
| --- | --- | --- |
| Secrets and certificates management experience in the {{site.data.keyword.cloud_notm}} console | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Pricing plans | [Trial and Standard](/catalog/services/secrets-manager){: external} | [Free](/catalog/services/certificate-manager){: external} |
| Worldwide availability in multizone regions | ![Checkmark icon](../icons/checkmark-icon.svg)[^instance-limit] | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Data isolation through single-tenancy | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to integrate a key management service| ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources through private service endpoints | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources in an {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to import SSL/TLS certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to order public SSL/TLS certificates from Let's Encrypt | ![Checkmark icon](../icons/checkmark-icon.svg)[^dns-providers] | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to create private SSL/TLS certificates by using an internal certificate authority | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to manage secrets of various types | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Notifications |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Go, Python, Node.js, and Java SDKs | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| CLI plug-in | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Logging and monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |

{: caption="Table 1. Feature comparison between the {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} offerings" caption-side="top"}

[^instance-limit]: As of 23 March 2022, you can create an unlimited number of {{site.data.keyword.secrets-manager_short}} instances per account with the Standard pricing plan.

[^dns-providers]: The DNS providers that are supported by {{site.data.keyword.secrets-manager_short}} include {{site.data.keyword.cis_full_notm}} (CIS) and the Domain Name Registration service, which is available as part of {{site.data.keyword.cloud_notm}} classic infrastructure. [Learn more](#migrate-prepare).


## Migrating {{site.data.keyword.cloudcerts_short}} resources to {{site.data.keyword.secrets-manager_short}}
{: #migrate-process}

You can take advantage of the data isolation benefits of a single-tenant secrets management service by migrating your existing certificates in {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.

### {{site.data.keyword.secrets-manager_short}} limits and considerations
{: #migrate-prepare}

Before you begin, consider the following items and service limitations that might impact your experience as you integrate to {{site.data.keyword.secrets-manager_short}}.

- **{{site.data.keyword.secrets-manager_short}} is a paid service with Standard and Trial pricing plans.**

  You can create an instance of {{site.data.keyword.secrets-manager_short}} by choosing either the Standard and Trial pricing plans. To try out {{site.data.keyword.secrets-manager_short}} or create a Standard instance of the service, a credit card-equipped {{site.data.keyword.cloud_notm}} account is required. Review the [catalog entry for {{site.data.keyword.secrets-manager_short}}](/catalog/services/secrets-manager){: external} to learn more about pricing plans for the service.

- **Provisioning a {{site.data.keyword.secrets-manager_short}} instance takes 5 - 15 minutes to complete.**

  Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} is a single-tenant offering. During instance provisioning, {{site.data.keyword.secrets-manager_short}} creates various dedicated resources that are assigned to your service instance only. If you dynamically provision instances of {{site.data.keyword.cloudcerts_short}} and you plan to do the same with {{site.data.keyword.secrets-manager_short}} instances, keep in mind that {{site.data.keyword.secrets-manager_short}} provisioning is asynchronous and takes 5 - 15 minutes to complete.

- **Secret groups in {{site.data.keyword.secrets-manager_short}} are used to enforce granular access to secrets.**

  In {{site.data.keyword.cloudcerts_short}}, you can create access policies on individual certificates. In {{site.data.keyword.secrets-manager_short}}, you can set access policies on secret groups that contain one or more certificates. Additionally, {{site.data.keyword.secrets-manager_short}} supports a **SecretsReader** IAM role that provides read-only access to download certificates.

- **{{site.data.keyword.secrets-manager_short}} provides a unique endpoint URL for each service instance.**

  Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} constructs a unique endpoint URL for your service instance. {{site.data.keyword.secrets-manager_short}} endpoints uses the `appdomain.cloud` domain, whereas {{site.data.keyword.cloudcerts_short}} uses the `cloud.ibm.com` domain. For more information, review the {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} API docs.

- **{{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} APIs are different in structure.**

  If you use {{site.data.keyword.cloudcerts_short}} to manage your certificates programmatically, be sure to review the [{{site.data.keyword.secrets-manager_short}} API docs](/apidocs/secrets-manager) to understand how moving your certificates impacts your current experience.

- **Ordering Let's Encrypt certificates with {{site.data.keyword.secrets-manager_short}} requires an Automatic Certificate Management Environment (ACME) account.**

  Before you can order Let's Encrypt certificates through {{site.data.keyword.secrets-manager_short}}, you must configure the [public certificates secrets engine](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates) for your instance. This process involves granting access to Let's Encrypt by registering an Automatic Certificate Management Environment (ACME) account and providing your ACME account credentials. Be sure to review the documentation to understand how to enable your instance to order public certificates.

- **{{site.data.keyword.secrets-manager_short}} supports {{site.data.keyword.cis_full_notm}} (CIS) and classic infrastructure as integrated DNS providers, but also offers the ability to configure your own.**

  In {{site.data.keyword.cloudcerts_short}}, you might be working with either {{site.data.keyword.cis_full_notm}} (CIS) or a [third-party DNS provider](/docs/certificate-manager?topic=certificate-manager-ordering-certificates#other_provider) to order domain-validated certificates.

### Migration guidelines
{: #migrate-guidelines}

If you're ready to start your transition to {{site.data.keyword.secrets-manager_short}}, you can use automation tools to begin your migration. Start by setting up your {{site.data.keyword.secrets-manager_short}} service instance.

If you have a cluster that is integrated with {{site.data.keyword.cloudcerts_short}}, ensure that you read through the [migration steps in the {{site.data.keyword.containershort}} docs](/docs/containers?topic=containers-ingress-types#manage_certs) to ensure that you have full feature parity. For a step-by-step guide, see the [How to migrate certificates](https://www.ibm.com/cloud/blog/how-to-migrate-certificates-from-ibm-certificate-manager-to-ibm-cloud-secrets-manager){: external} blog.
{: note}

1. [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
2. Determine an access hierarchy for your certificates within {{site.data.keyword.secrets-manager_short}}.

   Create [secret groups](/docs/secrets-manager?topic=secrets-manager-secret-groups) in {{site.data.keyword.secrets-manager_short}} ahead of time so that you can organize your incoming certificates by mapped IAM policies. 

   Be sure to create secret groups first because you can't change assignments to certificates after you migrate them. If you accidentally assign an incoming certificate to the wrong secret group, or if you don't want a certificate to belong to the default secret group, you must delete the certificate and add it again.
   {: tip}

3. Optional. Configure the [public certificates secrets engine](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates).

   If you plan to use {{site.data.keyword.secrets-manager_short}} to order Let's Encrypt certificates, you can add certificate authority and DNS provider configurations to your {{site.data.keyword.secrets-manager_short}} service instance.

   To add a Let's Encrypt certificate authority configuration, an Automatic Certificate Management Environment (ACME) is required. For more information, see [Creating a Let's Encrypt ACME account](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#create-acme-account).
   {: note}

4. Migrate your certificates by using the [{{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}} migration scripts](https://github.com/ibm-cloud-security/certificate-manager-to-secrets-manager){: external}.


