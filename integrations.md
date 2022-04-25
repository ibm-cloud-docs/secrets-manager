---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-18"

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

# Integrations
{: #integrations}

With {{site.data.keyword.secrets-manager_full}}, you can save time with platform integrations that help you to dynamically create and retrieve secrets while you work with supported {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Start by creating an authorization between your {{site.data.keyword.secrets-manager_short}} instance and the {{site.data.keyword.cloud_notm}} service that supports an integration.

## Available integrations
{: #available-integrations}

The following table lists the services that can be authorized to work with {{site.data.keyword.secrets-manager_short}}.

| Service | Supports | Description |
| ------------------ | ----------- | ----------- |
| [Catalog management](/docs/account?topic=account-create-private-catalog) | Arbitrary secrets | Centrally manage the credentials for software in your private catalogs. For more information about this integration, check out [Onboarding software to your account](/docs/account?topic=account-create-private-catalog).
| [{{site.data.keyword.en_short}}](/docs/event-notifications) | Certificates | Be notified of events that take place in your {{site.data.keyword.secrets-manager_short}} service instance, and route alerts to your preferred destinations, such as email or SMS. For more information about this integration, check out [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).  |
| [{{site.data.keyword.containershort}}](/docs/containers) | Arbitrary secrets  \n Certificates  \n IAM credentials  \n Key-value secrets  \nUser credentials | Centrally manage Ingress subdomain certificates and other secrets for your Kubernetes clusters. For more information about this integration, check out [Managing TLS and Opaque certificates and secrets with {{site.data.keyword.secrets-manager_short}}](/docs/containers?topic=containers-ingress-types#manage_certs_secrets_mgr). |
| [{{site.data.keyword.openshiftshort}}](/docs/openshift) | Arbitrary secrets  \n Certificates  \n IAM credentials  \n Key-value secrets  \nUser credentials | Centrally manage Ingress subdomain certificates and other secrets for your {{site.data.keyword.openshiftshort}} clusters. For more information about this integration, check out [Managing TLS and Opaque certificates and secrets with {{site.data.keyword.secrets-manager_short}}](/docs/openshift?topic=openshift-ingress-roks4#manage_certs_secrets_mgr). |
| [Toolchain](/docs/ContinuousDelivery?topic=ContinuousDelivery-secretsmanager) | Arbitrary secrets | Centrally manage the credentials for your {{site.data.keyword.contdelivery_short}} toolchain. For more information about this integration, check out [Configuring {{site.data.keyword.secrets-manager_short}}](/docs/ContinuousDelivery?topic=ContinuousDelivery-secretsmanager).  |
{: caption="Table 1. Available integrations" caption-side="top"}

## Creating an authorization between {{site.data.keyword.secrets-manager_short}} and another {{site.data.keyword.cloud_notm}} service
{: #create-authorization}

To authorize another service to access your {{site.data.keyword.secrets-manager_short}} instance, you can [create an authorization between the services](/docs/account?topic=account-serviceauth). Be sure that you have the [**SecretsReader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam) on your {{site.data.keyword.secrets-manager_short}} instance.

1. In the console, click **Manage > Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source and target service for the authorization.

    1. From the **Source service** list, select the service that you want to integrate with {{site.data.keyword.secrets-manager_short}}.
    2. From the **Target service** list, select {{site.data.keyword.secrets-manager_short}}.
4. Select the **SecretsReader** role.

    With SecretsReader permissions, the source service can browse and retrieve the secrets that are available in your {{site.data.keyword.secrets-manager_short}} instance. The source service can't create secrets on your behalf.
5. Click **Authorize**.



