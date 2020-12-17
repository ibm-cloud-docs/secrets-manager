---

copyright:
  years: 2020
lastupdated: "2020-12-11"

keywords: troubleshoot secrets manager, troubleshooting for secrets manager provisioning, provisioning stuck, provisioning stuck, troubleshooting Secrets Manager, unable to create instance, can't create instance

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}


# Why is my instance of {{site.data.keyword.secrets-manager_short}} stuck on provisioning?
{: #troubleshoot-provision}
{: troubleshoot}
{: support}

You try to create an instance of {{site.data.keyword.secrets-manager_full}}, but the provisioning doesn't complete.
{:shortdesc}


When you try to create an instance of the service in the {{site.data.keyword.cloud_notm}} console, you see the **Provisioning...** status in your resource list, but the status never transitions to an **Active** state.
{: tsSymptoms}
   
Because an instance of the service is created that is dedicated only to you, provisioning might take a few minutes to complete. Or, there might be an error in the provisioning process.
{: tsCauses}

To resolve the issue, try waiting 5 - 8 minutes and refreshing your web browser. If the problem persists, contact {{site.data.keyword.cloud_notm}} support.
{: tsResolve}

