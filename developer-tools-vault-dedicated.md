---

copyright:
  years: 2026
lastupdated: "2026-08-31"

keywords: Secrets Manager developer tools, integrate with application, API, SDK, CLI, Vault Dedicated

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

# Integrating {{site.data.keyword.secrets-manager_short}} with your apps by using the Vault Dedicated plan
{: #integrate-with-apps-vault-dedicated}

Ready to integrate {{site.data.keyword.secrets-manager_full}} Vault Dedicated into your existing apps or services? Take advantage of our supported developer tools.
{: shortdesc}

## Supported developer tools
{: #dev-tool-list-vault-dedicated}

### {{site.data.keyword.secrets-manager_short}} instance management SDKs
{: #dev-tool-sdks-vault-dedicated}

{{site.data.keyword.secrets-manager_short}} offers instance management software development kits (SDKs) for programmatic access to control plane operations for your Vault Dedicated instance. These SDKs cover instance management operations only — such as generating and revoking admin tokens and retrieving instance details. For runtime secrets operations, use the native [Vault API](https://developer.hashicorp.com/vault/api-docs){: external} directly. For more information, check out the following repositories on GitHub:

- [Go management SDK](https://github.com/IBM/secrets-manager-management-go-sdk){: external}
- [Node.js management SDK](https://github.com/IBM/secrets-manager-node-sdk){: external}
- [Java management SDK](https://github.com/IBM/secrets-manager-management-java-sdk){: external}
- [Python management SDK](https://github.com/IBM/secrets-manager-management-python-sdk){: external}

### {{site.data.keyword.secrets-manager_short}} instance management CLI plug-in
{: #dev-tool-cli-vault-dedicated}

If you're already using the [{{site.data.keyword.cloud_notm}} Command Line Interface (CLI)](/docs/cli?topic=cli-getting-started), you can install the {{site.data.keyword.secrets-manager_short}} instance management plug-in to perform control plane operations on your Vault Dedicated instance from the command line, such as generating and revoking admin tokens and viewing instance details.

To install the {{site.data.keyword.secrets-manager_short}} instance management CLI plug-in, run `ibmcloud plugin install secrets-manager-instance-management`.
{: note}

For the full CLI reference, see [{{site.data.keyword.secrets-manager_short}} instance management CLI](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli). For a history of changes, see the [{{site.data.keyword.secrets-manager_short}} instance management CLI change log](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli-change-log).

### {{site.data.keyword.secrets-manager_short}} instance management API
{: #dev-tool-api-vault-dedicated}

The {{site.data.keyword.secrets-manager_short}} instance management API provides control plane operations for your Vault Dedicated instance. It covers a focused set of operations: retrieving instance details, generating an admin token, and revoking admin tokens. It is not a runtime secrets API — for secrets operations such as reading and writing secrets, use the native [Vault API](https://developer.hashicorp.com/vault/api-docs){: external} directly against your Vault cluster endpoint.

To call the instance management API, copy the control plane service endpoint URL from the **Endpoints** page in your {{site.data.keyword.secrets-manager_short}} service dashboard and generate an [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token](/docs/iam?topic=iam-iamtoken_from_apikey).

```sh
curl -X GET  \
  -H "Authorization: Bearer {access_token}" \
  -H "Accept: application/json"
  "{base_url}/api/v2/instance"
```
{: codeblock}

Replace `{base_url}` with your control plane service endpoint URL, and `{access_token}` with your IAM token.

### Vault Agent
{: #dev-tool-vault-agent}

Vault Agent is a client-side daemon that runs alongside your application and handles authentication to your Vault Dedicated instance automatically. It manages token renewal and can retrieve secrets on behalf of your application, removing the need to write Vault authentication logic into your code.

Common uses include:

- Automatically authenticating to Vault using a configured auth method (such as AppRole or Kubernetes).
- Writing secrets to a file or template that your application reads directly.
- Renewing tokens before they expire, so your application always has valid access.

For more information, see the [Vault Agent documentation](https://developer.hashicorp.com/vault/docs/agent-and-proxy/agent){: external}.

### Working with the Vault Dedicated plan
{: #dev-tool-vault-dedicated}

The Vault Dedicated plan provides a managed HashiCorp Vault Enterprise cluster. You can interact with your {{site.data.keyword.secrets-manager_short}} Vault Dedicated instance by using the native Vault HTTP API or CLI.

For more information, check out the following HashiCorp resources:

- [Vault API reference](https://developer.hashicorp.com/vault/api-docs){: external}
- [Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external}

## HashiCorp Vault resources
{: #vault-dedicated-hashicorp-resources}

Because Vault Dedicated uses the same binary as self-hosted Vault Enterprise, you can use the full HashiCorp documentation to configure and operate your instance. The following topics are most relevant for getting started.

### Authentication
{: #vault-dedicated-auth-resources}

| Topic | Description |
|-------|-------------|
| [Tokens](https://developer.hashicorp.com/vault/docs/concepts/tokens){: external} | Core concepts for Vault tokens, including token types, TTLs, and renewal. Essential background for working with admin tokens. |
| [Auth methods overview](https://developer.hashicorp.com/vault/docs/auth){: external} | Overview of all supported authentication methods. Configure an auth method after initial setup so your teams and apps do not need to rely on the admin token. |
| [Token auth method](https://developer.hashicorp.com/vault/docs/auth/token){: external} | Reference for the built-in token auth method. |
| [AppRole auth method](https://developer.hashicorp.com/vault/docs/auth/approle){: external} | The most common programmatic auth pattern for applications using Vault Dedicated. |
| [Kubernetes auth method](https://developer.hashicorp.com/vault/docs/auth/kubernetes){: external} | Authenticate workloads running in Kubernetes clusters using service account tokens. |
{: caption="HashiCorp Vault authentication resources" caption-side="bottom"}

### Secrets engines
{: #vault-dedicated-secrets-engine-resources}

| Topic | Description |
|-------|-------------|
| [KV secrets engine v2](https://developer.hashicorp.com/vault/docs/secrets/kv/kv-v2){: external} | Store and retrieve arbitrary static secrets. The core secrets engine for most Vault Dedicated users. |
| [PKI secrets engine](https://developer.hashicorp.com/vault/docs/secrets/pki){: external} | Issue and manage X.509 certificates, including support for ACME clients. |
| [Transit secrets engine](https://developer.hashicorp.com/vault/docs/secrets/transit){: external} | Encryption-as-a-service: encrypt, decrypt, sign, and verify data without exposing key material. |
| [Database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases){: external} | Generate dynamic, short-lived credentials for databases. |
| [SSH secrets engine](https://developer.hashicorp.com/vault/docs/secrets/ssh){: external} | Issue dynamic SSH credentials and signed certificates for secure host access. |
{: caption="HashiCorp Vault secrets engine resources" caption-side="bottom"}

### Policies and access control
{: #vault-dedicated-policy-resources}

| Topic | Description |
|-------|-------------|
| [Policies](https://developer.hashicorp.com/vault/docs/concepts/policies){: external} | Vault ACL policies with fine-grained path and operation-level authorization. |
| [Sentinel policy enforcement](https://developer.hashicorp.com/vault/docs/enterprise/sentinel){: external} | Code-based policy guardrails and compliance automation using Sentinel. |
| [Control Groups](https://developer.hashicorp.com/vault/docs/enterprise/control-groups){: external} | Human approval workflows for high-value operations. |
{: caption="HashiCorp Vault policy resources" caption-side="bottom"}

### Namespaces
{: #vault-dedicated-namespace-resources}

| Topic | Description |
|-------|-------------|
| [Namespaces](https://developer.hashicorp.com/vault/docs/enterprise/namespaces){: external} | Namespace-based isolation for multi-team environments. |
| [Namespace structure](https://developer.hashicorp.com/vault/docs/enterprise/namespaces/namespace-structure){: external} | Guidance for designing your namespace hierarchy across teams and workloads. |
{: caption="HashiCorp Vault namespace resources" caption-side="bottom"}

### Vault UI
{: #vault-dedicated-ui-resources}

| Topic | Description |
|-------|-------------|
| [Vault UI](https://developer.hashicorp.com/vault/docs/ui){: external} | Use the native Vault web interface to manage secrets engines, auth methods, policies, and namespaces in your Vault Dedicated instance. |
{: caption="HashiCorp Vault UI resources" caption-side="bottom"}

### Audit logging
{: #vault-dedicated-audit-resources}

| Topic | Description |
|-------|-------------|
| [Audit devices](https://developer.hashicorp.com/vault/docs/audit){: external} | Configure Vault-side audit logging to capture a detailed record of all requests and responses in your instance. |
| [Audit best practices](https://developer.hashicorp.com/vault/docs/audit/best-practices){: external} | Guidance on structuring audit device configuration for production deployments. |
{: caption="HashiCorp Vault audit resources" caption-side="bottom"}
