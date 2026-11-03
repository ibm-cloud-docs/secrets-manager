---

copyright:
  years: 2026
lastupdated: "2026-11-03"

keywords: Vault Dedicated, database secrets engine, dynamic credentials, IBM Cloud Databases, PostgreSQL, MySQL, MongoDB, Redis, Elasticsearch, ICD

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

# Working with the Databases secrets engine
{: #vault-dedicated-db-engine}

**Plan availability**: [Vault Dedicated]{: tag-green} plan only.

The Databases secrets engine generates short-lived, dynamic credentials for supported database engines. Instead of using long-lived shared passwords, Vault creates and revokes database users on demand according to the roles you define. This reduces the risk of credential exposure and eliminates the need for manual credential rotation.
{: shortdesc}

## How it works
{: #vault-dedicated-db-engine-how-it-works}

The Databases secrets engine uses a connectivity model that involves two layers:

1. **Destination** — A control-plane resource that establishes a private network link between your Vault Dedicated cluster and a target {{site.data.keyword.cloud_notm}} Databases instance. You create and manage destinations through the {{site.data.keyword.secrets-manager_short}} Instance Management API or CLI.
2. **Database secrets engine** — A Vault-native engine that authenticates to your database using a connection you configure. Vault uses this connection to create and revoke temporary database users according to the roles you define.

You must configure a destination before you can use the Databases secrets engine. The destination provides the network path; the engine handles authentication and credential lifecycle.

## Supported database types
{: #vault-dedicated-db-engine-supported-types}

The Databases secrets engine supports the following {{site.data.keyword.cloud_notm}} Databases instance types:

| Database | {{site.data.keyword.cloud_notm}} Databases service |
|----------|---------------------------------------------------|
| PostgreSQL | {{site.data.keyword.databases-for-postgresql_full}} |
| MySQL | {{site.data.keyword.databases-for-mysql_full}} |
| MongoDB | {{site.data.keyword.databases-for-mongodb_full}} |
| Redis | {{site.data.keyword.databases-for-redis_full}} |
| Elasticsearch | {{site.data.keyword.databases-for-elasticsearch_full}} |
{: caption="Supported database types for the Databases secrets engine" caption-side="bottom"}

Only {{site.data.keyword.cloud_notm}} Databases instances with private endpoints enabled are supported. {{site.data.keyword.databases-for-redis_full}} Gen 1 instances do not support dynamic roles, so they cannot be used with the Databases secrets engine. This is resolved in Gen2 instances.
{: note}

## Prerequisites
{: #vault-dedicated-db-engine-prereqs}

Before you configure the Databases secrets engine, ensure that you have the following:

- A running [Vault Dedicated]{: tag-green} instance. See [Setting up your Vault Dedicated instance](/docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance).
- An {{site.data.keyword.cloud_notm}} Databases instance with private endpoints enabled.
- A service-to-service IAM authorization policy configured with {{site.data.keyword.secrets-manager_short}} as the source and your {{site.data.keyword.cloud_notm}} Databases instance as the target, granting the **Viewer** and **Authorization Delegator** roles. See [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth){: external}.

  Without the service-to-service authorization, {{site.data.keyword.secrets-manager_short}} cannot establish a destination to the target database instance.
  {: important}

- A destination configured for your target database instance. See [Configuring outbound connectivity destinations](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-destinations).
- An admin token for your Vault Dedicated instance. See [Managing admin tokens](/docs/secrets-manager?topic=secrets-manager-manage-admin-tokens).

## Setup overview
{: #vault-dedicated-db-engine-setup-overview}

Setting up the Databases secrets engine involves the following steps:

1. Create a service-to-service IAM authorization policy.
2. Configure a destination to establish the private network link.
3. Enable the Databases secrets engine in the Vault Web UI or CLI.
4. Create a database connection that specifies how Vault authenticates to your database.
5. Define a role with the SQL or command statements that Vault runs to create and revoke users.
6. Generate dynamic credentials using the role.

## Database-specific configuration
{: #vault-dedicated-db-engine-db-specific}

Choose the tutorial that matches your database instance:

- [Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen2](/docs/secrets-manager?topic=secrets-manager-tutorial-db-credentials-engine-icd-gen2): Covers PostgreSQL, MongoDB, MySQL, Redis, and Elasticsearch on Gen2 instances. The plug-in, connection, and role values for each database are listed in tables.
- [Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen 1](/docs/secrets-manager?topic=secrets-manager-tutorial-db-credentials-engine-icd-gen1): Covers PostgreSQL, MongoDB, MySQL, and Elasticsearch on Gen 1 instances. You use the Vault Web UI (and the Vault CLI for Elasticsearch) and a TLS certificate from the service credential. The connection, role, and login values for each database are listed in tables.

For all other plug-in options, see the [HashiCorp Vault database secrets engine documentation](https://developer.hashicorp.com/vault/docs/secrets/databases){: external}.

## Revoking credentials
{: #vault-dedicated-db-engine-revoke}

Dynamic credentials are automatically revoked by Vault when their TTL expires. You can also revoke credentials manually at any time using the Vault CLI or API.

To revoke a lease manually:

```sh
vault lease revoke <lease_id>
```
{: pre}

After revocation, any attempt to authenticate to the database with the revoked credentials returns an authentication failure. This behavior confirms that the credential lifecycle is correctly managed by Vault.

For more information on lease revocation, see the [HashiCorp Vault lease documentation](https://developer.hashicorp.com/vault/docs/concepts/lease){: external}.

## Rotating root credentials
{: #vault-dedicated-db-engine-rotate-root}

The root credentials used to configure a database connection can be rotated without disrupting active dynamic credentials. When you rotate the root credentials, Vault generates a new password for the root user and updates the stored connection configuration.

To rotate root credentials:

```sh
vault write -force database/rotate-root/<connection_name>
```
{: pre}

After rotating the root credentials, generate a new set of dynamic credentials and verify that database connectivity succeeds. Existing leases that were issued before the rotation continue to work until they expire.

For more information, see [HashiCorp Vault database secrets engine — rotating root credentials](https://developer.hashicorp.com/vault/docs/secrets/databases#rotating-the-root-credentials){: external}.
