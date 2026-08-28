---

copyright:
  years: 2026
lastupdated: "2026-08-28"

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

Admin tokens provide root-level access to your Vault Dedicated cluster and are required for initial setup and administrative operations. You generate and revoke admin tokens through the {{site.data.keyword.secrets-manager_short}} Instance Management API.
{: shortdesc}

Admin tokens should be treated as highly sensitive credentials. Generate them only when needed for administrative tasks, and revoke them immediately after use.
{: important}

## How admin token generation works
{: #admin-token-how-it-works}

Each time you request an admin token, the service creates a non-renewable admin token with a time-to-live (TTL) of 1 hour. The token expires automatically after 1 hour and cannot be renewed.

The admin token gives you administrative access to the Vault data plane — secrets engines, authentication methods, policies, and namespaces within your instance — but does not allow infrastructure-level operations such as sealing or unsealing Vault, scaling the cluster, or modifying storage settings.

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

- **Generate an admin token** – Creates a new Vault admin token that you can use to authenticate to the Vault API and UI. See [Generate an admin token](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#generate-admin-token) in the Instance Management API reference.

- **Revoke all admin tokens** – Immediately invalidates all active admin tokens for your instance, requiring new tokens to be generated for future administrative access. See [Revoke all admin tokens](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#revoke-admin-tokens) in the Instance Management API reference.

For authentication requirements, base URL details, and full request and response specifications for both operations, see [Instance Management API reference](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis#managing-admin-tokens).
