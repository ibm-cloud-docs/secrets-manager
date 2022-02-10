---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-10"

keywords: migrate from Certificate Manager, migrate to Secrets Manager, migrate certificates

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

With {{site.data.keyword.secrets-manager_full}}, you can centralize your application secrets in a single service, including the SSL or TLS certificates that you might already store and manage in {{site.data.keyword.cloudcerts_long_notm}}. Learn about suggested guidelines for moving your certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.
{: shortdesc}


As of 10 February 2022, {{site.data.keyword.cloudcerts_long_notm}} is deprecated. The strategic alternative for managing certificates in {{site.data.keyword.cloud_notm}} is {{site.data.keyword.secrets-manager_short}}. For more information, see the [deprecation announcement](/docs/certificate-manager?topic=certificate-manager-release-notes#certificate-manager-feb1022).
{: note}


## Comparison between {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}}
{: #migrate-differences}

Both {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} provide a secure repository for storing and managing certificates. All of the features that are available for the {{site.data.keyword.cloudcerts_short}} service are supported by {{site.data.keyword.secrets-manager_short}}. The following table compares and contrasts some common characteristics between the services.


| Characteristics | {{site.data.keyword.secrets-manager_short}} | {{site.data.keyword.cloudcerts_short}} |
| --- | --- | --- |
| Secrets or certificates management experience in the {{site.data.keyword.cloud_notm}} console | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Worldwide availability in multizone regions | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Data isolation through single-tenancy | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to integrate a key management service| ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources through private service endpoints | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources in an {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to import SSL or TLS certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to order public SSL or TLS certificates from Let's Encrypt | ![Checkmark icon](../icons/checkmark-icon.svg)| ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage secrets of various types | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Notifications |  ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Go, Python, Node.js, and Java SDKs | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| CLI plug-in | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Logging and monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison between the {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} offerings" caption-side="top"}


## Migrating {{site.data.keyword.cloudcerts_short}} resources to {{site.data.keyword.secrets-manager_short}}
{: #migrate-process}

You can take advantage of the data isolation benefits of a single-tenant secrets management service by migrating your existing certificates in {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.

### {{site.data.keyword.secrets-manager_short}} limits and considerations
{: #migrate-prepare}

Before you begin, consider the following items and service limitations that might impact your experience as you integrate to {{site.data.keyword.secrets-manager_short}}.

- **{{site.data.keyword.secrets-manager_short}} instances are limited to 1 per account.**

  Currently, {{site.data.keyword.secrets-manager_short}} is a free service that enforces a limit of one instance per {{site.data.keyword.cloud_notm}} account. Pricing and service limits for {{site.data.keyword.secrets-manager_short}} are subject to change. During the transition period, you can create a {{site.data.keyword.secrets-manager_short}} service instance to start migrating your existing certificates from {{site.data.keyword.cloudcerts_short}}. {{site.data.keyword.secrets-manager_short}} does not enforce a limit on the total number of secrets or certificates you can store per instance.

- **Provisioning a {{site.data.keyword.secrets-manager_short}} instance takes 5 - 8 minutes to complete.** 

  Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} is a single-tenant offering. During instance provisioning, {{site.data.keyword.secrets-manager_short}} creates various dedicated resources that are assigned to your service instance only. If you dynamically provision instances of {{site.data.keyword.cloudcerts_short}} and you plan to do the same with {{site.data.keyword.secrets-manager_short}} instances, keep in mind that {{site.data.keyword.secrets-manager_short}} provisioning is asynchronous and takes 5 - 8 minutes to complete.

- **Secret groups in {{site.data.keyword.secrets-manager_short}} are used to enforce granular access to secrets.** 

  In {{site.data.keyword.cloudcerts_short}}, you can create access policies on individual certificates. In {{site.data.keyword.secrets-manager_short}}, you can set access policies on secret groups that contain one or more certificates. Additionally, {{site.data.keyword.secrets-manager_short}} supports a **SecretsReader** IAM role that provides read-only access to download certificates.

- **{{site.data.keyword.secrets-manager_short}} provides a unique endpoint URL for each service instance.**

  Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} constructs a unique endpoint URL for your service instance. {{site.data.keyword.secrets-manager_short}} endpoints uses the `appdomain.cloud` domain, whereas {{site.data.keyword.cloudcerts_short}} uses the `cloud.ibm.com` domain. For more information, review the {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} API docs.

- **{{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} APIs are different in structure.**

  If you use {{site.data.keyword.cloudcerts_short}} to manage your certificates programmatically, be sure to review the [{{site.data.keyword.secrets-manager_short}} API docs](/apidocs/secrets-manager) to understand how moving your certificates impacts your current experience.


### Migration guidelines
{: #migrate-guidelines}

If you're ready to start your transition to {{site.data.keyword.secrets-manager_short}}, you can use automation tools to begin your migration. Start by setting up your {{site.data.keyword.secrets-manager_short}} service instance.

1. [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
2. Determine an access hierarchy for your certificates within {{site.data.keyword.secrets-manager_short}}. 

    Create [secret groups](/docs/secrets-manager?topic=secrets-manager-secret-groups) in {{site.data.keyword.secrets-manager_short}} ahead of time so that you can organize your incoming certificates by mapped IAM policies. 

    Be sure to create secret groups first because you can't change assignments to certificates after you migrate them. If you accidentally assign an incoming certificate to the wrong secret group, or if you don't want a certificate to belong to the default secret group, you must delete the certificate and add it again.
    {: tip}

3. Migrate your certificates by using the [{{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}} migration scripts](https://github.com/ibm-cloud-security/certificate-manager-to-secrets-manager){: external}.

