---

copyright:
  years: 2021
lastupdated: "2021-01-20"

keywords: faqs, Frequently Asked Questions, question, Secrets Manager, dynamic what is a secret, what is an arbitrary secret, what is an IAM credential, arbitrary secret, IAM credential, what happens when secret expires 

subcollection: secrets-manager

content-type: faq

---

{:faq: data-hd-content-type='faq'}
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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# FAQs for {{site.data.keyword.secrets-manager_short}}
{: #faqs}


FAQs for {{site.data.keyword.secrets-manager_full}} might include questions about secrets or credentials. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}


## What is a secret?
{: #faq-secret}
{: faq}
{: support}

A secret is a piece of sensitive information. For example, a secret might be a username and password combination or an API key that you use while you develop your applications. To keep your applications secure, it is important to regulate which secrets can access what and who has access to them. 

In addition to the static secrets described, there are other types of secrets that you might work with in the {{site.data.keyword.secrets-manager_short}} service. To learn more about secret types, check out [Types of secrets](/docs/secrets-manager?topic=secrets-manager-secret-basics#secret-types).

## What is a secrets engine?
{: #faq-secret-engine}
{: faq}

A secrets engine is a component that enables a connection between a {{site.data.keyword.secrets-manager_short}} instance and another service, such as IAM. The connection grants authorization between the services and allows the {{site.data.keyword.secrets-manager_short}} instance to create time-based secrets for the target service on demand.

For more information about secrets engines, see [Configuring secret types](/docs/secrets-manager?topic=secrets-manager-secret-engines).

## What is a secret group?
{: #faq-secret-group}
{: faq}

A secret group is a means to organize and control access to the secrets that you store within {{site.data.keyword.secrets-manager_short}}. There are several different strategies that you might use to approach secret groups. For more information and recommendations, see [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).


## What is an IAM credential?
{: #faq-credential}
{: faq}

An IAM credential is a type of dynamic secret that you can use to access an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication. When you create an IAM credential through {{site.data.keyword.secrets-manager_short}}, the service creates a service ID and an API key on your behalf. For more information about creating and storing IAM credentials, see [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-store-secrets#store-iam-credentials).


## What happens when I rotate my secret?
{: #faq-secret-rotate}
{: faq}
{: support}

When a secret is rotated, a new version of its value becomes available for use. You can choose to manually add a value or automatically generate one at regular intervals by enabling automatic rotation.  

For more information about secret rotation, see [Rotating secrets](/docs/secrets-manager?topic=secrets-manager-rotate-secrets).

## What happens when my secret expires?
{: #faq-secret-expire}
{: faq}
{: support}

If you are storing an arbitrary secret or user credentials, you can choose to set the date and time at which your secret expires. When the secret reaches its expiration date, it transitions to a *Destroyed* state. When that happens, the value that is associated with the secret is no longer recoverable.

For more information about how your information is protected, see [Securing your data](/docs/secrets-manager?topic=secrets-manager-mng-data).

## What are differences between the Reader and SecretsReader roles?
{: #faq-differences-secretsreader-roles}
{: faq}

Both the Reader and SecretsReader roles help you to assign read-only access to {{site.data.keyword.secrets-manager_short}} resources.

- As a reader, you can browse a high-level view of secrets in your instance. Readers can't access the payload of a secret.
- As a secrets reader, you can browse a high-level view of secrets, and you can access the payload of a secret. A secrets reader can't create secrets or modify the value of an existing secret.

## How is {{site.data.keyword.secrets-manager_short}} different from {{site.data.keyword.keymanagementserviceshort}}?
{: #faq-differences-key-protect}
{: faq}

There are a few key differences between using {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.secrets-manager_short}} to store your sensitive data. {{site.data.keyword.secrets-manager_short}} offers flexibility with the types of secrets that you can create and lease to applications and services on-demand. Whereas, {{site.data.keyword.keymanagementserviceshort}} delivers on encryption keys that are rooted in FIPS 140-2 Level 3 [hardware security modules (HSMs)](#x6704988){: term}.

For more information, see [Managing secrets in {{site.data.keyword.cloud_notm}}](/docs/secrets-manager?topic=secrets-manager-manage-secrets-ibm-cloud).

## How is {{site.data.keyword.secrets-manager_short}} different from Vault?
{: #faq-differences-vault}
{: faq}

With {{site.data.keyword.secrets-manager_short}}, you can centrally manage secrets for your services or apps in a dedicated, single tenant instance. To control who on your team has access to specific secrets, you can create secret groups that map to [Identity and Access Management (IAM)](/docs/secrets-manager?topic=secrets-manager-iam) access policies in your {{site.data.keyword.cloud_notm}} account. And, you can use [{{site.data.keyword.at_full_notm}}](/docs/secrets-manager?topic=secrets-manager-at-events) to track how users and applications interact with your {{site.data.keyword.secrets-manager_short}} instance. 

## Are community plug-ins for Vault supported by {{site.data.keyword.secrets-manager_short}}?
{: #faq-vault-community-plugins}
{: faq}

Currently, {{site.data.keyword.secrets-manager_short}} offers foundational capabilities that don't exist in upstream Vault but are required to support operations for {{site.data.keyword.secrets-manager_short}} as a managed service. These capabilities include a set of secrets engines to support secrets of various types in {{site.data.keyword.secrets-manager_short}}, and an {{site.data.keyword.cloud_notm}} Auth method that handles authentication between Vault and your {{site.data.keyword.cloud_notm}} account.

{{site.data.keyword.secrets-manager_short}} will continue to align with upstream Vault capabilities and plug-ins as it extends its support for more secrets engines in coming quarters. Keep in mind that plug-ins or components that are offered by the open source Vault community might not work with {{site.data.keyword.secrets-manager_short}}, unless they are written against a [secrets engine](/docs/secrets-manager?topic=secrets-manager-secret-engines) that {{site.data.keyword.secrets-manager_short}} currently supports.


## Can I manage {{site.data.keyword.cloud_notm}} secrets by using an on-premises Vault?
{: #faq-ibm-cloud-plugins-for-vault}
{: faq}

If you're looking to manage {{site.data.keyword.cloud_notm}} secrets through the full Vault native experience, you can use the stand-alone {{site.data.keyword.cloud_notm}} plug-ins for Vault. These open source plug-ins can be used independently from each other so that you can manage {{site.data.keyword.cloud_notm}} secrets through your on-premises Vault server.

- To set up authentication between Vault and your {{site.data.keyword.cloud_notm}} account, you can use the [{{site.data.keyword.cloud_notm}} Auth Method plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-auth-ibmcloud){:external}.
- To dynamically create API keys for {{site.data.keyword.cloud_notm}} service IDs, you can use the [{{site.data.keyword.cloud_notm}} Secrets Backend plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-secrets-ibmcloud){: external}.

## I'm not familiar with Vault. Can I still use {{site.data.keyword.secrets-manager_short}}?
{: #faq-vault-secrets-manager}
{: faq}

Yes. To use {{site.data.keyword.secrets-manager_short}}, you don't need to install Vault or the {{site.data.keyword.cloud_notm}} plug-ins for Vault. You can try {{site.data.keyword.secrets-manager_short}} for free, without needing an extensive background on how to use Vault. To get started, [choose the type of secret](/docs/secrets-manager?topic=secrets-manager-secret-basics) that you want to create. Then, you can integrate with the standard [Secrets Manager APIs](/apidocs/secrets-manager){:external} so that you can access the secret programmatically.