---

copyright:
  years: 2026
lastupdated: "2026-08-28"

keywords: Vault Dedicated, secrets engines, KV, PKI, transit, database, SSH

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

# Working with secrets engines in Vault Dedicated
{: #vault-dedicated-secrets-engines}

Vault Dedicated exposes the full set of HashiCorp Vault Enterprise secrets engines. A secrets engine is a component that stores, generates, or encrypts data. You enable and configure secrets engines in your Vault Dedicated instance using the Vault UI, CLI, or API.
{: shortdesc}

## Supported secrets engines
{: #vault-dedicated-secrets-engines-supported}

The following secrets engines are most commonly used with Vault Dedicated:

| Secrets engine | What it does | HashiCorp docs |
|----------------|--------------|----------------|
| **KV v2** | Store and retrieve arbitrary static secrets as key-value pairs, with versioning support. | [KV secrets engine v2](https://developer.hashicorp.com/vault/docs/secrets/kv/kv-v2){: external} |
| **PKI** | Issue and manage X.509 certificates, including support for ACME clients for automated certificate lifecycle management. | [PKI secrets engine](https://developer.hashicorp.com/vault/docs/secrets/pki){: external} |
| **Transit** | Encryption-as-a-service: encrypt, decrypt, sign, verify, and derive keys without key material leaving Vault. | [Transit secrets engine](https://developer.hashicorp.com/vault/docs/secrets/transit){: external} |
| **Database** | Generate dynamic, short-lived credentials for databases. Eliminates the need for static database passwords. | [Database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases){: external} |
| **SSH** | Issue dynamic SSH credentials and signed certificates for secure host access. | [SSH secrets engine](https://developer.hashicorp.com/vault/docs/secrets/ssh){: external} |
{: caption="Supported secrets engines for Vault Dedicated" caption-side="bottom"}

For the full list of supported secrets engines, see the [HashiCorp Vault secrets engines documentation](https://developer.hashicorp.com/vault/docs/secrets){: external}.

## Before you begin
{: #vault-dedicated-secrets-engines-prereqs}

Before you enable a secrets engine, ensure that you have:

- A running Vault Dedicated instance. See [Creating a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
- An admin token for your instance. See [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens).
- An authentication method configured. See [Configuring authentication methods](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-auth-methods).

## Enabling a secrets engine
{: #vault-dedicated-secrets-engines-enable}

Use the Vault CLI or API to enable a secrets engine on your instance. The following example enables the KV v2 secrets engine.

1. Authenticate to your Vault Dedicated instance using your admin token.

    ```sh
    export VAULT_ADDR="https://<instance_id>.vault.<region>.secrets-manager.appdomain.cloud"
    export VAULT_TOKEN="<admin_token>"
    ```
    {: pre}

2. Enable the secrets engine at a path.

    ```sh
    vault secrets enable -path=secret kv-v2
    ```
    {: pre}

3. Write a secret.

    ```sh
    vault kv put secret/myapp/config username="myuser" password="mypassword"
    ```
    {: pre}

4. Read the secret.

    ```sh
    vault kv get secret/myapp/config
    ```
    {: pre}

For full configuration options and examples for each secrets engine, see the [HashiCorp Vault secrets engines documentation](https://developer.hashicorp.com/vault/docs/secrets){: external}.

## Next steps
{: #vault-dedicated-secrets-engines-next}

- [Configuring authentication methods](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-auth-methods)
- [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens)
- [HashiCorp Vault policies](https://developer.hashicorp.com/vault/docs/concepts/policies){: external}
- [HashiCorp Vault namespaces](https://developer.hashicorp.com/vault/docs/enterprise/namespaces){: external}
