---

copyright:
  years: 2026
lastupdated: "2026-11-05"

keywords: faqs, Frequently Asked Questions, Vault Dedicated, admin token, secrets engine, destination, billing, resource unit, authentication method, namespace

subcollection: secrets-manager

content-type: faq

---

{:faq: data-hd-content-type='faq'}
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

# FAQ for the Vault Dedicated plan
{: #faqs-vault-dedicated}

Frequently asked questions for the Vault Dedicated plan. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}

## How is the Vault Dedicated plan different from the Trial and Standard plans?
{: #faq-vd-difference}
{: faq}

The Vault Dedicatedplan provides a single-tenant HashiCorp Vault Enterprise cluster that you fully administer through the native Vault API, CLI, and UI. It is designed for organizations that need stronger workload isolation, advanced compliance support, and access to the full breadth of HashiCorp Vault Enterprise capabilities.

The [Trial and Standard]{: tag-blue} plans are managed multi-tenant offerings where {{site.data.keyword.cloud_notm}} abstracts secrets management into a simplified API. The Vault Dedicated plan requires you to configure and manage your own Vault namespaces, secrets engines, authentication methods, and policies.

For a full feature comparison, see [Secrets Manager plans and features](/docs/secrets-manager?topic=secrets-manager-feature-overview).

## What is an admin token and when should I use it?
{: #faq-vd-admin-token}
{: faq}

An admin token grants administrative access to your Vault Dedicated cluster's admin namespace. It is required for initial setup tasks such as enabling secrets engines, configuring authentication methods, and defining Vault policies.

Admin tokens are valid for 1 hour and cannot be renewed. You should use them only for administrative tasks and revoke them immediately after use. For day-to-day application access, configure a dedicated authentication method such as AppRole or Userpass instead.

For more information, see [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens).

## What authentication methods are supported?
{: #faq-vd-auth-methods}
{: faq}

Vault Dedicated plan supports the following authentication methods:

- **Token** – For initial setup and generating scoped child tokens.
- **AppRole** – For machine-to-machine authentication in applications and services.
- **Userpass** – For username and password authentication for human users.

For configuration instructions, see [Configuring authentication methods](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-auth-methods).

## What secrets engines are supported?
{: #faq-vd-secrets-engines}
{: faq}

Vault Dedicated plan supports the following secrets engines: KV v2, Transit, PKI (internal CA), PKI (external CA), Transform, TOTP, and Databases.

Before you use the Databases secrets engine, you must configure a [destination](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-destinations) to establish a private network link to your target database instance.

For the full list and links to HashiCorp documentation, see [Working with secrets engines](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-secrets-engines).

## What is a destination and why do I need one?
{: #faq-vd-destination}
{: faq}

Vault Dedicated instances block all outbound connectivity by default. A destination resource creates a private network link between your Vault cluster and a supported external service, such as an {{site.data.keyword.cloud_notm}} Databases instance.

You must configure a destination before you can use the Databases secrets engine. Only explicitly approved destination types are supported — general-purpose outbound egress is not available.

For more information, see [Configuring outbound connectivity destinations](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-destinations).

## Do I have access to the root namespace?
{: #faq-vd-root-namespace}
{: faq}

No. The root namespace is reserved for service operations and is not accessible to customers. You administer your Vault cluster through the admin namespace. All customer workloads should be organized within namespaces you create under the admin namespace.

## Can I install custom plugins or audit devices?
{: #faq-vd-plugins}
{: faq}

No. Plugin installation, audit device configuration, and backup lifecycle are all service-managed. {{site.data.keyword.cloud_notm}} handles Vault software upgrades, cluster scaling, storage management, and recovery key handling on your behalf.

For a full list of service-managed capabilities, see [Known issues and limits for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits-vault-dedicated).

## How is Vault Dedicated billed?
{: #faq-vd-billing}
{: faq}

Vault Dedicated plan is billed monthly using two metrics:

- **Enterprise Vault Installation** – $7,000.00 USD per instance per month.
- **Resource Unit** – $4.00 USD per resource unit per month.

Resource units measure your monthly secrets usage across secrets, keys, certificates, and data protection operations. New instances are billed only for the days they were provisioned during the billing cycle.

For full pricing details and resource unit definitions, see [Pricing](/docs/secrets-manager?topic=secrets-manager-pricing#vault-dedicated-plan).

## What counts as a resource unit?
{: #faq-vd-resource-unit}
{: faq}

A resource unit measures monthly secrets usage. The following types of resources consume RUs:

- **Secrets** – Static secrets and Dynamic Secret Roles managed by your instance. Cryptographic Keys, secrets that support Data Protection Operations, and secrets managed by the KMIP engine are excluded.
- **Dynamic Secret Roles** – Named roles in secrets engines such as AWS, Azure, databases, and Kubernetes that generate credentials on demand.
- **Cryptographic Keys** – Keys managed by the KMSE or TOTP secrets engines.
- **Credential Units** – Certificates and other cryptographically secured identity attestations, such as x.509 certificates, JWTs, SVIDs, and SSH certificates.
- **Data Protection Operations** – API calls using the Transit, Transform, or Google Cloud KMS secrets engines.

For the complete definitions table, see [What counts as a resource unit](/docs/secrets-manager?topic=secrets-manager-pricing#vault-dedicated-ru-types).

## What happens if I delete my {{site.data.keyword.cloud_notm}} Databases instance without deleting the destination first?
{: #faq-vd-destination-delete}
{: faq}

If you delete the target {{site.data.keyword.cloud_notm}} Databases instance without first deleting the destination, the destination and its associated outbound network connection remain in place. The destination is not automatically removed. You must delete the destination explicitly to clean up the network artifacts.

## Can I configure cross-region failover for the Vault Dedicated plan?
{: #faq-vd-cross-region-failover}
{: faq}

No. The {{site.data.keyword.secrets-manager_short}} Vault Dedicated plan does not support cross-region failover or replication-based recovery. If more than one availability zone becomes unavailable, the region is considered to be in a disaster state. Design your workloads with this in mind.

Automated data replication and snapshots across instances are also not supported. To keep a copy of your data elsewhere, you must export and import your secrets and configuration by using HashiCorp Vault's native tools. For more information, see [Understanding data portability by using the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-data-portability-vault-dedicated) and [Known issues and limits for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits-vault-dedicated).

## Why is my destination stuck in the provisioning state?
{: #faq-vd-destination-provisioning}
{: faq}

Creating a destination is an asynchronous operation. A destination can remain in the `not_started` or `provisioning` state, or move to `failed`, if the managed network artifacts cannot be successfully created or validated.

1. Check the destination state by sending a `GET /v2/instances/{instance_id}/destinations/{destination_id}` request, and read the `message` field.
2. Confirm that the target {{site.data.keyword.cloud_notm}} Databases instance has private endpoints enabled and that the required service-to-service authorization exists.
3. If the destination is in the `failed` state, delete it, resolve the underlying issue, and create a new destination. A `failed` destination counts against your quota of 20 destinations per instance until you delete it.

For more information, see [Configuring outbound connectivity destinations](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-destinations#vault-dedicated-destinations-lifecycle-states).

## Where can I find the Vault API and CLI references for Vault Dedicated?
{: #faq-vd-api-reference}
{: faq}

Vault Dedicated uses the native HashiCorp Vault API and CLI, plus the {{site.data.keyword.cloud_notm}} Instance Management API for control-plane operations such as generating admin tokens and managing destinations.

- [Instance Management API reference](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis)
- [HashiCorp Vault API reference](https://developer.hashicorp.com/vault/api-docs){: external}
- [HashiCorp Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external}

For more information on Secrets Manager FAQs, see [FAQ for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-faqs).

For more information on Trial and Standard FAQs, see [FAQ for the Trial and Standard plans](/docs/secrets-manager?topic=secrets-manager-faqs-trial-standard).
