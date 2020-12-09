---

copyright:
  years: 2020
lastupdated: "2020-12-09"

keywords: 

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
{:api: .ph data-hd-interface='api'} 
{:cli: .ph data-hd-interface='cli'} 
{:ui: .ph data-hd-interface='ui'}

# WIP - Integrations with HashiCorp Vault
{: #vault-integrations}

Learn about how {{site.data.keyword.secrets-manager_full}} integrates with open source HashiCorp Vault.
{: shortdesc}

## How is {{site.data.keyword.secrets-manager_short}} different from Vault?
{: #secrets-manager-vault-differences}

With {{site.data.keyword.secrets-manager_short}}, you can centrally manage secrets for your services or apps in a dedicated, single tenant instance. To control who on your team has access to specific secrets, you can create secret groups that map to [Identity and Access Management (IAM)](/docs/secrets-manager?topic=secrets-manager-iam) access policies in your {{site.data.keyword.cloud_notm}} account. And, you can use [{{site.data.keyword.at_full_notm}}](/docs/secrets-manager?topic=secrets-manager-at-events) to track how users and applications interact with your {{site.data.keyword.secrets-manager_short}} instance. Because {{site.data.keyword.secrets-manager_short}} is a managed cloud service, you can focus on defining your secrets strategy without the added cost and complexity of building and managing your own infrastructure. To use {{site.data.keyword.secrets-manager_short}}, you don't need to install Vault or the {{site.data.keyword.cloud_notm}} plug-ins for Vault.

## Which Vault components are supported by {{site.data.keyword.secrets-manager_short}}?
{: #supported-vault-components}

{{site.data.keyword.secrets-manager_short}} doesn't support all components that are available for Vault. Instead, the service builds on a custom version of open source Vault to support operations in {{site.data.keyword.secrets-manager_short}} for various secret types.

The following table lists the Vault components that are supported by {{site.data.keyword.secrets-manager_short}}.

| Component name | Description |
| --- | --- |
| TBU | TBU |
| TBU | TBU |
| TBU | TBU |
{: caption="Table 1. Vault components that are supported by {{site.data.keyword.secrets-manager_short}}" caption-side="top"}

## Can I manage {{site.data.keyword.cloud_notm}} secrets by using an on-premises Vault?
{: #vault-plugins}

If you're running your own on-premises Vault, you can still integrate with {{site.data.keyword.secrets-manager_short}} by using {{site.data.keyword.cloud_notm}} open source plug-ins for Vault. These plug-ins can be used independently from each other so that you can manage {{site.data.keyword.cloud_notm}} secrets through your on-premises Vault server.

- To set up authentication between Vault and your {{site.data.keyword.cloud_notm}} account, you can use the {{site.data.keyword.cloud_notm}} Auth Method plug-in for Vault. [View this project in GitHub](https://github.com/ibm-cloud-security/vault-plugin-auth-ibmcloud){:external}
- To dynamically create API keys for {{site.data.keyword.cloud_notm}} service IDs, you can use the {{site.data.keyword.cloud_notm}} Secrets Backend plug-in for Vault. [View this project in GitHub](https://github.com/ibm-cloud-security/vault-plugin-secrets-ibmcloud){: external}

## I'm not familiar with Vault. Can I still use {{site.data.keyword.secrets-manager_short}}?
{: #using-secrets-manager}

Yes! You can try {{site.data.keyword.secrets-manager_short}} for free, without needing an extensive background on how to use Vault. To get started, [choose the type of secret](/docs/secrets-manager?topic=secrets-manager-secret-basics) that you want to create. Then, you can integrate with the standard [Secrets Manager APIs](/apidocs/secrets-manager){:external} so that you can access the secret programmatically.

