---

copyright:
  years: 2026
lastupdated: "2026-10-30"

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

The following table describes each billing metric.

| Metric | What it measures |
|--------|-----------------|
| **Enterprise Vault Installation** | A monthly charge per deployment instance and its entitlements. |
| **Resource Unit** | A consumption-based metric that measures monthly secrets usage across secrets, keys, certificates, and data protection operations. |
{: caption="Vault Dedicated billing metrics" caption-side="bottom"}

New instances are billed only for the days they were provisioned during the billing cycle.
{: note}

### Vault Dedicated pricing
{: #vault-dedicated-pricing}

The [Vault Dedicated]{: tag-green} plan is billed monthly based on the number of instances and resource units in your account.

The following table outlines the monthly price for each billing metric:

| Metric | Monthly price |
|--------|--------------|
| **Enterprise Vault Installation** | $7,000.00 USD per instance |
| **Resource Unit** | $4.00 USD per resource unit |
{: caption="Vault Dedicated pricing" caption-side="bottom"}

### What counts as a resource unit
{: #vault-dedicated-ru-types}

A resource unit measures your monthly secrets usage.

The following terms define what qualifies as each type of resource unit in the [Vault Dedicated]{: tag-green} plan:

| Term | Definition |
|------|------------|
| **Secret** | A Static Secret or Dynamic Secret Role managed by a [Vault Dedicated]{: tag-green} instance. Excludes Cryptographic Keys, secrets and roles that directly support Data Protection Operations and credential issuance, and any secrets or roles managed by the KMIP secrets engine. |
| **Dynamic Secret Role** | A named role in an applicable secrets engine that generates credentials on demand according to its configuration. Applicable engines include AWS, Azure, Google Cloud, databases, Terraform, Kubernetes, LDAP, and Local Accounts. |
| **Cryptographic Key** | A cryptographic key managed by the KMSE (Key Management Secrets Engine) or TOTP (Time-based One-Time Password) secrets engines. |
| **Credential Unit** | A measure of the validity period of a unique, cryptographically secured digital attestation of identity, such as x.509 certificates, JSON Web Tokens (JWTs), SPIFFE Verifiable Identity Documents (SVIDs), SSH certificates, and SSH one-time passwords. |
| **Data Protection Operation** | An API call that uses the Transit, Transform, or Google Cloud KMS secrets engines. |
{: caption="Resource unit terms and definitions" caption-side="bottom"}

For pricing details, see the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} or contact your IBM representative.
