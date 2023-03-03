---

copyright:
  years: 2020, 2023
lastupdated: "2023-03-01"

keywords: Secrets Manager integrations, enable integration, create authorization, service to service, grant access between services, using Secrets Manager with other services, authorize Secrets Manager

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
{:terraform: .ph data-hd-interface="terraform"}
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

# Integrations for {{site.data.keyword.secrets-manager_short}}
{: #integrations}

With {{site.data.keyword.secrets-manager_full}}, you can save time with platform integrations that help you to dynamically create and retrieve secrets while you work with supported {{site.data.keyword.cloud_notm}} services.
{: shortdesc}



## Available integrations
{: #available-integrations}

The following table lists the services that can be authorized to work with {{site.data.keyword.secrets-manager_short}}.

| Service | Supports | Description |
| ------------------ | ----------- | ----------- |
| [{{site.data.keyword.alb_full}}](/docs/vpc?topic=vpc-load-balancers)  | Certificates | Centrally manage the SSL/TLS certificates that are required for load balancers to perform SSL offloading tasks. Create an authorization between **VPC Infrastructure Services** and {{site.data.keyword.secrets-manager_short}} to give a load balancer access to your certificates. [Learn more about this integration](/docs/vpc?topic=vpc-load-balancers#ssl-offloading-and-required-authorizations). |
| [App Configuration](/docs/app-configuration) | All secret types | A property value can be imported from {{site.data.keyword.secrets-manager_short}} into the App Configuration service. [Learn more](/docs/app-configuration?topic=app-configuration-ac-properties#property-type-secret-reference). |
| [API Connect](/docs/apiconnect?topic=apiconnect-getting-started-v5) | Certificates | Store your custom domain certificates in {{site.data.keyword.secrets-manager_short}}, then use certificate CRNs to bind with custom domains in API Gateway. |    
| [Catalog management](/docs/account?topic=account-create-private-catalog) | Arbitrary secrets | Centrally manage the credentials for software in your private catalogs. [Learn more about this integration](/docs/account?topic=account-create-private-catalog). |
| [Continuous Delivery](/docs/ContinuousDelivery?topic=ContinuousDelivery-secretsmanager) | Arbitrary secrets  \n IAM credentials | Centrally manage the credentials for your {{site.data.keyword.contdelivery_short}} toolchain. Create an authorization between **Toolchain** and {{site.data.keyword.secrets-manager_short}} to give a toolchain access to your secrets. [Learn more about this integration](/docs/ContinuousDelivery?topic=ContinuousDelivery-secretsmanager).  |
| [{{site.data.keyword.en_short}}](/docs/event-notifications) | Arbitrary secrets  \n Certificates  \n IAM credentials  \nUser credentials| Send notifications of events in {{site.data.keyword.secrets-manager_short}} to other users, or human destinations, by using email, SMS, or other supported delivery channels. [Learn more about this integration](/docs/secrets-manager?topic=secrets-manager-event-notifications). |
| [{{site.data.keyword.containershort}}](/docs/containers) | Arbitrary secrets  \n Certificates  \n IAM credentials  \n Key-value secrets  \nUser credentials | Centrally manage Ingress subdomain certificates and other secrets for your Kubernetes clusters. [Learn more about this integration](/docs/containers?topic=containers-ingress-types#manage_certs_secrets_mgr). |
| [{{site.data.keyword.openshiftshort}}](/docs/openshift) | Arbitrary secrets  \n Certificates  \n IAM credentials  \n Key-value secrets  \nUser credentials | Centrally manage Ingress subdomain certificates and other secrets for your {{site.data.keyword.openshiftshort}} clusters. [Learn more about this integration](/docs/openshift?topic=openshift-ingress-roks4#manage_certs). |
{: caption="Table 1. Available integrations" caption-side="top"}


## Authorizing an {{site.data.keyword.cloud_notm}} service to access {{site.data.keyword.secrets-manager_short}}
{: #create-authorization}

To authorize a supported {{site.data.keyword.cloud_notm}} service to access your {{site.data.keyword.secrets-manager_short}} instance, you can [create an authorization between the services](/docs/account?topic=account-serviceauth). Be sure that you have the [**SecretsReader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam) on your {{site.data.keyword.secrets-manager_short}} instance.

1. In the console, click **Manage > Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source account for the authorization. 
4. From the **Source service** list, select the service that you want to integrate with {{site.data.keyword.secrets-manager_short}}.
5. Specify whether you want the authorization for the source service to apply to all the instances that are associated with the account, only a specific instance, or instances that are only in a specific resource group.
2. From the **Target service** list, select {{site.data.keyword.secrets-manager_short}}.
5. Specify whether you want the authorization for the target service to apply to all the instances that are associated with the account, only a specific instance, or instances that are only in a specific resource group.
6. Select the required service access role.
    
    Some integrations might require a specific role. To understand which service role is needed, see the documentation for the service that you want to integrate with {{site.data.keyword.secrets-manager_short}}.
    {: note}
    
7. Click **Authorize**.
