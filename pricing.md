---

copyright:
  years: 2026
lastupdated: "2026-08-31"

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

# Pricing for {{site.data.keyword.secrets-manager_short}} on {{site.data.keyword.cloud_notm}}
{: #pricing}

Pricing in IBM Cloud Secrets Manager is based on the number of service instances in an account, and the number of active secrets in each of those instances. The `Trial` plan is free for 30 days. The `Standard` plan is billed monthly. For an overview of what each plan includes, see [Secrets Manager plans and features](/docs/secrets-manager?topic=secrets-manager-feature-overview).
{: shortdesc}

For an overview of what each plan includes, see [Secrets Manager plans and features](/docs/secrets-manager?topic=secrets-manager-feature-overview).

## Trial and Standard plans
{: #plans}

### What you get
{: #trial-standard-what-you-get}

The `Trial` plan provides unlimited access to all service capabilities for a limited time of 30 days. The `Standard` plan provides an unlimited number of Secrets Manager instances per IBM Cloud account, and unlimited access to all service capabilities.
{: shortdesc}

A service instance is billed based on the day it was provisioned in the month.
{: note}

At the end of each billing period you're charged for the number of instances in your account, plus the maximum number of secrets with an active status in those instances. If you create more instances after the start of the monthly billing period, the cost of each additional instance for the first month is prorated based on the number of days that remain in the billing period.

### What counts as an active secret
{: #active-secret-definition}

A secret is counted toward your bill when it has an `active` status. Secrets that are destroyed or expired are not counted. [TODO: confirm whether secrets in a `pre-activation` or `deactivated` state are included in the count, and whether the count is taken at peak, at a point in time, or as a daily average.]

### Trial plan limitations
{: #trial-plan-limitations}

When your Trial instance expires, you lose access to your secrets and integrations. To preserve your data, and prevent any disruptions in your workflow, you must upgrade to the Standard plan before your Trial plan expires. Follow these steps to [update your pricing plan](/docs/account?topic=account-changing). You can use the UI, API, and CLI to complete this process.

You can create only one Trial instance of Secrets Manager per account. Before you can create a new Trial instance, you must [delete the existing Trial instance and its reclamation](/docs/secrets-manager?topic=secrets-manager-mng-data#service-delete).
{: note}

## Vault Dedicated plan
{: #vault-dedicated-plan}

The `Vault Dedicated` plan is a distinct offering that provides enterprise Vault capabilities for organizations that need stronger isolation, broader deployment flexibility, and advanced compliance support.

The Vault Dedicated plan is currently available as a public beta. Beta features are provided for evaluation and testing purposes and have limitations compared to generally available features.
{: beta}

### Vault Dedicated public beta limitations
{: #vault-dedicated-beta-limitations}

During the public beta period, the [Vault Dedicated]{: tag-green} plan has the following temporary restrictions:

- **Instance limit**: Only 1 [Vault Dedicated]{: tag-green} instance per account during beta.
- **No upgrade path**: Cannot upgrade from beta to GA. All beta instances will be deleted before general availability.
- **Regional availability**: Available in 2 regions (Dallas and Frankfurt).
- **Free during beta**: No charges apply during the beta period.
- **Beta to GA migration**: Data migration from beta instances to GA instances is not supported.

These limitations are temporary and apply only during the public beta period. It will be removed or modified when the [Vault Dedicated]{: tag-green} plan reaches general availability.
{: important}

### How Vault Dedicated is billed
{: #vault-dedicated-billing}

The [Vault Dedicated]{: tag-green} plan uses two billing metrics.

| Metric | What it measures |
|--------|-----------------|
| **Install (cluster)** | A monthly charge per running production install. |
| **Resource Unit (RU)** | A consumption-based metric covering the secrets and credentials issued or managed in your instance. |
{: caption="Vault Dedicated billing metrics" caption-side="bottom"}

#### Resource Unit pricing
{: #vault-dedicated-ru-pricing}

Resource units are priced on a tiered model. Higher monthly RU volumes qualify for lower per-unit rates.

| Resource units (monthly) | Annual unit price | Monthly unit price | Total annual | Total monthly |
|-------------------------:|------------------:|-------------------:|-------------:|--------------:|
| 1 | $48.00 | $4.00 | $48 | $4 |
| 500 | $33.60 | $2.80 | $16,800 | $1,400 |
| 1,000 | $31.20 | $2.60 | $31,200 | $2,600 |
| 2,500 | $28.80 | $2.40 | $72,000 | $6,000 |
| 5,000 | $26.40 | $2.20 | $132,000 | $11,000 |
| 10,000 | $24.00 | $2.00 | $240,000 | $20,000 |
| 25,000 | $21.60 | $1.80 | $540,000 | $45,000 |
| 50,000 | $19.20 | $1.60 | $960,000 | $80,000 |
| 75,000 | $16.80 | $1.40 | $1,260,000 | $105,000 |
| 100,000 | $14.40 | $1.20 | $1,440,000 | $120,000 |
{: caption="Vault Dedicated Resource Unit pricing tiers" caption-side="bottom"}

#### How Resource Units are calculated
{: #vault-dedicated-ru-calculation}

The number of Resource Units consumed depends on the type of secret or credential.

| Resource type | RU ratio | How it is counted |
|---------------|----------|-------------------|
| **Static secrets** | 1 secret = 1 RU | Based on the monthly high-water mark of unique secrets. |
| **Dynamic / auto-rotated secrets** | 1 dynamic secret role = 1 RU | Counted by the number of roles configured, regardless of how many versions are issued. |
| **PKI credentials** | Duration-adjusted (730 hrs = 1 RU) | Each certificate's active hours within the month are divided by 730. |
| **SSH credentials** | Duration-adjusted (730 hrs = 1 RU) | Each credential's active hours within the month are divided by 730. |
{: caption="Resource unit ratios by secret type" caption-side="bottom"}

#### Resource Unit examples
{: #vault-dedicated-ru-examples}

The following examples illustrate how RUs are calculated in common scenarios.

- **100 static secrets, 20 deleted during the month**: RU count = 100 (monthly high-water mark applies).
- **80 secrets in a production cluster and 20 in a non-production cluster**: RU count = 100 (all instances in the account are counted together).
- **100 secrets rotated daily**: RU count = 100 (rotation does not increase the RU count).
- **100 dynamic secrets**: RU count = 100 (counted by roles configured, not versions issued).
- **100 PKI certificates active for the entire month (730 hours)**: RU count = 100.
- **100 PKI certificates active within the month, renewed daily**: RU count = 100.
- **1 certificate active for 500 hours within a month**: RU count = 1 (500 hrs ÷ 730 hrs, rounded up).
- **1 certificate active for 500 hours and a second active for 230 hours**: RU count = 1 (500 + 230 = 730 hrs = 1 RU).

For pricing details, see the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} or contact your IBM representative.
