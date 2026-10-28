---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: pricing plan, billing, cost

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
{{site.data.keyword.attribute-definition-list}}

# Pricing for {{site.data.keyword.secrets-manager_short}} on {{site.data.keyword.cloud_notm}}
{: #pricing}

Pricing in IBM Cloud Secrets Manager is based on the number of service instances in an account, and the number of active secrets in each of those instances. The `Trial` plan is free for 30 days. The `Standard` plan is billed monthly.
{: shortdesc}

For an overview of what each plan includes, see [Secrets Manager plans and features](/docs/secrets-manager?topic=secrets-manager-feature-overview).

## Trial and Standard plans
{: #plans}

The [Trial and Standard]{: tag-blue} plans run on shared multi-tenant infrastructure and are billed based on the number of active instances and secrets in your account.

### What you get
{: #trial-standard-what-you-get}

The `Trial` plan provides unlimited access to all service capabilities for a limited time of 30 days. The `Standard` plan provides an unlimited number of Secrets Manager instances per IBM Cloud account, and unlimited access to all service capabilities.
{: shortdesc}

A service instance is billed based on the day it was provisioned in the month.
{: note}

At the end of each billing period you're charged for the number of instances in your account, plus the maximum number of secrets with an active status in those instances. If you create more instances after the start of the monthly billing period, the cost of each additional instance for the first month is prorated based on the number of days that remain in the billing period.

### Trial plan limitations
{: #trial-plan-limitations}

When your Trial instance expires, you lose access to your secrets and integrations. To preserve your data, and prevent any disruptions in your workflow, you must upgrade to the Standard plan before your Trial plan expires. Follow these steps to [update your pricing plan](/docs/account?topic=account-changing). You can use the UI, API, and CLI to complete this process.

You can create only one Trial instance of Secrets Manager per account. Before you can create a new Trial instance, you must [delete the existing Trial instance and its reclamation](/docs/secrets-manager?topic=secrets-manager-mng-data#service-delete).
{: note}

## Vault Dedicated plan
{: #vault-dedicated-plan}

The [Vault Dedicated]{: tag-green} is a distinct offering that provides enterprise Vault capabilities for organizations that need stronger isolation, broader deployment flexibility, and advanced compliance support.

### How Vault Dedicated is billed
{: #vault-dedicated-billing}

The [Vault Dedicated]{: tag-green} plan uses two billing metrics.

| Metric | What it measures |
|--------|-----------------|
| **Enterprise Vault Installation** | A monthly charge per deployment instance and its entitlements. |
| **Resource Unit (RU)** | A consumption-based metric that measures monthly secrets usage across secrets, keys, certificates, and data protection operations. |
{: caption="Vault Dedicated billing metrics" caption-side="bottom"}

New instances are billed only for the days they were provisioned during the billing cycle.
{: note}

### Vault Dedicated pricing
{: #vault-dedicated-pricing}

| Metric | Monthly price |
|--------|--------------|
| **Enterprise Vault Installation** | $7,000.00 USD per instance |
| **Resource Unit (RU)** | $4.00 USD per resource unit |
{: caption="Vault Dedicated pricing" caption-side="bottom"}

### How resource units are calculated
{: #vault-dedicated-ru-calculation}

The number of resource units consumed depends on the type of secret or credential.

| Resource type | RU ratio | How it is counted |
|---------------|----------|-------------------|
| **Static secrets** | 1 secret = 1 RU | Based on the monthly high-water mark of unique secrets. |
| **Dynamic / auto-rotated secrets** | 1 dynamic secret role = 1 RU | Counted by the number of roles configured, regardless of how many versions are issued. |
| **PKI credentials** | Duration-adjusted (730 hrs = 1 RU) | Each certificate's active hours within the month are divided by 730. |
| **SSH credentials** | Duration-adjusted (730 hrs = 1 RU) | Each credential's active hours within the month are divided by 730. |
{: caption="Resource unit ratios by secret type" caption-side="bottom"}

For pricing details, see the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} or contact your IBM representative.
