---

copyright:
  years: 2026
lastupdated: "2026-11-05"

keywords: Vault Dedicated, database credentials engine, Gen2, ICD, IBM Cloud Databases, PostgreSQL, MongoDB, MySQL, Redis, Elasticsearch, dynamic credentials, DB engine

subcollection: secrets-manager

content-type: tutorial
services: secrets-manager, databases-for-postgresql, databases-for-mongodb, databases-for-mysql, databases-for-redis, databases-for-elasticsearch
account-plan: paid
completion-time: 60m

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

# Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen 2
{: #tutorial-db-credentials-engine-icd-gen2}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager, databases-for-postgresql, databases-for-mongodb, databases-for-mysql, databases-for-redis, databases-for-elasticsearch"}
{: toc-completion-time="60m"}

**Plan availability**: [Vault Dedicated]{: tag-green} plan only.

In this tutorial, you learn how to configure the database credentials engine in your Vault Dedicated instance to generate dynamic credentials for an {{site.data.keyword.cloud_notm}} Databases Gen2 instance. You can follow the same steps for PostgreSQL, MongoDB, MySQL, Redis, and Elasticsearch. Values that differ by database are listed in tables. For a Gen 1 PostgreSQL, MongoDB, MySQL, or Elasticsearch instance, see [Configuring the database credentials engine with {{site.data.keyword.cloud_notm}} Databases Gen 1](/docs/secrets-manager?topic=secrets-manager-tutorial-db-credentials-engine-icd-gen1).
{: shortdesc}

Dynamic database credentials are short-lived and scoped to the roles that you define. Instead of sharing long-lived passwords, Vault creates and revokes database users on demand. This reduces the risk of credential exposure and eliminates the need for manual credential rotation.

## What you set up
{: #db-credentials-icd-gen2-overview}

Your database is reachable only over the private network. In this tutorial, you set up the following components.

| Component | Purpose | Where you create it |
|-----------|---------|---------------------|
| Destination | Allows outbound traffic from {{site.data.keyword.secrets-manager_short}} to your database instance. It allowlists the private endpoint host of the database, so that Vault can connect to it. | {{site.data.keyword.cloud_notm}} console |
| Virtual Private Cloud (VPC) | A private network in {{site.data.keyword.cloud_notm}} that contains the test resources. | {{site.data.keyword.cloud_notm}} console |
| Virtual private endpoint (VPE) gateway | Gives resources in your VPC a private IP address for reaching your database instance. | {{site.data.keyword.cloud_notm}} console |
| Virtual server instance (VSI) | A virtual machine in your VPC that acts as a database client. You use it to prepare your database, if needed, and to test the credentials that Vault generates. Because the VSI is inside the VPC, it can reach the database over the private network. | {{site.data.keyword.cloud_notm}} console |
| Database credentials engine, connection, and role | The Vault configuration that generates the dynamic credentials. The connection tells Vault how to authenticate to your database, and the role defines the users that Vault creates. | Vault CLI |
{: caption="Components in this tutorial" caption-side="bottom"}

You create the destination and the network resources first. Then you use the VSI to prepare your database and, after you configure Vault, to test the credentials.

## Before you begin
{: #db-credentials-icd-gen2-prereqs}

Before you begin, make sure that you have the following:

- A [Vault Dedicated plan instance](/docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance&interface=ui) of {{site.data.keyword.secrets-manager_full}}.
- The [Vault CLI](https://developer.hashicorp.com/vault/install){: external}.
- The {{site.data.keyword.cloud_notm}} CLI with the [{{site.data.keyword.secrets-manager_short}} instance management CLI plug-in](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli), which you use to generate an admin token.

   ```sh
   ibmcloud plugin install secrets-manager-instance-management
   ```
   {: pre}

- A Gen2 {{site.data.keyword.cloud_notm}} Databases instance of one of the following types, with private endpoints enabled.

   | Database | Catalog |
   |----------|---------|
   | PostgreSQL | [Databases for PostgreSQL](https://cloud.ibm.com/catalog/services/databases-for-postgresql){: external} |
   | MongoDB | [Databases for MongoDB](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external} |
   | MySQL | [Databases for MySQL](https://cloud.ibm.com/catalog/services/databases-for-mysql){: external} |
   | Redis | [Databases for Redis](https://cloud.ibm.com/catalog/services/databases-for-redis){: external} |
   | Elasticsearch | [Databases for Elasticsearch](https://cloud.ibm.com/catalog/services/databases-for-elasticsearch){: external} |
   {: caption="Supported Gen2 databases" caption-side="bottom"}

   Only Gen2 {{site.data.keyword.cloud_notm}} Databases instances are covered in this tutorial.
   {: note}

- A service credential with the Manager role and private endpoints, generated for your database instance. See the User management topic in the documentation for your database. You need the hostname, port, username, and password from this credential.
- A [service-to-service (S2S) authorization](/docs/account?topic=account-serviceauth){: external} configured with:
  - **Source**: {{site.data.keyword.secrets-manager_short}}
  - **Target**: Your {{site.data.keyword.cloud_notm}} Databases instance
  - **Roles**: Authorization Delegator, Viewer

  Without this S2S authorization, {{site.data.keyword.secrets-manager_short}} cannot establish a destination to your database instance.
  {: important}

## Create a destination
{: #db-credentials-icd-gen2-destination}
{: step}

A destination allows outbound traffic from {{site.data.keyword.secrets-manager_short}} to reach a resource outside of the service, in this case your database instance. It allowlists the host of your database instance, which is always the private endpoint. Vault can connect to your database only if an active destination exists for it.

1. Open your {{site.data.keyword.secrets-manager_short}} instance.
2. In the navigation, click **Destinations**.
3. Click **Add destination**.
4. Enter a name, select the **Destination type** as **{{site.data.keyword.cloud_notm}} Databases** and add a description (optional).
5. Locate your database instance using one of the following options:
   - Select your instance from the **Instances** list.
   - Click the **Locate by CRN** tab and enter the CRN of your database instance.

Your destination now appears in the Destinations list. Wait until its status is marked as **Active** before proceeding to the next step. A destination that fails to become active typically means that private endpoints or the S2S authorization are not configured correctly. See [Troubleshooting](#db-credentials-icd-gen2-troubleshooting) for guidance.
{: note}

## Set up a network to test the credentials
{: #db-credentials-icd-gen2-network}
{: step}

Your database accepts connections only over the private network. To reach it for testing, you create the following resources:

- A VPC, which is the private network that contains the other resources.
- A subnet and a security group, which define the IP range of the VPC and the traffic that is allowed in and out.
- A VPE gateway that targets your database instance, so that resources in the VPC can reach the database at a private IP address.
- A VSI, which is the virtual machine that you use as a database client. You connect to it with an SSH key and a floating IP address, which is a public IP address that is attached to the VSI.

Using the {{site.data.keyword.cloud_notm}} console, go to **VPC infrastructure** and create the following resources in order. Create all of them in the same region and resource group as your database instance.

| Order | Resource | Settings |
|-------|----------|----------|
| 1 | Virtual private cloud (VPC) | **Name**: `vault-icd` |
| 2 | Security group | **Name**: `vault-icd-sg1`. **VPC**: the VPC from the previous row. Add an inbound rule and an outbound rule, each with **Protocol** set to `Any` and **Source** or **Destination** set to `0.0.0.0/0`. |
| 3 | Subnet | **Name**: `vault-icd-subnet1`. **Zone**: `<region>-1`. **Total IPv4 addresses**: `256`. |
| 4 | Virtual private endpoint gateway | **Name**: `vault-icd-vpe-gw`. **Target**: your {{site.data.keyword.cloud_notm}} Databases instance (a provider cloud service). **Security group**: `vault-icd-sg1`. **Reserved IP**: one reserved IP in `vault-icd-subnet1`. |
| 5 | SSH key | **Name**: `vault-icd-key`. Downloads a private key to your machine. |
| 6 | Virtual server instance | **Name**: `vault-icd-vsi1`. **Zone**: `<region>-1`. **Image**: `ibm-ubuntu-22-04-1-minimal-amd64-3`. **Profile**: `cx2-2x4`. **SSH key**: `vault-icd-key`. **Subnet** and **Security group**: `vault-icd-subnet1` and `vault-icd-sg1`. |
| 7 | Floating IP | **Name**: `vault-icd-fip1`. **Target**: the network interface of `vault-icd-vsi1`. |
{: caption="Network resources" caption-side="bottom"}

The security group rules in this tutorial allow all traffic from anywhere, for testing only. In production, restrict the rules to the source addresses and ports that your application and database need.
{: important}

When the VSI is running, log in to it through the floating IP.

```sh
mv ~/Downloads/vault-icd-key_rsa.priv ~/.ssh/
chmod 400 ~/.ssh/vault-icd-key_rsa.priv
ssh -i ~/.ssh/vault-icd-key_rsa.priv ubuntu@${floatingip}
```
{: pre}

| Variable | Description |
|----------|-------------|
| `${floatingip}` | The floating IP address of the VSI. |
{: caption="Variables for the VSI login" caption-side="bottom"}

## Connect to your database from the VSI
{: #db-credentials-icd-gen2-connect}
{: step}

Install the client for your database on the VSI, and look up the command that you use to connect. You use the command to create a database, if your database needs one, and later to test the credentials that Vault generates.

1. Install the client for your database on the VSI. Run `sudo apt-get update` first.

   | Database | Client | Install |
   |----------|--------|---------|
   | PostgreSQL | `psql` | `sudo apt-get install -y postgresql-common postgresql-client` |
   | MongoDB | `mongosh` | `wget https://downloads.mongodb.com/compass/mongosh-2.7.0-linux-x64.tgz`<br>`tar -zxvf mongosh-2.7.0-linux-x64.tgz`<br>`sudo cp mongosh-*-linux-x64/bin/mongosh /usr/local/bin/` |
   | MySQL | `mysql` | `wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client_8.4.0-1ubuntu22.04_amd64.deb"`<br>`wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client-core_8.4.0-1ubuntu22.04_amd64.deb"`<br>`wget https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-community-client-plugins_8.4.0-1ubuntu22.04_amd64.deb`<br>`wget "https://repo.mysql.com/apt/ubuntu/pool/mysql-8.4-lts/m/mysql-community/mysql-common_8.4.0-1ubuntu22.04_amd64.deb"`<br>`dpkg -i mysql-community-client-plugins_8.4.0-1ubuntu22.04_amd64.deb`<br>`dpkg -i mysql-community-client-core_8.4.0-1ubuntu22.04_amd64.deb`<br>`dpkg -i mysql-common_8.4.0-1ubuntu22.04_amd64.deb`<br>`dpkg -i mysql-community-client_8.4.0-1ubuntu22.04_amd64.deb` |
   | Redis | `redis-cli` | `sudo apt-get install -y redis-tools` |
   | Elasticsearch | `curl` | No installation is needed. |
   {: caption="Database clients" caption-side="bottom"}

2. Look up the command for your database.

   | Database | Command |
   |----------|---------|
   | PostgreSQL | `PGPASSWORD=${password} psql "host=${yourhost} port=${yourport} dbname=${yourdatabase} user=${username}"` |
   | MongoDB | `mongosh -u ${username} -p ${password} --tls --authenticationDatabase ${authdb} --host replset/${yourhosts}` |
   | MySQL | `mysql --tls-sni-servername=${yourhost} -h ${yourhost} -P ${yourport} -u ${username} -p'${password}' --ssl-mode=REQUIRED` |
   | Redis | `redis-cli -h ${yourhost} -p ${yourport} --user ${username} -a ${password} --tls --sni ${yourhost}` |
   | Elasticsearch | `curl -u ${username}:${password} "https://${yourhost}:${yourport}"` |
   {: caption="Connection commands by database" caption-side="bottom"}

   Replace the following variables with values from your service credential.

   | Variable | Description |
   |----------|-------------|
   | `${username}` | The username to connect with. Use the service credential username to create a database, and the username that Vault generated in [Generate credentials](#db-credentials-icd-gen2-generate) to test the credentials. |
   | `${password}` | The password for that username. |
   | `${yourhost}` | The hostname from your service credential. |
   | `${yourport}` | The port from your service credential. |
   | `${yourdatabase}` | The name of your database. For PostgreSQL, the default is `ibmclouddb`. For MongoDB and MySQL, use the database that you create in [Create a database](#db-credentials-icd-gen2-create-db). Not used for Redis and Elasticsearch. |
   | `${yourhosts}` | MongoDB only. All `hostname:port` pairs from your service credential, separated by commas. |
   | `${authdb}` | MongoDB only. The authentication database. Use `admin` when you connect with the service credential. Use `${yourdatabase}` when you connect with the credentials that Vault generated. |
   {: caption="Connection variables" caption-side="bottom"}

## Create a database (MongoDB and MySQL only)
{: #db-credentials-icd-gen2-create-db}
{: step}

For MongoDB and MySQL, the Vault connection and role refer to a user-created database rather than an {{site.data.keyword.cloud_notm}} created and managed database. Create it before you configure Vault. Skip this step for PostgreSQL, Redis, and Elasticsearch.

1. Choose a name for the database, such as `icdtest`. This is the `${yourdatabase}` value in the later steps.
2. On the VSI, connect to your database by using the command from [Connect to your database from the VSI](#db-credentials-icd-gen2-connect). Use the service credential username and password.
3. Create the database.

   | Database | Command |
   |----------|---------|
   | MongoDB | `db.getSiblingDB("${yourdatabase}").createCollection("init")` |
   | MySQL | `CREATE DATABASE ${yourdatabase};` |
   {: caption="Create database commands" caption-side="bottom"}

MongoDB creates a database only when you first store data in it, so the MongoDB command creates a collection.
{: note}

## Set up the Vault CLI
{: #db-credentials-icd-gen2-vault-cli}
{: step}

Authenticate the Vault CLI to your Vault Dedicated instance by using an admin token. You can run the Vault CLI from your own machine or from the VSI that you created in the previous step. If you use the VSI, install the Vault CLI and the {{site.data.keyword.cloud_notm}} CLI on it first.

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

The admin token is valid for 1 hour and is non-renewable. If it expires, generate a new one and update `VAULT_TOKEN`.
{: note}

If your instance uses private-only endpoints, the Vault API is reachable only through a VPE gateway that targets your Vault Dedicated instance. Run the Vault CLI from a machine that has this access, such as a VSI in a VPC with that VPE gateway. See [Using service endpoints to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
{: important}

## Enable the database credentials engine
{: #db-credentials-icd-gen2-enable-engine}
{: step}

Enable the database credentials engine at its default path.

```sh
vault secrets enable database
```
{: pre}

You can enable the engine once and add a connection and role for each database type that you want to use.
{: tip}

## Configure a database connection
{: #db-credentials-icd-gen2-connection}
{: step}

Create a connection that specifies how the database engine authenticates to your database instance. The command is the same for every database. Only the plug-in, the names, and the database-specific parameters differ.

For MongoDB and MySQL, the database that you named `${yourdatabase}` must exist before you create the connection. If you haven't created it, complete [Create a database](#db-credentials-icd-gen2-create-db) first.
{: important}

1. Look up the values for your database.

   | Database | `${plugin}` | `${connection}` | `${role}` | Username and password in the service credential |
   |----------|-------------|-----------------|-----------|--------------------------------------------------|
   | PostgreSQL | `postgresql-database-plugin` | `icd-postgresql` | `icd-postgresql-role` | `connection.postgres.authentication` |
   | MongoDB | `mongodb-database-plugin` | `icd-mongodb` | `icd-mongodb-role` | Top-level `username` and `password` |
   | MySQL | `mysql-database-plugin` | `icd-mysql` | `icd-mysql-role` | Top-level `username` and `password` |
   | Redis | `redis-database-plugin` | `icd-redis` | `icd-redis-role` | Top-level `username` and `password` |
   | Elasticsearch | `elasticsearch-database-plugin` | `icd-elasticsearch` | `icd-elasticsearch-role` | `connection.elasticsearch.authentication` |
   {: caption="Connection values by database" caption-side="bottom"}

2. Look up the database-specific parameters for your database, which replace `${parameters}` in the next step. For every database except Redis, build `${connection_url}` as described in the table. Your hostnames, ports, and query parameters come from your service credential.

   | Database | `${parameters}` | How to build `${connection_url}` | Example |
   |----------|-----------------|----------------------------------|---------|
   | PostgreSQL | `connection_url='${connection_url}'` | Copy `connection.postgres.composed[0]`. Replace the username with `{{username}}` and the password with `{{password}}`, and replace `ibmclouddb` with `${yourdatabase}`. | `postgres://{{username}}:{{password}}@${yourhost}:${yourport}/${yourdatabase}?sslmode=verify-full` |
   | MongoDB | `connection_url='${connection_url}'` | Copy `connection.mongodb.composed[0]`. Replace `$MONGOUSER` with `{{username}}` and `$MONGOPASS` with `{{password}}`, and replace `ibmclouddb` with `${yourdatabase}`. Keep every host in the list. | `mongodb://{{username}}:{{password}}@${yourhost1}:${yourport1},${yourhost2}:${yourport2},${yourhost3}:${yourport3}/${yourdatabase}?authSource=admin&replicaSet=replset` |
   | MySQL | `connection_url='${connection_url}'` | Build it from the hostname and port in `connection.mysql.hosts[0]`. | `{{username}}:{{password}}@tcp(${yourhost}:${yourport})/${yourdatabase}?tls=true` |
   | Redis | `host=${yourhost} port=${yourport} tls=true ca_cert=@isrg-root-x1.pem` | Not applicable. Redis doesn't use a URL. | Not applicable. |
   | Elasticsearch | `url='${connection_url}'` | Copy `connection.elasticsearch.composed[0]`. Replace `$PASSWORD` with `{{password}}`. Keep the username that is already in the URL. | `https://${yourusername}:{{password}}@${yourhost}:${yourport}` |
   {: caption="Database-specific connection parameters" caption-side="bottom"}

   Keep any query parameters, such as `sslmode`, `authSource`, and `replicaSet`, exactly as they appear in the `composed[0]` value of your service credential.

   Keep the `{{username}}` and `{{password}}` template placeholders exactly as shown and do not substitute real values. Vault replaces them at run time when it verifies and manages the connection. Replacing them with literal values causes the connection to fail. You must use the private endpoint.
   {: important}

3. For Redis only, download the ISRG Root X1 certificate from [Let's Encrypt certificates](https://letsencrypt.org/certificates/){: external} and save it as `isrg-root-x1.pem` in your current directory.

   Because of a known issue, Redis connections require a CA certificate, even though all Gen2 certificates are issued through Let's Encrypt. For all other databases, don't supply a certificate.
   {: important}

4. Create the connection.

   ```sh
   vault write database/config/${connection} \
     plugin_name=${plugin} \
     allowed_roles=${role} \
     verify_connection=true \
     username=${dbusername} \
     password=${dbpassword} \
     ${parameters}
   ```
   {: pre}

   Replace `${yourhost}`, `${yourport}`, and `${yourdatabase}` with the hostname and port from your service credential and the name of your database. Replace `${dbusername}` and `${dbpassword}` with the service credential username and password from the location shown in the last column of the table in step 1.

Vault verifies the connection when you create it. For the verification to succeed, the connection must use the private endpoint host of the database instance that you created the active destination for.
{: note}

## Create a role
{: #db-credentials-icd-gen2-role}
{: step}

A role defines the statements that Vault runs when it creates and revokes temporary database users.

For MongoDB and MySQL, the statements refer to the database that you created in [Create a database](#db-credentials-icd-gen2-create-db).
{: note}

1. Look up the creation and revocation statements for your database. Replace `${yourdatabase}` with the name of your database.

   | Database | Creation statements | Revocation statements |
   |----------|---------------------|-----------------------|
   | PostgreSQL | `CREATE USER "{{name}}" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}'; GRANT SELECT ON ALL TABLES IN SCHEMA public TO "{{name}}";` | `REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA public FROM "{{name}}"; DROP USER IF EXISTS "{{name}}";` |
   | MongoDB | `{"db":"${yourdatabase}","roles":[{"role":"read","db":"${yourdatabase}"}]}` | `{"db":"${yourdatabase}"}` |
   | MySQL | `CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}'; GRANT SELECT ON ${yourdatabase}.* TO '{{name}}'@'%';` | `REVOKE ALL PRIVILEGES ON ${yourdatabase}.* FROM '{{name}}'@'%'; DROP USER IF EXISTS '{{name}}'@'%';` |
   | Redis | None | None |
   | Elasticsearch | `{"elasticsearch_role_definition": {"indices": [{"names":["*"], "privileges":["read"]}]}}` | None |
   {: caption="Role statements by database" caption-side="bottom"}

   These are sample read-only roles. For production workloads, use least-privilege policies.
   {: note}

   For Redis, if you don't provide a creation statement, Vault creates a read-only user with the ACL `["~*", "+@read"]`, which can read any key.
   {: note}

2. Store the statements in shell variables. Paste the statements for your database from the table between the `EOF` lines, and replace `${yourdatabase}` where it appears. The quoted `'EOF'` delimiter stops the shell from interpreting the quotes, braces, and `$` characters in the statements. This example uses the PostgreSQL statements.

   ```sh
   creation_statement=$(cat <<'EOF'
   CREATE USER "{{name}}" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}'; GRANT SELECT ON ALL TABLES IN SCHEMA public TO "{{name}}";
   EOF
   )
   revocation_statement=$(cat <<'EOF'
   REVOKE ALL PRIVILEGES ON ALL TABLES IN SCHEMA public FROM "{{name}}"; DROP USER IF EXISTS "{{name}}";
   EOF
   )
   ```
   {: pre}

   Skip the variable for any statement that the table lists as None.

3. Create the role. Delete the `creation_statements` or `revocation_statements` line if your database has no such statement.

   ```sh
   vault write database/roles/${role} \
     db_name=${connection} \
     creation_statements="${creation_statement}" \
     revocation_statements="${revocation_statement}" \
     default_ttl=1h \
     max_ttl=1h
   ```
   {: pre}

   Use the same `${role}` and `${connection}` values that you used in [Configure a database connection](#db-credentials-icd-gen2-connection).

## Generate credentials
{: #db-credentials-icd-gen2-generate}
{: step}

Generate a set of dynamic credentials for your role.

```sh
vault read database/creds/${role}
```
{: pre}

Vault creates a temporary database user and returns a username and password. These credentials are valid for the time-to-live (TTL) configured on the role, after which Vault automatically runs the revocation statements to remove the user.

You need the username and password values that are returned in a later step.
{: note}

## Test the generated credentials
{: #db-credentials-icd-gen2-login}
{: step}

From the VSI, run the command for your database from [Connect to your database from the VSI](#db-credentials-icd-gen2-connect). Use the username and password that Vault generated in [Generate credentials](#db-credentials-icd-gen2-generate) as `${username}` and `${password}`. For MongoDB, set `${authdb}` to `${yourdatabase}`.

If you connect successfully, your dynamic credentials work.

## Troubleshooting
{: #db-credentials-icd-gen2-troubleshooting}

Review the following common issues and solutions if you encounter problems when setting up dynamic database credentials.

### Destination creation fails or stays in a non-succeeded state
{: #db-credentials-icd-gen2-ts-destination}

If your destination does not reach an **Active** state, check the following:

- Verify that the private endpoints are enabled for your {{site.data.keyword.cloud_notm}} Databases instance.
- Verify that a service-to-service (S2S) authorization is configured with **Source** set to {{site.data.keyword.secrets-manager_short}}, **Target** set to your {{site.data.keyword.cloud_notm}} Databases instance, and **Roles** set to **Authorization Delegator** and **Viewer**.

### Connection creation fails
{: #db-credentials-icd-gen2-ts-connection}

If the connection fails to create, verify the following configuration values:

- Make sure your destination has reached the **Active** state before configuring a connection.
- Confirm that the connection uses the private endpoint hostname of the database instance that you created the destination for.
- Do not replace the `{{username}}` and `{{password}}` placeholders in the connection URL with real values. These are Vault templates and must remain as-is.
- Double-check the host, port, and database name. These can be found in the service credential you created.
- For Redis, verify that `tls=true` is set and that `isrg-root-x1.pem` contains the ISRG Root X1 certificate. For all other databases, do not supply a certificate.
- Verify that `VAULT_ADDR`, `VAULT_NAMESPACE`, and `VAULT_TOKEN` are set, and that the admin token has not expired.

### Role creation or credential generation fails
{: #db-credentials-icd-gen2-ts-role}

If the role fails to create, or credentials fail to generate, check the following:

- Confirm that `db_name` matches the connection name, and that the role name is listed in the connection's `allowed_roles`.
- Verify that your creation statement is valid for your database type and version.
- Confirm that the credentials you provided when creating the connection have sufficient privileges to run the creation and revocation statements.

### Testing the generated credentials fails
{: #db-credentials-icd-gen2-ts-login}

If you cannot log in with the generated credentials, check the following:

| Database | Common cause |
|----------|--------------|
| PostgreSQL | The creation statement does not include `WITH LOGIN`. Without this clause, the created user cannot authenticate. |
| MongoDB | `${authdb}` is not set to `${yourdatabase}`, the database in the `db` field of the creation statement. |
| MySQL | The user was created for a different host than `'%'`, or the database name in the `GRANT` statement is wrong. |
| Redis | The client is not connecting with `--tls` and `--sni`. |
| Elasticsearch | The role definition doesn't grant `read` on the indices that you are querying. |
{: caption="Login failures by database" caption-side="bottom"}

### Network connection fails
{: #db-credentials-icd-gen2-ts-network}

If you cannot reach your database from the VSI, check the following:

| Symptom | Common cause |
|---------|--------------|
| Cannot log in to the VSI with SSH | The floating IP is not attached to the VSI, the security group blocks inbound traffic, or the private key file doesn't match the SSH key on the VSI. |
| The database hostname doesn't resolve to the reserved IP | The VPE gateway targets a different database instance, or private endpoints are not enabled on the database instance. |
| The connection times out | The security group of the VPE gateway or the VSI blocks the database port. |
{: caption="Network failures" caption-side="bottom"}

## Next steps
{: #db-credentials-icd-gen2-next-steps}

Now that you learned how to set up dynamic database credentials for {{site.data.keyword.cloud_notm}} Databases Gen2 by using {{site.data.keyword.secrets-manager_short}}, you can learn how to [revoke credentials](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-db-engine#vault-dedicated-db-engine-revoke) and [rotate root credentials](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-db-engine#vault-dedicated-db-engine-rotate-root). You can also explore more about Vault's database secrets engine capabilities in the [HashiCorp Vault database secrets engine documentation](https://developer.hashicorp.com/vault/docs/secrets/databases){: external}.

For more information about connecting to your database and configuring the Vault plug-in, see the following topics.

| Database | Connecting to Gen2 | Vault plug-in configuration |
|----------|--------------------|-----------------------------|
| PostgreSQL | Connecting with psql | [PostgreSQL database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/postgresql){: external} |
| MongoDB | Connecting from the console | [MongoDB database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/mongodb){: external} |
| MySQL | Connecting to MySQL | [MySQL/MariaDB database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/mysql-maria){: external} |
| Redis | Connecting with the CLI client | [Redis database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/redis){: external} |
| Elasticsearch | Connecting with curl | [Elasticsearch database secrets engine](https://developer.hashicorp.com/vault/docs/secrets/databases/elasticdb){: external} |
{: caption="Reference links by database" caption-side="bottom"}
