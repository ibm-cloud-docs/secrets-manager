---

copyright:
  years: 2020, 2026
lastupdated: "2026-08-31"

keywords: secrets, secret types, supported secrets, static secrets, dynamic secrets,

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

# What is a secret?
{: #what-is-secret}

A secret is a piece of sensitive information. For example, an API key, password, or any type of credential that you might use to access a confidential system.
{: shortdesc}

By using secrets, you're able to authenticate to protected resources as you build your applications. For example, when you try to access an external service API, you're asked to provide a unique credential. After you supply your credential, the external service can understand who you are and whether you're authorized to interact with it.

## Working with secrets
{: #working-with-secrets}

For the [Trial and Standard]{: tag-blue} plans, secrets in {{site.data.keyword.secrets-manager_short}} can be static or dynamic, and are classified by type based on their general purpose. Each secret has a lifecycle that moves through defined states from creation to destruction.

The following topics apply:

- [Secret types and components](/docs/secrets-manager?topic=secrets-manager-secret-types): The types of secrets you can create, how each type is structured, and a feature comparison across types.
- [Secret states and transitions](/docs/secrets-manager?topic=secrets-manager-secret-types#secret-states-transitions): How secrets move through pre-activation, active, deactivated, and destroyed states during their lifecycle.

For the [Vault Dedicated]{: tag-green} plan, secrets are managed through native HashiCorp Vault secrets engines. Each engine handles a specific class of secret, from static key-value pairs to dynamically generated certificates and encryption keys.

The following topics apply:

- [Working with secrets engines](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-secrets-engines): An overview of how secrets engines work in Vault Dedicated and how to enable them.
- [Supported secrets engines](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-secrets-engines#vault-dedicated-secrets-engines-supported): The secrets engines available during the public beta, including KV v2, Transit, PKI, Transform, and TOTP.
