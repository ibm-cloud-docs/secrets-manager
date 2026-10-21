---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: Vault Dedicated, database credentials engine, PostgreSQL, ICD, IBM Cloud Databases, dynamic credentials, DB engine

subcollection: secrets-manager

content-type: tutorial
services: secrets-manager, databases-for-postgresql
account-plan: paid
completion-time: 30m

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

# Configuring the database credentials engine with {{site.data.keyword.databases-for-postgresql_full}}
{: #tutorial-db-credentials-engine-postgresql}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager, databases-for-postgresql"}
{: toc-completion-time="30m"}

**Plan availability**: [Vault Dedicated]{: tag-green} plan only.

In this tutorial, you learn how to configure the database credentials engine in your Vault Dedicated instance to generate dynamic credentials for an {{site.data.keyword.databases-for-postgresql_full}} instance.
{: shortdesc}

Dynamic database credentials are short-lived and scoped to the roles that you define. Instead of sharing long-lived passwords, Vault creates and revokes database users on demand. This reduces the risk of credential exposure and eliminates the need for manual credential rotation.

## Before you begin
{: #db-credentials-postgresql-prereqs}

Before you begin, make sure that you have the following:

- A [Vault Dedicated plan instance](docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance&interface=ui) of {{site.data.keyword.secrets-manager_full}}.
- An [{{site.data.keyword.cloud_notm}} Databases for PostgreSQL](https://cloud.ibm.com/catalog/services/databases-for-postgresql){: external} instance.

   Only Gen 1 {{site.data.keyword.cloud_notm}} Databases instances are supported.
   {: note}

- A [service credential](https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-user-management&interface=ui#creating-users-in-_service-credentials_){: external} generated for your PostgreSQL instance. You need the hostname, port, username, password, and Transport Security Layer (TLS) certificate from this credential.
- Private endpoints enabled for your PostgreSQL instance.
<!-- TODO: re-enable S2S prereq when S2S requirement is confirmed for GA
- A [service-to-service (S2S) authorization](/docs/account?topic=account-serviceauth){: external} configured with:
  - **Source**: {[sm-short]}
  - **Target**: Your {{site.data.keyword.cloud_notm}} Databases for PostgreSQL instance
  - **Roles**: Authorization Delegator, Viewer

  Without this S2S authorization, {[sm-short]} cannot establish a destination to your database instance.
  {: important}
-->

## Create a destination
{: #db-credentials-postgresql-destination}
{: step}

A destination tells {{site.data.keyword.secrets-manager_short}} how to reach your PostgreSQL instance over the private network.

1. Open your {{site.data.keyword.secrets-manager_short}} instance.
2. In the navigation, click **Destinations**.
3. Click **Add destination**.
4. Enter a name, select the **Destination type** as **{{site.data.keyword.cloud_notm}} Databases** and add a description (optional).
5. Locate your PostgreSQL instance using one of the following options:
   - Select your instance from the **Instances** list.
   - Click the **Locate by CRN** tab and enter the CRN of your PostgreSQL instance.

Your destination now appears in the Destinations list. Wait until its status is marked as **Active** before proceeding to the next step. A destination that fails to become active typically means that private endpoints are not configured correctly. See [Troubleshooting](#db-credentials-postgresql-troubleshooting) for guidance.

{: note}

## Enable the database credentials engine
{: #db-credentials-postgresql-enable-engine}
{: step}

Enable the database credentials engine in the Vault Web UI.

1. From your {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Launch web UI**.
2. In the Vault Web UI, go to **Secrets** > **Secrets engines**, and click **Enable new engine**.
3. Select **Database** and keep all fields at their default values.
4. Click **Enable engine**.


## Configure a database connection
{: #db-credentials-postgresql-connection}
{: step}

Create a connection that specifies how the database engine authenticates to your PostgreSQL instance.

1. From the database credentials engine, click **Create connection**.
2. Enter the following connection details:

   | Field | Value |
   |-------|-------|
   | **Database plugin** | PostgreSQL |
   | **Connection name** | `pgdb` |
   | **Connection URL** | `postgres://{{username}}:{{password}}@${yourhost}.private.databases.appdomain.cloud:${yourport}/ibmclouddb?sslmode=verify-full` |
   | **Username** | The username from the service credential you generated in the prerequisite step. |
   | **Password** | The password from the service credential you generated in the prerequisite step. |
   {: caption="Database connection configuration" caption-side="bottom"}

   In the **Connection URL**, replace `${yourhost}` and `${yourport}` with the hostname and port from your service credential. Keep the `{{username}}` and `{{password}}` template placeholders exactly as shown and do not substitute real values. You must use the private endpoint.
   {: important}

3. Scroll down and expand **TLS options** to paste the TLS certificate from your PostgreSQL service credential in the **TLS CA** field, or upload it as a file.
4. Click **Create database**.

The `{{username}}` and `{{password}}` placeholders in the **Connection URL** are Vault templates. Vault replaces them at run time when it verifies and manages the connection. Replacing them with literal values causes the connection to fail.
{: note}


## Create a role
{: #db-credentials-postgresql-role}
{: step}

A role defines the SQL statements Vault runs when it creates and revokes temporary database users.

1. From the database credentials engine, click **Create role**.
2. Enter the following role details:

   | Field | Value |
   |-------|-------|
   | **Role name** | `logindb` |
   | **Connection name** | `pgdb` |
   | **Type of role** | Dynamic |
   {: caption="Role configuration" caption-side="bottom"}

3. In the **Creation statements** field, enter:

   ```sql
   CREATE USER "{{name}}" WITH LOGIN ENCRYPTED PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';
   GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "{{name}}";
   ```
   {: codeblock}

   This is a sample role only. Users are advised to create DB credentials using least privilege policies for their production workloads.
   {:note}

4. In the **Revocation statements** field, enter:

   ```sql
   REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA public FROM "{{name}}";
   DROP ROLE IF EXISTS "{{name}}";
   ```
   {: codeblock}

   This is a sample role only. Users are advised to create DB credentials using least privilege policies for their production workloads.
   {:note}

5. Click **Save**.


## Generate credentials
{: #db-credentials-postgresql-generate}
{: step}

Generate a set of dynamic credentials for the `logindb` role.

1. From the database credentials engine, select the `logindb` role.
2. Click **Generate credentials**.

Vault creates a temporary PostgreSQL user and returns a username and password. These credentials are valid for the time-to-live (TTL) configured on the role after which Vault automatically runs the revocation statements to remove the user.

You need the username and password values that are returned in the next step.
{: note}

## Log in to PostgreSQL
{: #db-credentials-postgresql-login}
{: step}

Use the generated credentials to connect to your PostgreSQL instance.

```sh
PGPASSWORD=${Password} PGSSLROOTCERT=/path/to/TLS_certificate \
  psql "host=${yourhost}.databases.appdomain.cloud \
        port=${yourport} \
        dbname=ibmclouddb \
        user=${Username} \
        sslmode=verify-full"
```
{: pre}

Replace the following variables with values from your PostgreSQL service credential.

| Variable | Description |
|----------|-------------|
| `${Password}` | The password generated in the previous step. |
| `${Username}` | The username generated in the previous step. |
| `/path/to/TLS_certificate` | The local path to the TLS CA certificate from your PostgreSQL service credential. |
| `${yourhost}` | The hostname from your PostgreSQL service credential. |
| `${yourport}` | The port from your PostgreSQL service credential. |
{: caption="psql login variables" caption-side="bottom"}


## Troubleshooting
{: #db-credentials-postgresql-troubleshooting}

Review the following common issues and solutions if you encounter problems when setting up dynamic database credentials for PostgreSQL.

### Destination creation fails or stays in a non-succeeded state
{: #db-credentials-postgresql-ts-destination}

If your destination does not reach an **Active** state, check the following:

- Verify that the private endpoints are enabled for your {{site.data.keyword.databases-for-postgresql_full}} instance.
<!-- TODO: re-enable S2S troubleshooting step when S2S requirement is confirmed for GA
- Verify that a service-to-service (S2S) authorization is configured with **Source** set to {[sm-short]}, **Target** set to your {{site.data.keyword.databases-for-postgresql_full}} instance, and **Roles** set to **Authorization Delegator** and **Viewer**.
-->

### Connection creation fails
{: #db-credentials-postgresql-ts-connection}

If the connection fails to create, verify the following configuration values:

- Make sure your destination has reached the `succeeded` state before configuring a connection.
- Confirm that the CRN you passed when creating the destination matches the PostgreSQL instance referenced in the **Connection URL**.
- Confirm that the **Connection URL** uses the private endpoint hostname (`.private.databases.appdomain.cloud`).
- Do not replace the `{{username}}` and `{{password}}` placeholders in the **Connection URL** with real values. These are Vault templates and must remain as-is.
- Double-check the `host`, `port`, and `dbname` values in the **Connection URL**. These can be found in the service credential you created. Verify that the `dbname` you specified exists in your PostgreSQL instance.
- Verify that you uploaded the TLS certificate to the **TLS CA** field under **TLS options**.

### Role creation fails
{: #db-credentials-postgresql-ts-role}

If the role fails to create, check the following:

- Select **Dynamic** as the role type.
- Verify that your creation statement is valid SQL for your PostgreSQL version.

### Credential generation fails
{: #db-credentials-postgresql-ts-generate}

If credentials fail to generate, verify that the connection is configured with sufficient database privileges:

- Confirm that the credentials you provided when creating the connection have sufficient privileges to execute the creation and revocation statements in the target database.

### Login with generated credentials fails
{: #db-credentials-postgresql-ts-login}

If you cannot log in with the generated credentials, check the role creation statement:

- Verify that your creation statement includes `WITH LOGIN` permissions. Without this clause, the created user cannot authenticate.

## Next steps
{: #db-credentials-postgresql-next-steps}

Now that you learned how to set up dynamic database credentials for PostgreSQL by using {{site.data.keyword.secrets-manager_short}}, you can explore more about Vault's database secrets engine capabilities in the [HashiCorp Vault database secrets engine documentation](https://developer.hashicorp.com/vault/docs/secrets/databases){: external}.
