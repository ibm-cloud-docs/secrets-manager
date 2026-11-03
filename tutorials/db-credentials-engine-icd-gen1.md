---

copyright:
  years: 2026
lastupdated: "2026-11-03"

keywords: Vault Dedicated, database credentials engine, Gen 1, ICD, IBM Cloud Databases, PostgreSQL, MongoDB, MySQL, Elasticsearch, dynamic credentials, DB engine

subcollection: secrets-manager

content-type: tutorial
services: secrets-manager, databases-for-postgresql, databases-for-mongodb, databases-for-mysql, databases-for-elasticsearch
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

# Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen 1
{: #tutorial-db-credentials-engine-icd-gen1}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager, databases-for-postgresql, databases-for-mongodb, databases-for-mysql, databases-for-elasticsearch"}
{: toc-completion-time="30m"}

**Plan availability**: [Vault Dedicated]{: tag-green} plan only.

In this tutorial, you learn how to configure the database credentials engine in your Vault Dedicated instance to generate dynamic credentials for an {{site.data.keyword.cloud_notm}} Databases Gen 1 instance. You can follow the same steps for PostgreSQL, MongoDB, MySQL, and Elasticsearch. Values that differ by database are listed in tables. This tutorial applies to Gen 1 instances only.
{: shortdesc}

Dynamic database credentials are short-lived and scoped to the roles that you define. Instead of sharing long-lived passwords, Vault creates and revokes database users on demand. This reduces the risk of credential exposure and eliminates the need for manual credential rotation.

This tutorial uses the Vault Web UI and a TLS certificate from your service credential. If your instance is Gen2, or if you use Redis, follow [Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen2](/docs/secrets-manager?topic=secrets-manager-tutorial-db-credentials-engine-icd-gen2) instead.
{: note}

## Before you begin
{: #db-credentials-icd-gen1-prereqs}

Before you begin, make sure that you have the following:

- A [Vault Dedicated plan instance](/docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance&interface=ui) of {{site.data.keyword.secrets-manager_full}}.
- A Gen 1 {{site.data.keyword.cloud_notm}} Databases instance of one of the following types, with private endpoints enabled.

   | Database | Catalog |
   |----------|---------|
   | PostgreSQL | [Databases for PostgreSQL](https://cloud.ibm.com/catalog/services/databases-for-postgresql){: external} |
   | MongoDB | [Databases for MongoDB](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external} |
   | MySQL | [Databases for MySQL](https://cloud.ibm.com/catalog/services/databases-for-mysql){: external} |
   | Elasticsearch | [Databases for Elasticsearch](https://cloud.ibm.com/catalog/services/databases-for-elasticsearch){: external} |
   {: caption="Supported Gen 1 databases" caption-side="bottom"}

   Only Gen 1 {{site.data.keyword.cloud_notm}} Databases instances are covered in this tutorial.
   {: note}

- A service credential generated for your database instance. See the User management topic in the documentation for your database. You need the hostname, port, username, password, and Transport Security Layer (TLS) certificate from this credential. For MongoDB, you also need the password of the `admin` user.
- A [service-to-service (S2S) authorization](/docs/account?topic=account-serviceauth){: external} configured with:
  - **Source**: {{site.data.keyword.secrets-manager_short}}
  - **Target**: Your {{site.data.keyword.cloud_notm}} Databases instance
  - **Roles**: Authorization Delegator, Viewer

  Without this S2S authorization, {{site.data.keyword.secrets-manager_short}} cannot establish a destination to your database instance.
  {: important}

- A client for your database, to test the credentials that Vault generates. The client must run on a host that can reach your database over the {{site.data.keyword.cloud_notm}} private network, such as {{site.data.keyword.cloud_notm}} Shell.
- For Elasticsearch only, the [Vault CLI](https://developer.hashicorp.com/vault/install){: external}, and the {{site.data.keyword.cloud_notm}} CLI with the [{{site.data.keyword.secrets-manager_short}} instance management CLI plug-in](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli), which you use to generate an admin token.

   ```sh
   ibmcloud plugin install secrets-manager-instance-management
   ```
   {: pre}

## Create a destination
{: #db-credentials-icd-gen1-destination}
{: step}

A destination allows outbound traffic from {{site.data.keyword.secrets-manager_short}} to reach a resource outside of the service, in this case your database instance. It allowlists the private endpoint host of your database instance. Vault can connect to your database only if an active destination exists for it.

1. Open your {{site.data.keyword.secrets-manager_short}} instance.
2. In the navigation, click **Destinations**.
3. Click **Add destination**.
4. Enter a name, select the **Destination type** as **{{site.data.keyword.cloud_notm}} Databases** and add a description (optional).
5. Locate your database instance using one of the following options:
   - Select your instance from the **Instances** list.
   - Click the **Locate by CRN** tab and enter the CRN of your database instance.

Your destination now appears in the Destinations list. Wait until its status is marked as **Active** before proceeding to the next step. A destination that fails to become active typically means that private endpoints or the S2S authorization are not configured correctly. See [Troubleshooting](#db-credentials-icd-gen1-troubleshooting) for guidance.
{: note}

## Enable the database credentials engine
{: #db-credentials-icd-gen1-enable-engine}
{: step}

Enable the database credentials engine in the Vault Web UI.

1. From your {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Launch web UI**.
2. In the Vault Web UI, go to **Secrets** > **Secrets engines**, and click **Enable new engine**.
3. Select **Database** and keep all fields at their default values.
4. Click **Enable engine**.

You can enable the engine once and add a connection and role for each database type that you want to use.
{: tip}

## Set up the Vault CLI (Elasticsearch only)
{: #db-credentials-icd-gen1-vault-cli}
{: step}

The Vault Web UI can't accept the Elasticsearch TLS certificate as inline content in a Vault Dedicated instance, so you create the Elasticsearch connection by using the Vault CLI. Skip this step for PostgreSQL, MongoDB, and MySQL.

1. Log in to {{site.data.keyword.cloud_notm}} and generate an admin token for your instance.

   ```sh
   ibmcloud login
   ibmcloud secrets-manager-instance-management admin-token-create --id ${instanceid}
   ```
   {: pre}

2. Set the environment variables for the Vault CLI.

   ```sh
   export VAULT_ADDR="${vaultapi}"
   export VAULT_NAMESPACE="admin"
   export VAULT_TOKEN="${admintoken}"
   ```
   {: pre}

   | Variable | Description |
   |----------|-------------|
   | `${instanceid}` | The ID of your Vault Dedicated instance. |
   | `${vaultapi}` | The `vault_api` URL from the endpoints of your instance. See [Service endpoints for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-endpoints#vault-dedicated-service-endpoints). |
   | `${admintoken}` | The admin token that was returned in the previous step. |
   {: caption="Vault CLI variables" caption-side="bottom"}

3. Verify that the CLI can reach Vault.

   ```sh
   vault token lookup
   ```
   {: pre}

The admin token is valid for 1 hour and is non-renewable. If it expires, generate a new one and update `VAULT_TOKEN`. Treat the token as sensitive.
{: note}

If your instance uses private-only endpoints, the Vault API is reachable only through a VPE gateway that targets your Vault Dedicated instance. Run the Vault CLI from a machine that has this access. See [Using service endpoints to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
{: important}

## Configure a database connection
{: #db-credentials-icd-gen1-connection}
{: step}

Create a connection that specifies how the database engine authenticates to your database instance. For PostgreSQL, MongoDB, and MySQL, use the Vault Web UI. For Elasticsearch, use the Vault CLI.

1. Look up the values for your database. Your hostnames, ports, and credentials come from your service credential.

   | Database | Database plugin | Connection name | Username and password |
   |----------|-----------------|-----------------|-----------------------|
   | PostgreSQL | PostgreSQL | `pgdb` | The username and password from your service credential. |
   | MongoDB | MongoDB | `mongodbdb` | The `admin` user and its password. Don't use the `ibm_cloud_` service credential user. It doesn't have enough privileges to create and remove database users. |
   | MySQL | MySQL/MariaDB | `mysqldb` | The username and password from your service credential. |
   | Elasticsearch | `elasticsearch-database-plugin` | `elasticsearchdb` | The username and password from your service credential. The username usually begins with `ibm_cloud_`. Don't use `admin`. |
   {: caption="Connection values by database" caption-side="bottom"}

   For MySQL, select **MySQL/MariaDB**. Do not select **MySQL (Aurora)**, **MySQL (Legacy)**, or **MySQL (RDS)**.
   {: important}

2. For PostgreSQL, MongoDB, and MySQL, build the connection URL.

   | Database | Connection URL |
   |----------|----------------|
   | PostgreSQL | `postgres://{{username}}:{{password}}@${yourhost}.private.databases.appdomain.cloud:${yourport}/ibmclouddb?sslmode=verify-full` |
   | MongoDB | `mongodb://{{username}}:{{password}}@${host0}.private.databases.appdomain.cloud:${port},${host1}.private.databases.appdomain.cloud:${port},${host2}.private.databases.appdomain.cloud:${port}/admin?authSource=admin&replicaSet=replset&tls=true` |
   | MySQL | `{{username}}:{{password}}@tcp(${yourhost}.private.databases.appdomain.cloud:${yourport})/ibmclouddb?tls=true` |
   {: caption="Connection URLs by database" caption-side="bottom"}

   Replace `${yourhost}`, `${host0}`, `${host1}`, `${host2}`, `${yourport}`, and `${port}` with the private hostnames and port from your service credential. Keep the `{{username}}` and `{{password}}` template placeholders exactly as shown and do not substitute real values. Vault replaces them at run time when it verifies and manages the connection. Replacing them with literal values causes the connection to fail. You must use the private endpoint.
   {: important}

   The MySQL URL uses the Go MySQL driver data source name (DSN) format. Don't add a `mysql://` prefix.
   {: note}

3. For PostgreSQL, MongoDB, and MySQL, create the connection in the Vault Web UI.

   1. From the database credentials engine, click **Create connection**.
   2. Select the **Database plugin**, enter the **Connection name**, **Connection URL**, **Username**, and **Password** from the previous steps.
   3. Expand **TLS options** and paste the TLS certificate from your service credential in the **TLS CA** field, or upload it as a file.
   4. Click **Create database**.

4. For Elasticsearch, create the connection by using the Vault CLI.

   1. Save the TLS certificate from your Elasticsearch service credential to a local file.

      ```sh
      cat > /tmp/es-ca.pem <<'EOF_CA'
      <paste TLS certificate from your service credential here>
      EOF_CA
      ```
      {: pre}

   2. Create the connection.

      ```sh
      vault write database/config/elasticsearchdb \
        plugin_name="elasticsearch-database-plugin" \
        allowed_roles="logindb-elasticsearch" \
        url="https://${yourhost}.private.databases.appdomain.cloud:${yourport}" \
        username="${dbusername}" \
        password="${dbpassword}" \
        ca_pem=@/tmp/es-ca.pem
      ```
      {: pre}

      Replace `${yourhost}` and `${yourport}` with the hostname and port from your service credential. Replace `${dbusername}` and `${dbpassword}` with the service credential username and password. The URL contains only the HTTPS endpoint. Don't add `{{username}}:{{password}}` to it.

## Create a role
{: #db-credentials-icd-gen1-role}
{: step}

A role defines the statements that Vault runs when it creates and revokes temporary database users.

1. From the database credentials engine, click **Create role**.
2. Enter the following role details:

   | Database | Role name | Connection name | Type of role |
   |----------|-----------|-----------------|--------------|
   | PostgreSQL | `logindb-postgresql` | `pgdb` | Dynamic |
   | MongoDB | `logindb-mongodb` | `mongodbdb` | Dynamic |
   | MySQL | `logindb-mysql` | `mysqldb` | Dynamic |
   | Elasticsearch | `logindb-elasticsearch` | `elasticsearchdb` | Dynamic |
   {: caption="Role configuration by database" caption-side="bottom"}

3. Look up the creation and revocation statements for your database, and enter them in the **Creation statements** and **Revocation statements** fields.

   | Database | Creation statements | Revocation statements |
   |----------|---------------------|-----------------------|
   | PostgreSQL | `CREATE USER "{{name}}" WITH LOGIN ENCRYPTED PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';`<br>`GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "{{name}}";` | `REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA public FROM "{{name}}";`<br>`DROP ROLE IF EXISTS "{{name}}";` |
   | MongoDB | `{"db": "admin", "roles": [{"role": "readWrite", "db": "ibmclouddb"}]}` | None |
   | MySQL | `CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';`<br>`GRANT SELECT, INSERT, UPDATE, DELETE ON ibmclouddb.* TO '{{name}}'@'%';` | `REVOKE ALL PRIVILEGES, GRANT OPTION FROM '{{name}}'@'%';`<br>`DROP USER IF EXISTS '{{name}}'@'%';` |
   | Elasticsearch | `{"elasticsearch_roles": ["superuser"]}` | None |
   {: caption="Role statements by database" caption-side="bottom"}

   For MySQL, enter each SQL statement separately, and click **Add** after each one.
   {: note}

   For MongoDB and Elasticsearch, leave **Revocation statements** empty. The plug-in removes the user automatically when its lease expires or is revoked.
   {: note}

   These are sample roles only. The Elasticsearch `superuser` role is highly privileged. For production workloads, use least-privilege policies.
   {: important}

4. Click **Save**.

## Generate credentials
{: #db-credentials-icd-gen1-generate}
{: step}

Generate a set of dynamic credentials for your role.

1. From the database credentials engine, select the role that you created.
2. Click **Generate credentials**.

Vault creates a temporary database user and returns a username, a password, and a lease ID. These credentials are valid for the time-to-live (TTL) configured on the role, after which Vault automatically revokes them and removes the user.

You need the username and password values that are returned in the next step.
{: note}

## Test the generated credentials
{: #db-credentials-icd-gen1-login}
{: step}

Use the generated credentials to connect to your database from a host that can reach it over the private network.

1. Install the client for your database on the host, if it isn't already installed. The following commands are for Ubuntu. Run `sudo apt-get update` first. On other operating systems, use the equivalent commands.

   | Database | Client | Install |
   |----------|--------|---------|
   | PostgreSQL | `psql` | `sudo apt-get install -y postgresql-common postgresql-client` |
   | MongoDB | `mongosh` | `wget https://downloads.mongodb.com/compass/mongosh-2.3.1-linux-x64.tgz`<br>`tar -zxvf mongosh-2.3.1-linux-x64.tgz`<br>`export PATH="$PATH:$(pwd)/mongosh-2.3.1-linux-x64/bin"` |
   | MySQL | `mysql` | `wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client_8.4.0-1ubuntu22.04_amd64.deb"`<br>`wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client-core_8.4.0-1ubuntu22.04_amd64.deb"`<br>`wget https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client-plugins_8.4.0-1ubuntu22.04_amd64.deb`<br>`wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-common_8.4.0-1ubuntu22.04_amd64.deb"`<br>`sudo dpkg -i mysql-community-client-plugins_8.4.0-1ubuntu22.04_amd64.deb`<br>`sudo dpkg -i mysql-community-client-core_8.4.0-1ubuntu22.04_amd64.deb`<br>`sudo dpkg -i mysql-common_8.4.0-1ubuntu22.04_amd64.deb`<br>`sudo dpkg -i mysql-community-client_8.4.0-1ubuntu22.04_amd64.deb` |
   | Elasticsearch | `curl` | No installation is needed. |
   {: caption="Database clients" caption-side="bottom"}

2. Save the TLS certificate from your service credential to a local file, such as `/path/to/TLS_certificate`.

3. Run the command for your database.

   | Database | Command |
   |----------|---------|
   | PostgreSQL | `PGPASSWORD=${password} PGSSLROOTCERT=/path/to/TLS_certificate psql "host=${yourhost} port=${yourport} dbname=ibmclouddb user=${username} sslmode=verify-full"` |
   | MongoDB | `mongosh "mongodb://${username}:${password}@${host0}:${port},${host1}:${port},${host2}:${port}/admin?authSource=admin&replicaSet=replset" --tls --tlsCAFile /path/to/TLS_certificate --eval 'db.runCommand({connectionStatus:1})'` |
   | MySQL | `mysql --host=${yourhost} --port=${yourport} --user=${username} --password=${password} --ssl-ca=/path/to/TLS_certificate --ssl-mode=VERIFY_CA ibmclouddb` |
   | Elasticsearch | `curl -s --cacert /path/to/TLS_certificate -u "${username}:${password}" "https://${yourhost}:${yourport}/_cluster/health"` |
   {: caption="Login commands by database" caption-side="bottom"}

   Replace the following variables.

   | Variable | Description |
   |----------|-------------|
   | `${username}` | The username that Vault generated in [Generate credentials](#db-credentials-icd-gen1-generate). |
   | `${password}` | The password that Vault generated in [Generate credentials](#db-credentials-icd-gen1-generate). |
   | `${yourhost}` | The hostname from your service credential. |
   | `${yourport}` | The port from your service credential. |
   | `${host0}`, `${host1}`, `${host2}`, `${port}` | MongoDB only. The hostnames and port from your service credential. |
   | `/path/to/TLS_certificate` | The local path to the TLS certificate from your service credential. |
   {: caption="Login command variables" caption-side="bottom"}

   Use a hostname that is reachable from the host where you run the command. Your service credential contains both public and private hostnames.
   {: note}

Your dynamic credentials work if you see the following results.

| Database | Expected result |
|----------|-----------------|
| PostgreSQL | The `psql` prompt opens. |
| MongoDB | The output includes the generated user under `authenticatedUsers` and the assigned roles under `authenticatedUserRoles`. |
| MySQL | The `mysql` prompt opens. |
| Elasticsearch | The response is the cluster health JSON. The `status` can be `green`, `yellow`, or `red`. Any of these values confirms that authentication succeeded. |
{: caption="Expected results by database" caption-side="bottom"}

## Troubleshooting
{: #db-credentials-icd-gen1-troubleshooting}

Review the following common issues and solutions if you encounter problems when setting up dynamic database credentials.

### Destination creation fails or stays in a non-succeeded state
{: #db-credentials-icd-gen1-ts-destination}

If your destination does not reach an **Active** state, check the following:

- Verify that the private endpoints are enabled for your {{site.data.keyword.cloud_notm}} Databases instance.
- Verify that a service-to-service (S2S) authorization is configured with **Source** set to {{site.data.keyword.secrets-manager_short}}, **Target** set to your {{site.data.keyword.cloud_notm}} Databases instance, and **Roles** set to **Authorization Delegator** and **Viewer**.

### Connection creation fails
{: #db-credentials-icd-gen1-ts-connection}

If the connection fails to create, verify the following configuration values:

- Make sure your destination has reached the **Active** state before configuring a connection.
- Confirm that the CRN you passed when creating the destination matches the database instance that the connection refers to.
- Confirm that the connection uses the private endpoint hostname (`.private.databases.appdomain.cloud`).
- Do not replace the `{{username}}` and `{{password}}` placeholders in the connection URL with real values. These are Vault templates and must remain as-is. This doesn't apply to the Elasticsearch URL, which doesn't use placeholders.
- Double-check the host, port, and database name. These can be found in the service credential you created.
- Verify that you pasted the TLS certificate from your service credential in the **TLS CA** field under **TLS options**. TLS certificates apply only to {{site.data.keyword.cloud_notm}} Databases Gen 1 instances.
- Verify that you are using the correct user for your database:

   | Database | Check |
   |----------|-------|
   | MongoDB | Use the `admin` user, not the `ibm_cloud_` service credential user. The connection URL includes `authSource=admin` and `replicaSet=replset`. |
   | MySQL | The plug-in is **MySQL/MariaDB**, and the URL has no `mysql://` prefix. |
   | Elasticsearch | Use the `ibm_cloud_` service credential user, not `admin`. The error `unable to parse connection url` means that the URL contains `{{username}}:{{password}}`. The error `timeout exceeded` means that the destination isn't active or the hostname isn't a private hostname. A `401` error means that the username or password is wrong. |
   {: caption="Connection checks by database" caption-side="bottom"}

- For Elasticsearch, verify that `VAULT_ADDR`, `VAULT_NAMESPACE`, and `VAULT_TOKEN` are set, and that the admin token has not expired.

### Role creation fails
{: #db-credentials-icd-gen1-ts-role}

If the role fails to create, check the following:

- Select **Dynamic** as the role type.
- Verify that your creation statement is valid for your database type and version. For Elasticsearch and MongoDB, the statement must be valid JSON.
- For MySQL, enter each statement separately, and click **Add** after each one.
- For Elasticsearch, confirm that the role name is listed in the `allowed_roles` of the connection.

### Credential generation fails
{: #db-credentials-icd-gen1-ts-generate}

If credentials fail to generate, verify that the connection is configured with sufficient database privileges:

- Confirm that the credentials you provided when creating the connection have sufficient privileges to run the creation and revocation statements in the target database. For MongoDB, the `admin` user must be able to create and drop users. For Elasticsearch, the service credential user must be able to use the `_security/user` API.

### Testing the generated credentials fails
{: #db-credentials-icd-gen1-ts-login}

If you cannot log in with the generated credentials, check the following:

| Database | Common cause |
|----------|--------------|
| All | The TTL of the credentials expired. Generate new credentials. You aren't connecting from a host with access to the {{site.data.keyword.cloud_notm}} private network. The TLS certificate path or content is wrong. |
| PostgreSQL | The creation statement does not include `WITH LOGIN`. Without this clause, the created user cannot authenticate. |
| MongoDB | The authentication database isn't `admin`, the database in the `db` field of the creation statement. |
| MySQL | The user was created for a different host than `'%'`. |
| Elasticsearch | The assigned Elasticsearch role doesn't allow the operation that you are running. |
{: caption="Login failures by database" caption-side="bottom"}

## Next steps
{: #db-credentials-icd-gen1-next-steps}

Now that you learned how to set up dynamic database credentials for {{site.data.keyword.cloud_notm}} Databases Gen 1 by using {{site.data.keyword.secrets-manager_short}}, you can learn how to [revoke credentials](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-db-engine#vault-dedicated-db-engine-revoke) and [rotate root credentials](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-db-engine#vault-dedicated-db-engine-rotate-root). You can also explore more about Vault's database secrets engine capabilities in the [HashiCorp Vault database secrets engine documentation](https://developer.hashicorp.com/vault/docs/secrets/databases){: external}.

If your database is a Gen2 instance, or you use Redis, see [Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen2](/docs/secrets-manager?topic=secrets-manager-tutorial-db-credentials-engine-icd-gen2).

For more information about the Vault plug-in configuration, see the following topics.

| Database | Vault plug-in configuration |
|----------|-----------------------------|
| PostgreSQL | [PostgreSQL database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/postgresql){: external} |
| MongoDB | [MongoDB database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/mongodb){: external} |
| MySQL | [MySQL/MariaDB database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/mysql-maria){: external} |
| Elasticsearch | [Elasticsearch database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/elasticdb){: external} |
{: caption="Reference links by database" caption-side="bottom"}
