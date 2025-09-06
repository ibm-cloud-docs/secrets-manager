---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-06"

keywords: can't create IAM credentials, can't regenerate IAM credentials, IAM credentials not working, IP address restrictions enabled, IP address not allowed

subcollection: secrets-manager

content-type: troubleshoot

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

# Why am I blocked from ordering a public certificate or generating IAM credentials?
{: #ip-address-not-allowed}
{: troubleshoot} 
{: support}

You try to use {{site.data.keyword.secrets-manager_full}} to order public certificates or generate IAM credentials, but the service is unable to complete the action.
{: shortdesc}

You're working in an {{site.data.keyword.cloud_notm}} account that has [IP address access restrictions](/docs/account?topic=account-ips). When you try to use a feature in {{site.data.keyword.secrets-manager_short}} that requires a user-provided {{site.data.keyword.cloud_notm}} API key, for example when you generate IAM credentials, you encounter an error similar to the following examples:
{: tsSymptoms}

```json
IAM credentials couldn't be regenerated because IP address restrictions are enabled for the account. Update the IP address settings in your account to include IP addresses for Secrets Manager and try again.
```
{: screen}

```json
Cloud Internet Services (CIS) couldn't be reached because IP address restrictions are enabled for the account. Update the IP address settings in your account to include IP addresses for Secrets Manager and try again.
```
{: screen}

These errors can occur when {{site.data.keyword.secrets-manager_short}} attempts to log in to the target account with the configured API key to complete the request. However, the service is unable to do so because the account allows access to specific IP addresses only. To allow the account to accept requests from {{site.data.keyword.secrets-manager_short}}, you must specify an allowlist of IP addresses, along with your own IP address.
{: tsCauses}

To resolve the issue, ensure that the IP address restriction settings in the account are updated to allow the IP addresses that correspond with the region in which your {{site.data.keyword.secrets-manager_short}} is located.
{: tsResolve}

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage > Access (IAM)**, and select **Settings**.
2. From the Account restrictions section, edit the **IP address access** setting.
3. In the Allowed IP addresses field, include the allowlist of IP addresses that you defined based on the locations where access requests originate. For more information, see [Managing access with context-based restrictions](/docs/secrets-manager?topic=secrets-manager-access-control-cbr).
