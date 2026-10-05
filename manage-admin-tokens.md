---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: Secrets Manager, Vault Dedicated, admin tokens, instance management API

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

# Managing admin tokens
{: #manage-admin-tokens}

IBM Cloud Vault Dedicated clusters operate from the admin namespace, unlike a self-managed Vault Enterprise cluster which operates from the root namespace.T he root namespace is reserved for service operations and not customer accessible.
{: shortdesc}

Admin tokens provide administrative access to your Vault Dedicated cluster admin namespace and are required for initial setup and ongoing administrative operations performed through the CLI, API, or Terraform. When accessing Vault through the IBM Cloud console UI, admin tokens are handled automatically. For more information, see [Launching the Vault Web UI](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis&interface=ui#launch-vault-web-ui). You generate and revoke admin tokens through the Secrets Manager Instance Management API.

Treat admin tokens as highly sensitive credentials. Generate them only when needed for administrative tasks, and revoke them immediately after use.
{: important}

## How admin token generation works
{: #admin-token-how-it-works}

Each time you request an admin token, the service creates a non-renewable admin token with a time-to-live (TTL) of 1 hour. The token expires automatically after 1 hour and cannot be renewed.

The admin token provides administrative access to your Vault Dedicated cluster's admin namespace. It can be used to create namespaces, configure secrets engines and authentication methods, manage policies, and perform other administrative tasks.
The admin token does not provide access to service-managed operations such as sealing or unsealing Vault, cluster scaling, and storage management, those operations are performed exclusively by the IBM Cloud Vault Dedicated service.

## Accessing Vault through the {{site.data.keyword.cloud_notm}} console
{: #admin-token-ui-access}

When you click **Launch Vault Web UI** in the {{site.data.keyword.cloud_notm}} console, the service automatically generates an admin token using Vault response wrapping. The real admin token is never transmitted through the {{site.data.keyword.cloud_notm}} control plane or exposed to the browser in plain text. Instead, a short-lived wrapping token (valid for 30 seconds, single-use) is passed directly to the Vault Web UI, which exchanges it for the authenticated session automatically.

You do not need to generate, copy, or paste an admin token to use the Vault Web UI. For details about the browser-based login experience and how the wrapping token flow works, see [Launching the Vault Web UI](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis&interface=ui#launch-vault-web-ui).

## Accessing Vault through the CLI, API, or Terraform
{: #admin-token-cli-api-access}

To use the CLI, API, or Terraform to interact with your Vault instance, you must explicitly generate an admin token. Treat the admin token as a highly sensitive credential. Store it securely, use it only for the intended task, and revoke it immediately afterward.

For step-by-step instructions, see [Generating an admin token](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#managing-admin-tokens) for CLI, API, and Terraform.

## Recommended practices
{: #admin-token-best-practices}

The admin token is intended for initial configuration and emergency access only. For day-to-day operations, configure an authentication method (such as AppRole, Kubernetes, or JWT) within your Vault instance so that your teams and applications can generate tokens without depending on the admin token.

- Use the admin token to perform initial setup: enable secrets engines, configure authentication methods, and define access policies.
- Revoke the admin token after each administrative session.
- Avoid storing the admin token in scripts or automation. Use a dedicated authentication method instead.
- A rate limit applies to admin token generation to prevent abuse. If you exceed the limit, wait before requesting a new token.

## API operations
{: #admin-token-api-operations}

The Instance Management API exposes the following operations for managing admin tokens:

- **Generate an admin token** – Creates a new Vault admin token for use with the CLI, API, or Terraform. For full request and response details, see [Generating an admin token](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#managing-admin-tokens) for CLI, API, and Terraform.

- **Revoke all admin tokens** – Immediately invalidates all active admin tokens for your instance, requiring new tokens to be generated for future administrative access. See [Revoking all admin tokens](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis&interface=ui#revoke-admin-tokens-ui) in the Instance Management API reference.

For authentication requirements, base URL details, and full request and response specifications for both operations, see [Instance Management API reference](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#managing-admin-tokens).
