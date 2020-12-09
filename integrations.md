---

copyright:
  years: 2020
lastupdated: "2020-12-09"

keywords: Secrets Manager integrations, enable integration, service to service, grant access between services

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



# Integrations
{: #integrations}<staging>

# Integrations with other {{site.data.keyword.cloud_notm}} services
{: #cloud-integrations}<staging>

With {{site.data.keyword.secrets-manager_full}}, you can save time with platform integrations that help you to dynamically create and retrieve secrets while you work with supported {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Start by creating an authorization between your {{site.data.keyword.secrets-manager_short}} instance and the {{site.data.keyword.cloud_notm}} service that supports an integration. 

## Available integrations
{: #available-integrations}

The following table lists the services that can be authorized to work with {{site.data.keyword.secrets-manager_short}}.

| Service | Description |
| ------------------ | ----------- |
| [Catalog management](/docs/account?topic=account-create-private-catalog) | Centrally manage access to products in the {{site.data.keyword.cloud_notm}} catalog by creating and customizing private catalogs.    |
{: caption="Table 1. Available integrations" caption-side="top"}

## Creating an authorization between {{site.data.keyword.secrets-manager_short}} and another {{site.data.keyword.cloud_notm}} service
{: #create-authorization}

To authorize another service to use a secret that's stored your {{site.data.keyword.secrets-manager_short}} instance, you can [create an authorization between the services](/docs/account?topic=account-serviceauth). Be sure that you have the [**SecretsReader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam) on your {{site.data.keyword.secrets-manager_short}} instance.

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage > Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source and target service for the authorization.
   
   1. From the **Source service** list, select the service that you want to integrate with {{site.data.keyword.secrets-manager_short}}.
   2. From the **Target service** list, select {{site.data.keyword.secrets-manager_short}}.
4. Select the **SecretsReader** role.

    With SecretsReader permissions, the source service can browse and retrieve the secrets that are available in your {{site.data.keyword.secrets-manager_short}} instance. The source service can't create secrets on your behalf.
5. Click **Authorize**.

