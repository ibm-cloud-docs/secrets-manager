---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-19"

keywords: secrets management best practice, secrets best practices, coding, developers

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

# Best practices for using {{site.data.keyword.secrets-manager_short}}
{: #best-practices-using}

Review the following suggested guidelines for implementing best practices around your secrets management with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

- {{site.data.keyword.secrets-manager_full}} is a regional service. Provision {{site.data.keyword.secrets-manager_short}} instances per region to spread your workloads and limit the impact of a regional outage.
- {{site.data.keyword.secrets-manager_short}} is a single-tenant service. CPU and memory limits are applied per {{site.data.keyword.secrets-manager_short}} instance. Those limits restrict the API request rates based on the usage pattern. As a rule of thumb, it is recommended to keep the rate below 20 req/s. Additionally, limit the number of unique clients that make requests to a single {{site.data.keyword.secrets-manager_short}} instance.
- Use {{site.data.keyword.secrets-manager_short}} as a cold storage. Apply caching and throttling to regulate the rate of requests to a {{site.data.keyword.secrets-manager_short}} instance.
- In case requests fail with timeouts or 429 or 503 HTTP status codes, apply exponential backoff retries within the described rate limits.
