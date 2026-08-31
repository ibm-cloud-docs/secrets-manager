---

copyright:
  years: 2026
lastupdated: "2026-08-31"

keywords: Secrets Manager, Vault Dedicated, API, admin tokens, instance details

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

# Instance Management API reference
{: #vault-dedicated-apis}

Use the IBM Cloud Secrets Manager Instance Management API to manage service instances of the `Vault Dedicated` plan. For Vault runtime operations such as secrets management, authentication methods, and policies, use the HashiCorp Vault API.
{: shortdesc}

## IBM Cloud Secrets Manager Instance Management API
{: #ibm-cloud-instance-management-api}

The IBM Cloud Secrets Manager Instance Management API provides control plane operations for managing your Vault Dedicated service instances. These APIs allow you to retrieve instance metadata, manage admin tokens, and configure instance settings.

### Authentication
{: #api-authentication}

All API requests require authentication using an IBM Cloud IAM token. Include your IAM token in the `Authorization` header of each request:

```sh
Authorization: Bearer {iam_token}
```
{: codeblock}

For information on generating IAM tokens, see [Creating an IAM access token for a user or service ID](/docs/iam?topic=iam-iamtoken_from_apikey).

### Base URL
{: #api-base-url}

The base URL for the Instance Management API is the control plane service endpoint for your instance. You can find this endpoint in the **Endpoints** page of your Secrets Manager service dashboard.

```
https://{instance_id}.{region}.secrets-manager.appdomain.cloud
```
{: codeblock}

## Getting instance details
{: #instance-details-api}

Use the instance details API to retrieve metadata about your Vault Dedicated service instance, including cluster state, endpoints, and key management service configuration.

### Getting instance details in the UI
{: #get-instance-details-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your Vault Dedicated instance.
3. In the instance dashboard, review the **Endpoints** section to find the cluster state, Vault API endpoint, and other instance details.

### Getting instance details from the CLI
{: #get-instance-details-cli}
{: cli}

To retrieve the details of your Vault Dedicated instance by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud resource service-instance <instance_name> --output json
```
{: pre}

The output includes the instance ID, region, and resource group details. To retrieve the Vault-specific endpoints, use the Instance Management API.

### Getting instance details with the API
{: #get-instance-details-api}
{: api}

Retrieve detailed information about your Vault Dedicated instance.

**Request**

```sh
GET /api/v2/instance
```
{: codeblock}

**Example request**

```sh
curl -X GET \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v2/instance"
```
{: codeblock}

**Response**

The response includes the following information:

- **Cluster state**: Current operational state of the Vault cluster
- **Endpoints**: API and Vault UI endpoints for your instance
- **Key management service**: Details about the KMS configuration for encryption
- **Region**: The region where your instance is deployed
- **Version**: The Vault version running on your instance

**Example response**

```json
{
  "id": "instance-id",
  "name": "my-vault-dedicated-instance",
  "region": "us-south",
  "cluster_state": "active",
  "endpoints": {
    "api": "https://instance-id.us-south.secrets-manager.appdomain.cloud",
    "vault": "https://instance-id.vault.us-south.secrets-manager.appdomain.cloud"
  },
  "kms": {
    "instance_id": "kms-instance-id",
    "key_id": "root-key-id"
  },
  "vault_version": "1.15.0"
}
```
{: codeblock}

### Getting instance details with Terraform
{: #instance-details-terraform}
{: terraform}

To get the details of a Vault Dedicated instance with Terraform, use the `ibm_sm_instance` data source.

The following example shows a configuration that you can use to get the instance details.

```terraform
data "ibm_sm_instance" "my_vault_dedicated_instance" {
  instance_id = local.instance_id
}
```
{: codeblock}

After your data source is created you can access its attributes to get the instance details. For example, to get the public Vault API endpoint use the following:

```
data.ibm_sm_instance.my_vault_dedicated_instance.endpoints.0.public.0.vault_api
```
{: codeblock}

## Managing admin tokens
{: #managing-admin-tokens}

Admin tokens provide root-level access to your Vault Dedicated cluster and are required for initial setup and administrative operations.

Admin tokens should be treated as highly sensitive credentials. Generate them only when needed for administrative tasks, and revoke them immediately after use.
{: important}

### Generating an admin token in the UI
{: #generate-admin-token-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your Vault Dedicated instance.
3. In the instance dashboard, click **Create token** in the **Create new admin token** section.
4. Copy the generated admin token and store it securely. You need this token to sign in to the Vault UI.

### Generating an admin token from the CLI
{: #generate-admin-token-cli}
{: cli}

To generate a new Vault admin token by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud secrets-manager instance-admin-token-create \
  --service-url "https://{instance_id}.{region}.secrets-manager.appdomain.cloud"
```
{: pre}

The command returns the Vault admin token. Store it securely — you need this token to sign in to the Vault UI.

### Generating an admin token with the API
{: #generate-admin-token-api}
{: api}

Generate a new Vault admin token for authenticating to your Vault Dedicated cluster. This token provides root-level access and should be used only for initial setup and administrative operations.

**Request**

```sh
POST /api/v2/instance/admin_token
```
{: codeblock}

**Example request**

```sh
curl -X POST \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v2/instance/admin_token"
```
{: codeblock}

**Response**

The response includes a Vault admin token that can be used to authenticate to the Vault API and UI.

**Example response**

```json
{
  "token": "hvs.CAESIJ...",
  "expires_at": "2024-01-15T10:30:00Z"
}
```
{: codeblock}

**Using the admin token**

Use the generated token to authenticate to the Vault API:

```sh
curl -X GET \
  -H "X-Vault-Token: hvs.CAESIJ..." \
  "https://{instance_id}.vault.{region}.secrets-manager.appdomain.cloud/v1/sys/health"
```
{: codeblock}

### Generating an admin token with Terraform
{: #generate-admin-token-terraform}
{: terraform}

To generate a new Vault admin token for authenticating to your Vault Dedicated cluster with Terraform, use the `ibm_sm_admin_token` resource.

The following example shows a configuration that you can use to generate an admin token.

```terraform
resource "ibm_sm_admin_token" "my_admin_token" {
  instance_id = local.instance_id
}
```
{: codeblock}

After your resource is created the vault admin token is stored in the `token` attribute. The token is valid for 1 hour and grants administrative privileges. The token is automatically refreshed if it expired or it is about to expire.

### Revoking all admin tokens in the UI
{: #revoke-admin-tokens-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your Vault Dedicated instance.
3. In the instance dashboard, click **Revoke** in the **Revoke all admin tokens** section.
4. Confirm the revocation when prompted.

Revoking the token immediately invalidates it and helps reduce the risk of unintended access.

### Revoking all admin tokens from the CLI
{: #revoke-admin-tokens-cli}
{: cli}

To revoke all active Vault admin tokens by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud secrets-manager instance-admin-token-revoke \
  --service-url "https://{instance_id}.{region}.secrets-manager.appdomain.cloud"
```
{: pre}

This operation immediately invalidates all admin tokens, requiring new tokens to be generated for future administrative access.

### Revoking all admin tokens with the API
{: #revoke-admin-tokens-api}
{: api}

Revoke all active Vault admin tokens for your instance. This operation immediately invalidates all admin tokens, requiring new tokens to be generated for future administrative access.

**Request**

```sh
DELETE /api/v2/instance/admin_token
```
{: codeblock}

**Example request**

```sh
curl -X DELETE \
  -H "Authorization: Bearer {iam_token}" \
  "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v2/instance/admin_token"
```
{: codeblock}

**Response**

A successful revocation returns a `204 No Content` status code.

## HashiCorp Vault API
{: #vault-dedicated-hashicorp-api}

For Vault runtime operations such as secrets management, authentication methods, policies, and secrets engines, use the HashiCorp Vault API and CLI documentation.

- [HashiCorp Vault API documentation](https://developer.hashicorp.com/vault/api-docs){: external}
- [Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external}

## Next steps
{: #vault-dedicated-apis-next-steps}

- Review the [HashiCorp Vault API documentation](https://developer.hashicorp.com/vault/api-docs){: external} for secrets management operations
- Learn about [Vault authentication methods](https://developer.hashicorp.com/vault/docs/auth){: external}
- Explore [Vault secrets engines](https://developer.hashicorp.com/vault/docs/secrets){: external}
