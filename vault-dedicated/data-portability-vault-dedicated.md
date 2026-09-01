---

copyright:
  years: 2026
lastupdated: "2026-09-01"

keywords: data portability, Vault Dedicated, export secrets, import secrets, Vault CLI, kv commands, snapshots

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Understanding data portability by using the Vault Dedicated plan
{: #data-portability-vault-dedicated}

[Data Portability](#x2113280){: term} involves a set of tools and procedures that enable customers to export the digital artifacts that are needed to implement similar workload and data processing on different service providers or on-premises software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on the customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities-vault-dedicated}

IBM Cloud services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures. This can involve:

- Planning and execution for setting up alternate infrastructure on different cloud providers or on-premises software that provide similar capabilities to the IBM services
- Planning and execution for porting the required application code on the alternate infrastructure, including the adaptation of customer's application code, deployment automation, and so on
- Conversion of the exported data and configuration to the format required by the alternate infrastructure and adapted applications

For more information about your responsibilities when using {{site.data.keyword.secrets-manager_short}}, see [Shared responsibilities for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-understanding-your-responsabilities).

## Data export procedures for Vault Dedicated
{: #data-portability-procedures-vault-dedicated}

The `Vault Dedicated` plan does not currently support automated [data replication](https://developer.hashicorp.com/vault/docs/internals/replication){: external} or the ability to [create and restore snapshots](https://developer.hashicorp.com/vault/docs/sysadmin/snapshots){: external} across different instances. You must create custom solutions to export and import their secrets and configurations using HashiCorp Vault's native tools and commands.

### Available export methods
{: #export-methods-vault-dedicated}

You can use the following HashiCorp Vault tools to export your data:

- **[Vault KV commands](https://developer.hashicorp.com/vault/docs/commands/kv){: external}**: Use the key-value secrets engine commands to read and export secrets.
- **[Vault CLI](https://developer.hashicorp.com/vault/docs/commands){: external}**: Use the command-line interface for comprehensive Vault operations.
- **[Vault API](https://developer.hashicorp.com/vault/api-docs){: external}**: Use the REST API for programmatic access to secrets and configurations.

### Exporting secrets using Vault KV commands
{: #export-vault-kv}

The Vault KV commands provide a straightforward way to export secrets from your Vault Dedicated instance:

```sh
# Set your Vault address and token
export VAULT_ADDR="https://{instance_id}.vault.{region}.secrets-manager.appdomain.cloud"
export VAULT_TOKEN="{your_vault_token}"

# Read a secret
vault kv get secret/myapp/config

# Read a secret in JSON format for easier processing
vault kv get -format=json secret/myapp/config > myapp-config.json

# List all secrets in a path
vault kv list secret/myapp
```
{: codeblock}

For more information, see [HashiCorp Vault KV commands documentation](https://developer.hashicorp.com/vault/docs/commands/kv){: external}.

### Exporting secrets using Vault CLI
{: #export-vault-cli}

The Vault CLI provides comprehensive access to all Vault operations:

```sh
# Read a secret using the CLI
vault read secret/data/myapp/config

# Export in JSON format
vault read -format=json secret/data/myapp/config > export.json
```
{: codeblock}

For more information about all available Vault CLI commands, see [HashiCorp Vault CLI documentation](https://developer.hashicorp.com/vault/docs/commands){: external}.

### Bulk export strategies
{: #bulk-export-strategies}

For exporting multiple secrets, consider creating scripts that iterate through your secrets paths:

```bash
#!/bin/bash
# List all secrets in a path
secrets=$(vault kv list -format=json secret/myapp | jq -r '.[]')

# Export each secret
for secret in $secrets; do
  vault kv get -format=json "secret/myapp/$secret" > "export-$secret.json"
done
```
{: codeblock}

### Importing secrets to a new instance
{: #import-secrets}

To import secrets into a new Vault Dedicated instance or another Vault deployment, you can use HashiCorp's [secrets import](https://developer.hashicorp.com/vault/docs/import){: external} capabilities or the Vault CLI:

```sh
# Set the new Vault instance details
export VAULT_ADDR="https://{new_instance_id}.vault.{region}.secrets-manager.appdomain.cloud"
export VAULT_TOKEN="{new_vault_token}"

# Import a secret using KV put
vault kv put secret/myapp/config @myapp-config.json

# Or use the Vault CLI
vault write secret/data/myapp/config @myapp-config.json
```
{: codeblock}

For more information about importing secrets, see [HashiCorp Vault secrets import documentation](https://developer.hashicorp.com/vault/docs/import){: external}.

## Exported data formats
{: #data-portability-data-formats-vault-dedicated}

Secrets exported from Vault Dedicated using the Vault CLI or API are in JSON format. The schema follows the HashiCorp Vault data structure, which includes:

- Secret data (key-value pairs)
- Metadata (version information, creation time, and so on)
- Custom metadata (if configured)

For detailed information about the data structure, see [HashiCorp Vault KV Secrets Engine documentation](https://developer.hashicorp.com/vault/docs/secrets/kv){: external}.

## Considerations and limitations
{: #data-portability-considerations-vault-dedicated}

When planning data portability for Vault Dedicated, consider the following:

- **No automated replication**: Vault Dedicated does not support [automated replication](https://developer.hashicorp.com/vault/docs/internals/replication){: external} across instances
- **No snapshot support**: The ability to [create and restore snapshots](https://developer.hashicorp.com/vault/docs/sysadmin/snapshots){: external} across different instances is not available
- **Custom solutions required**: You must implement your own backup and export processes using Vault CLI, KV commands, or the API
- **Authentication and policies**: Export and recreate authentication methods, policies, and access controls separately
- **Secrets engines configuration**: Document and recreate secrets engine configurations in the target environment
- **Namespace structure**: If using namespaces, ensure you export from and import to the correct namespace context
- **Encryption keys**: Vault's encryption keys are not exportable; the target instance will use its own encryption

## Data ownership
{: #data-portability-data-ownership-vault-dedicated}

All exported data is classified as customer content and therefore the full customer ownership and licensing rights apply to them, as stated in [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS).

## Next steps
{: #data-portability-next-steps-vault-dedicated}

- Review [HashiCorp Vault KV commands](https://developer.hashicorp.com/vault/docs/commands/kv){: external}
- Explore [HashiCorp Vault CLI documentation](https://developer.hashicorp.com/vault/docs/commands){: external}
- Learn about [HashiCorp Vault secrets import](https://developer.hashicorp.com/vault/docs/import){: external}
- Understand [Vault replication concepts](https://developer.hashicorp.com/vault/docs/internals/replication){: external}
- Review [Vault snapshot operations](https://developer.hashicorp.com/vault/docs/sysadmin/snapshots){: external}
