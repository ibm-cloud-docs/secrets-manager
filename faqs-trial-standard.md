---

copyright:
  years: 2020, 2026
lastupdated: "2026-10-30"

keywords: faqs, Frequently Asked Questions, Trial plan, Standard plan, secret group, IAM credentials, rotate secret, auto-rotation, secret expires, secret locks, upgrade, instance limit, pricing

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

# FAQ for the Trial and Standard plans
{: #faqs-trial-standard}

Frequently asked questions for the Trial and Standard plans of {{site.data.keyword.secrets-manager_full}}. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}

## What is a secret group?
{: #faq-secret-group}
{: faq}

A secret group is a means to organize and control access to the secrets that you store within {{site.data.keyword.secrets-manager_short}}. There are several different strategies that you might use to approach secret groups. For more information and recommendations, see [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).

## What is an IAM credential?
{: #faq-credential}
{: faq}

An IAM credential is a type of dynamic secret that you can use to access an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication. When you create an IAM credential through {{site.data.keyword.secrets-manager_short}}, the service creates a service ID and an API key on your behalf. For more information about creating and storing IAM credentials, see [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials).

## What happens when I rotate my secret?
{: #faq-secret-rotate}
{: faq}
{: support}

When a secret is rotated, a new version of its value becomes available for use. You can choose to manually add a value or automatically generate one at regular intervals by enabling automatic rotation.
For more information about secret rotation, see [Rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).

## Why is the auto-rotation interval for my secret not updating?
{: #faq-auto-rotation-interval-update}
{: faq}
{: support}

You might see the error `Error02009` when you update the auto-rotation interval if the current secret version expires before the next scheduled rotation. This error applies to secret types with TTL-based expiration.

To resolve this issue, rotate the secret to create a new version with a later expiration date, then retry the auto-rotation interval update.

For more information, see [Updating the auto-rotation interval](/docs/secrets-manager?topic=secrets-manager-automatic-rotation#update-auto-rotation-interval).

## What happens when my secret expires?
{: #faq-secret-expire}
{: faq}
{: support}

In some secret types such as `arbitrary` or `username_password`, you can set the date and time when your secret expires. When the secret reaches its expiration date, it transitions to a Destroyed state. When the transition happens, the value that is associated with the secret is no longer recoverable. The transition to the Destroyed state can take up to a couple of minutes after the secret expires, or a lock that prevented expiration is removed.

For more information about how your information is protected, see [Securing your data](/docs/secrets-manager?topic=secrets-manager-mng-data).

## How is {{site.data.keyword.secrets-manager_short}} different from Vault?
{: #faq-differences-vault}
{: faq}

With {{site.data.keyword.secrets-manager_short}}, you can centrally manage secrets for your services or apps in a dedicated, single-tenant instance. To control who on your team has access to specific secrets, you can create secret groups that map to [Identity and Access Management (IAM)](/docs/secrets-manager?topic=secrets-manager-iam) access policies in your {{site.data.keyword.cloud_notm}} account. And, you can use {{site.data.keyword.logs_full_notm}} to track how users and applications interact with your {{site.data.keyword.secrets-manager_short}} instance.

## I'm not familiar with Vault. Can I still use {{site.data.keyword.secrets-manager_short}}?
{: #faq-vault-secrets-manager}
{: faq}

Yes. To use {{site.data.keyword.secrets-manager_short}}, you don't need to install Vault or the {{site.data.keyword.cloud_notm}} plug-ins for Vault. You can try {{site.data.keyword.secrets-manager_short}} for free, without needing an extensive background on how to use Vault. To get started, [choose the type of secret](/docs/secrets-manager?topic=secrets-manager-what-is-secret) that you want to create. Then, you can integrate with the standard [Secrets Manager APIs](/apidocs/secrets-manager/secrets-manager-v2){: external} so that you can access the secret programmatically.

## What happens when my Trial plan expires?
{: #faq-trial-expires}
{: faq}

When your Trial instance expires after 30 days, you lose access to your secrets and integrations. The secrets stored in the instance are no longer retrievable. To preserve your data and prevent disruptions, upgrade to the Standard plan before the Trial plan expires.

For instructions, see [Updating your pricing plan](/docs/account?topic=account-changing).

## Can I upgrade from the Trial plan to the Standard plan?
{: #faq-trial-upgrade}
{: faq}

Yes. You can upgrade from the Trial plan to the Standard plan at any time before the Trial instance expires. After upgrading, your secrets and integrations are preserved and you continue using the same instance.

For instructions, see [Updating your pricing plan](/docs/account?topic=account-changing).

## How many instances can I create?
{: #faq-instance-limit}
{: faq}

You can create only one Trial instance per {{site.data.keyword.cloud_notm}} account. Before you can create a new Trial instance, you must [delete the existing Trial instance and its reclamation](/docs/secrets-manager?topic=secrets-manager-mng-data#service-delete).

The Standard plan allows an unlimited number of {{site.data.keyword.secrets-manager_short}} instances per account.

## What is a secret lock and when should I use it?
{: #faq-secret-lock}
{: faq}

A secret lock prevents a secret or one of its versions from being deleted or modified. Locks are useful when a secret is actively in use by an application and you want to prevent accidental deletion or rotation until the application releases it.

When all locks on a secret are removed, the secret can be deleted or modified normally. For more information, see [Locking secrets](/docs/secrets-manager?topic=secrets-manager-secret-locks).

## How am I billed for the Standard plan?
{: #faq-standard-billing}
{: faq}

The Standard plan is billed monthly based on the number of active service instances in your account and the maximum number of secrets with an active status across those instances during the billing period. If you create additional instances after the start of a billing period, the cost for each new instance is prorated based on the number of days remaining in that period.

For full pricing details, see [Pricing](/docs/secrets-manager?topic=secrets-manager-pricing#plans).

For more information on Secrets Manager FAQs, see [FAQ for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-faqs).

For more information on Vault Dedicated FAQs, see [FAQ for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-faqs-vault-dedicated).
