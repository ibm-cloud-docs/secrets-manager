---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-05"

keywords: secret engines, IAM secrets manager connection, Identity and access management, vault engine, dynamic secrets

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

# Configuring secret engines
{: #secret-engines}

After you create your {{site.data.keyword.secrets-manager_full}} service instance and assign access, you can enable secret engines so that you can create secrets of various types.
{: shortdesc} 

Secret engines are components in {{site.data.keyword.secrets-manager_short}} that are used to process operations for secrets of different types. These engines serve as backends for those secrets. Depending on its type, a secret engine is either enabled or disabled by default for your service instance. 

The following table lists the secret engines that are enabled for your instance by default.

| Engine type  | Description |
| ---- | ---- |
| [User credentials](/docs/secrets-manager?topic=secrets-manager-secret-basics#user-credentials) | Stores a username and password that you can use to log in or access an application or resource. |
| [Arbitrary text](/docs/secrets-manager?topic=secrets-manager-secret-basics#arbitrary-text) | Stores a custom secret, such as any structured or unstructured data, that you can use to access an application or resource. |
{: caption="Table 1. Secret engines that are available by default" caption-side="top"}



## Enabling secret engines for your instance
{: #configure-engines}

Some secret engines require an extra step to enable them. By enabling a secret engine, you configure a mapping between your {{site.data.keyword.secrets-manager_short}} service instance and another service. This connection grants your instance the permissions that it needs to create secrets on-demand for the service that you are targeting.

The following table lists the secret engines that are disabled for your instance by default.

| Engine type | Description |
|:------------|:------------|
| [IAM credentials](/docs/secrets-manager?topic=secrets-manager-secret-basics#iam-credentials) | Creates a service ID and an API key that you can use to access an {{site.data.keyword.cloud_notm}} resource. |
{: caption="Table 2. Secret engines that are disabled by default" caption-side="top"}

### Enabling the IAM secret engine
{: #configure-iam-engine}

You can enable the IAM secret engine for your {{site.data.keyword.secrets-manager_short}} service instance to create dynamic service IDs and API keys.

Start by entering an [{{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-serviceidapikeys) that is associated with a service ID in your {{site.data.keyword.cloud_notm}} account.

To allow your {{site.data.keyword.cloud_notm}} API key to create and manage other API keys dynamically, its associated service ID must have _Editor_ platform access for the Access Groups Service, and _Operator_ platform access for the IAM Identity Service.
{: note}

1. [Create a service ID](/docs/account?topic=account-serviceidapikeys).
2. Manage access for the service ID.

   1. From the **Actions** menu ![Actions icon](../../icons/actions-icon-vertical.svg), click **Manage service ID**.
   2. Click **Assign access**.
   3. Select the **Account management** tile.
   4. For the **IAM Access Groups Service**, add Editor platform access.
   5. For the **IAM Identity Service**, add Operator platform access
   6. Review your selections, and click **Assign**.
3. Create an API key for the service ID.
   
   1. Go to the **API keys** tab, and click **Create**. 
   2. Add a name and description to easily identify the API key.
   3. Click **Create**. 
   4. Click **Copy**.
4. Use the API key to configure the IAM secret engine.

   1. In a new browser tab, go to your instance of {{site.data.keyword.secrets-manager_short}}.
   2. Go to the **Settings** page.
   3. In the IAM secret engine section, enter the {{site.data.keyword.cloud_notm}} API key that you created in a previous step.
   4. Click **Save** to complete the configuration and enable the engine.

Now you can [create an IAM credential](/docs/secrets-manager?topic=secrets-manager-store-secrets#iam-credentials-ui) to work with dynamic service IDs and API keys. When the IAM credential reaches the end of its lease, the IAM secret engine deletes it automatically.

</staging>
