---

copyright:
  years: 2026
lastupdated: "2026-08-28"

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

### Get instance details
{: #get-instance-details}

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

### Get instance endpoints
{: #get-instance-endpoints}

Retrieve the API and Vault interface endpoints for your instance.

**Request**

```sh
GET /api/v2/instance/endpoints
```
{: codeblock}

**Example request**

```sh
curl -X GET \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v2/instance/endpoints"
```
{: codeblock}

**Example response**

```json
{
  "api": "https://instance-id.us-south.secrets-manager.appdomain.cloud",
  "vault": "https://instance-id.vault.us-south.secrets-manager.appdomain.cloud"
}
```
{: codeblock}

## Managing admin tokens
{: #managing-admin-tokens}

Admin tokens provide root-level access to your Vault Dedicated cluster and are required for initial setup and administrative operations. Use these APIs to generate new admin tokens or revoke all existing admin tokens.

Admin tokens should be treated as highly sensitive credentials. Generate them only when needed for administrative tasks, and revoke them immediately after use.
{: important}

### Generate an admin token
{: #generate-admin-token}

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

### Revoke all admin tokens
{: #revoke-admin-tokens}

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
{: #vault-api}

For Vault runtime operations such as secrets management, authentication methods, policies, and secrets engines, use the HashiCorp Vault API and CLI documentation.

- [HashiCorp Vault API documentation](https://developer.hashicorp.com/vault/api-docs){: external}
- [Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external}

## Next steps
{: #vault-dedicated-apis-next-steps}

- Review the [HashiCorp Vault API documentation](https://developer.hashicorp.com/vault/api-docs){: external} for secrets management operations
- Learn about [Vault authentication methods](https://developer.hashicorp.com/vault/docs/auth){: external}
- Explore [Vault secrets engines](https://developer.hashicorp.com/vault/docs/secrets){: external}
