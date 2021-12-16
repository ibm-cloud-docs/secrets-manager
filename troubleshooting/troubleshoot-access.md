---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-15"

keywords: unable to create IAM credentials, can't create IAM credentials, engine configuration required

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


# Why can't I create an IAM credential?
{: #troubleshoot-access}
{: troubleshoot}
{: support}

You try to use {{site.data.keyword.secrets-manager_full}} to create an IAM credential, but the option is disabled.
{: shortdesc}

In the {{site.data.keyword.secrets-manager_short}} UI, you click **Add** from the Secrets table to see a list of options. The option to create an IAM credential is disabled, and you see a message that shows `Engine configuration required`.
{: tsSymptoms}

To create an IAM credential, you must first enable the IAM credentials engine for your service instance. The engine creates a connection between {{site.data.keyword.secrets-manager_short}} and IAM that grants your service instance the permissions that are required to create dynamic service IDs and API keys on your behalf.
{: tsCauses}

First, [create an API key for a service ID](/docs/account?topic=account-serviceidapikeys) in your {{site.data.keyword.cloud_notm}} account. Be sure to assign it the [required level of access](/docs/secrets-manager?topic=secrets-manager-configure-iam-engine#before-configure-iam-engine) to be able to manage other API keys on your behalf. Then, go to the **Settings** page in the {{site.data.keyword.secrets-manager_short}} UI and enter the API key to complete the configuration.
{: tsResolve}


