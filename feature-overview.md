---

copyright:
  years: 2026
lastupdated: "2026-08-28"

keywords: feature overview, trial plan, standard plan, vault dedicated plan

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

# Secrets Manager plans and features
{: #feature-overview}

{{site.data.keyword.secrets-manager_full}} offers three distinct plans to meet different organizational needs: `Trial`, `Standard`, and `Vault Dedicated`. Each plan provides secure secrets management capabilities with varying features and operational characteristics.
{: shortdesc}

## Trial and Standard plans
{: #trial-standard-plans}

The [Trial and Standard]{: tag-blue} plans provide the same comprehensive secrets management capabilities. The `Trial` plan offers a time-limited way to explore {{site.data.keyword.secrets-manager_short}} at no cost, while the `Standard` plan provides production-ready secrets management for applications and services.

Trial and Standard plans offer the same feature set. The only difference is that Trial plans are limited to 30 days and you can have only one Trial instance per account.
{: note}

### What you get
{: #trial-standard-capabilities}

With the [Trial and Standard]{: tag-blue} plans, you get:

- A single-tenant, dedicated instance for each service instance.
- Support for all secret types (arbitrary, user credentials, service credentials, IAM credentials, key-value, imported certificates, public certificates, and private certificates).
- Automatic secret rotation capabilities.
- Secret versioning and lifecycle management.
- Secret groups for organizing and controlling access.
- Secret locks to prevent accidental deletion or modification.
- Private and public endpoints for secure access.
- Data encryption at rest with either service-managed encryption or customer-managed keys.
- Integration with {{site.data.keyword.cloud_notm}} services and third-party tools.
- Activity tracking through {{site.data.keyword.logs_full_notm}}.
- Event notifications for secret lifecycle events.

### Trial plan limitations
{: #trial-limitations}

The `Trial` plan has the following limitations:

- 30-day time limit.
- Only 1 Trial instance per account.
- Can only upgrade to Standard (not to Vault Dedicated).

For more information, see [Upgrading to the Standard plan](/docs/secrets-manager?topic=secrets-manager-create-instance#upgrade-instance-standard).

For more information about pricing, see [Pricing](/docs/secrets-manager?topic=secrets-manager-pricing#standard-plan).

## Vault Dedicated plan
{: #vault-dedicated-plan-overview}

The [Vault Dedicated]{: tag-green} plan is a distinct offering that delivers Vault Enterprise as a managed service in {{site.data.keyword.cloud_notm}}. It is designed for workloads that need dedicated Vault capabilities together with managed operations in {{site.data.keyword.cloud_notm}}.

### What you get
{: #vault-dedicated-capabilities}

With the [Vault Dedicated]{: tag-green} plan, you get:

- A managed Vault Enterprise cluster that is configured for high availability across three nodes in a multi-zone region.
- Namespace-based isolation for multi-team environments.
- Private and public endpoints for secure access to your instance.
- Data encryption at rest with either service-managed encryption or customer-managed keys.
- The native Vault UI for management and configuration tasks.
- Advanced compliance and security features:
   - FIPS 140-3 compliant cryptography using the Vault Enterprise FIPS build for supported Vault cryptographic operations.
   - Advanced policy enforcement capabilities:
     - Vault ACL policies with fine-grained path and operation-level authorization
     - Sentinel policy enforcement for code-based policy guardrails and compliance automation
     - Control Groups for human approval workflows on high-value operations
   - Transit Encryption Engine for encryption-as-a-service: encrypt, decrypt, sign, verify, and derive keys without key material leaving Vault.
- Enterprise-scale Vault capabilities for complex deployments.
- Dedicated activity tracking events that are routed through {{site.data.keyword.logs_full_notm}} for auditing and monitoring.

For more information about auditing events for the service, see [Instance operations events](/docs/secrets-manager?topic=secrets-manager-at_events#at-configuration-instance-operations).
For more information about pricing, see [Pricing](/docs/secrets-manager?topic=secrets-manager-pricing#vault-dedicated-plan).

**Key capabilities includes:**

- Namespace-based isolation for multi-team environments.
- Advanced compliance and security features.
- High availability and disaster recovery support.
- Enterprise-scale Vault capabilities for complex deployments.

### Vault Dedicated plan limitations
{: #vault-dedicated-limitations}

The [Vault Dedicated]{: tag-green} plan has the following limitations:

- Not compatible with the Trial or Standard plan. You cannot upgrade or migrate between plan types.
- Vault Dedicated instances cannot be provisioned in all regions. For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-regions).
- The native {{site.data.keyword.secrets-manager_short}} UI is not available. Management is performed through the native Vault UI, API, or CLI.
- Secret types supported by `Trial` and `Standard` plans (such as IAM credentials and service credentials) are not available.

The [Vault Dedicated]{: tag-green} is currently available as a public beta. Beta features are provided for evaluation and testing purposes and have limitations compared to generally available features.
{: beta}

### Vault Dedicated public beta limitations
{: #vault-dedicated-beta-limitations}

During the public beta period, the [Vault Dedicated]{: tag-green} plan has the following temporary restrictions:

- **Instance limit**: Only 1 Vault Dedicated instance per account during beta.
- **No upgrade path**: Cannot upgrade from beta to GA. All beta instances will be deleted before general availability.
- **Regional availability**: Available in 3 regions (Dallas, Frankfurt, Paris).
- **Free during beta**: No charges apply during the beta period.
- **Beta to GA migration**: Data migration from beta instances to GA instances is not supported.

These limitations are temporary and apply only during the public beta period. It will be removed or modified when the Vault Dedicated plan reaches general availability.
{: important}
