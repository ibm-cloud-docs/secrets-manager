---

copyright:
  years: 2026
lastupdated: "2026-08-28"

keywords: Vault Dedicated, authentication, auth methods, AppRole, Kubernetes, JWT, OIDC, token

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

# Configuring authentication methods
{: #vault-dedicated-auth-methods}

After you generate an admin token and complete initial setup of your Vault Dedicated instance, configure an authentication method so that your applications and teams can authenticate to Vault without depending on the admin token.
{: shortdesc}

The admin token is intended for initial configuration and emergency access only. For day-to-day operations, use a dedicated authentication method.
{: important}

## Supported authentication methods
{: #vault-dedicated-auth-methods-supported}

Vault Dedicated supports the full set of HashiCorp Vault authentication methods. The following methods are most commonly used:

| Auth method | Best for | HashiCorp docs |
|-------------|----------|----------------|
| **AppRole** | Machine-to-machine authentication for applications and services. | [AppRole auth method](https://developer.hashicorp.com/vault/docs/auth/approle){: external} |
| **Kubernetes** | Workloads running in Kubernetes clusters, authenticated using service account tokens. | [Kubernetes auth method](https://developer.hashicorp.com/vault/docs/auth/kubernetes){: external} |
| **JWT/OIDC** | Authenticating using JSON Web Tokens or OpenID Connect providers such as IBM Cloud IAM. | [JWT/OIDC auth method](https://developer.hashicorp.com/vault/docs/auth/jwt){: external} |
| **Token** | Direct token-based authentication. Used for initial setup and for generating child tokens with scoped policies. | [Token auth method](https://developer.hashicorp.com/vault/docs/auth/token){: external} |
{: caption="Supported authentication methods for Vault Dedicated" caption-side="bottom"}

For the full list of supported authentication methods, see the [HashiCorp Vault auth methods documentation](https://developer.hashicorp.com/vault/docs/auth){: external}.

## Before you begin
{: #vault-dedicated-auth-methods-prereqs}

Before you configure an authentication method, ensure that you have:

- A running Vault Dedicated instance. See [Creating a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
- An admin token for your instance. See [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens).
- The Vault CLI configured to connect to your instance. See the [HashiCorp Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external}.

## Enabling an auth method
{: #vault-dedicated-auth-methods-enable}

Use the Vault CLI or API to enable an authentication method on your instance. The following example enables the AppRole auth method.

1. Authenticate to your Vault Dedicated instance using your admin token.

    ```sh
    export VAULT_ADDR="https://<instance_id>.vault.<region>.secrets-manager.appdomain.cloud"
    export VAULT_TOKEN="<admin_token>"
    ```
    {: pre}

2. Enable the auth method.

    ```sh
    vault auth enable approle
    ```
    {: pre}

3. Create a role with the appropriate policies attached.

    ```sh
    vault write auth/approle/role/my-app \
        token_policies="my-policy" \
        token_ttl=1h \
        token_max_ttl=4h
    ```
    {: pre}

4. Retrieve the Role ID and generate a Secret ID for your application.

    ```sh
    vault read auth/approle/role/my-app/role-id
    vault write -f auth/approle/role/my-app/secret-id
    ```
    {: pre}

For full configuration options and examples for each auth method, see the [HashiCorp Vault auth methods documentation](https://developer.hashicorp.com/vault/docs/auth){: external}.

## Next steps
{: #vault-dedicated-auth-methods-next}

- [Working with secrets engines](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-secrets-engines)
- [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens)
- [HashiCorp Vault token concepts](https://developer.hashicorp.com/vault/docs/concepts/tokens){: external}
