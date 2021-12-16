---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-14"

keywords: logging, activity, monitor app, monitor secrets

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

# Logging for {{site.data.keyword.secrets-manager_short}}
{: #service-logs}

Use the {{site.data.keyword.la_full}} service to view {{site.data.keyword.secrets-manager_full}} logs for your instance.
{: shortdesc}

With {{site.data.keyword.la_full_notm}}, administrators, DevOps teams, and developers can review log data, define alerts, and design custom views to monitor application and system logs. For more information, see the [{{site.data.keyword.la_short}} docs](/docs/log-analysis?topic=log-analysis-getting-started).

## Before you begin
{: #before-logs}

If you're working with {{site.data.keyword.la_short}} for the first time, be sure that you create an instance in the same location as your {{site.data.keyword.secrets-manager_short}} instance. For more information, see [Configuring platform logs through the Observability dashboard](/docs/log-analysis?topic=log-analysis-config_svc_logs#config_svc_logs_ui).


## Viewing logs
{: #view-logs-ui}

Logs that are generated by a {{site.data.keyword.secrets-manager_short}} service instance are forwarded automatically to the {{site.data.keyword.la_short}} instance that is available in the same location.

To view {{site.data.keyword.secrets-manager_short}} logs, complete the following steps:

1. Log in to the {{site.data.keyword.cloud_notm}} console.
2. Go to **IBM Cloud > Observability** to access your [Observability dashboard](https://{DomainName}/observe){: external}.
3. From the navigation menu, click **Logging**.
4. Select a {{site.data.keyword.la_short}} instance, and click **Open Dashboard**.

    Don't have a {{site.data.keyword.la_short}} instance? [Create an instance](/docs/log-analysis?topic=log-analysis-provision) in the same location as your {{site.data.keyword.secrets-manager_short}} instance. Then, click **Configure platform logs** to receive logs from supported services in your account.
5. In the logging UI, filter by `secrets-manager` to view logs that are generated by {{site.data.keyword.secrets-manager_short}}.

    For more information about searching and filtering logs, check out the [{{site.data.keyword.la_short}} documentation](/docs/log-analysis?topic=log-analysis-monitor_logs).



