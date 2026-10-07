---

copyright:
  years: 2026
lastupdated: "2026-10-07"

keywords: Secrets Manager, Vault Dedicated, API, admin tokens, instance details, wrapped token

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

Use the {{site.data.keyword.secrets-manager_full}} instance management API to manage service instances of the `Vault Dedicated` plan. For Vault runtime operations such as secrets management, authentication methods, and policies, use the HashiCorp Vault API.
{: shortdesc}

For the interactive API reference with SDK examples, see the [Instance Management API reference](https://{DomainName}/apidocs/secrets-manager/secrets-manager-instance-management-v2){: external}.
{: tip}

## {{site.data.keyword.secrets-manager_short}} Instance Management API
{: #ibm-cloud-instance-management-api}

The {{site.data.keyword.secrets-manager_short}} instance management API provides control plane operations for managing your Vault Dedicated service instances. These APIs allow you to retrieve instance metadata, manage admin tokens, and configure instance settings.

### Authentication
{: #api-authentication}

All API requests require authentication using an {{site.data.keyword.cloud_notm}} IAM token. Include your IAM token in the `Authorization` header of each request:

```sh
Authorization: Bearer {iam_token}
```
{: codeblock}

For information on generating IAM tokens, see [Creating an IAM access token for a user or service ID](/docs/iam?topic=iam-iamtoken_from_apikey).

### Base URL
{: #api-base-url}

The base URL for the Instance Management API is the control plane service endpoint for your instance. You can find this endpoint in the **Endpoints** page of your Secrets Manager service dashboard.

```
https://{region}.secrets-manager.cloud.ibm.com
```
{: codeblock}

Replace `{region}` with the region where your instance is deployed (for example, `us-south`, `eu-de`).

### Getting instance details from the CLI
{: #get-instance-details-cli}
{: cli}

To retrieve the details of your Vault Dedicated instance by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud secrets-manager-instance-management instance-details --id {instance_id}
```
{: pre}

### Getting instance details with the API
{: #get-instance-details-api}
{: api}

Retrieve detailed information about your Vault Dedicated instance.

#### Request
{: #get-instance-request}

```sh
GET /v2/instances/{id}
```
{: codeblock}

#### Example request
{: #get-instance-example-request}

```sh
curl -X GET \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}"
```
{: codeblock}

#### Response
{: #get-instance-response}

The response includes the following information:

- **id**: The instance ID (UUID)
- **name**: The instance name
- **instance_crn**: The instance CRN identifier
- **plan**: The instance plan name (`dedicated`)
- **vault_cluster**: Vault cluster information, including `status` (`healthy`, `sealed`, or `not_initialized`) and `version`
- **endpoints**: Public and private endpoint URLs, each containing `vault_api` and `vault_ui` fields
- **encryption**: Key management service configuration, including `mode` (`service_managed` or `customer_managed`), and optionally `provider` and `key_crn` for customer-managed encryption

#### Example response
{: #get-instance-example-response}

```json
{
  "id": "bfc50c2e-d66d-4f37-9ccf-9713f8325b39",
  "name": "my-vault-dedicated-instance",
  "instance_crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/...:bfc50c2e-d66d-4f37-9ccf-9713f8325b39::",
  "plan": "dedicated",
  "vault_cluster": {
    "status": "healthy",
    "version": "2.0.4"
  },
  "endpoints": {
    "public": {
      "vault_api": "https://bfc50c2e-d66d-4f37-9ccf-9713f8325b39.us-south.secrets-manager.appdomain.cloud",
      "vault_ui": "https://bfc50c2e-d66d-4f37-9ccf-9713f8325b39.us-south.secrets-manager.appdomain.cloud/ui"
    },
    "private": {
      "vault_api": "https://private.bfc50c2e-d66d-4f37-9ccf-9713f8325b39.us-south.secrets-manager.appdomain.cloud",
      "vault_ui": "https://private.bfc50c2e-d66d-4f37-9ccf-9713f8325b39.us-south.secrets-manager.appdomain.cloud/ui"
    }
  },
  "encryption": {
    "mode": "service_managed"
  },
  "href": "https://us-south.secrets-manager.cloud.ibm.com/v2/instances/bfc50c2e-d66d-4f37-9ccf-9713f8325b39"
}
```
{: codeblock}

### Getting instance details with Terraform
{: #instance-details-terraform}
{: terraform}

To get the details of a Vault Dedicated instance with Terraform, use the `ibm_sm_instance` data source.

```terraform
data "ibm_sm_instance" "sm_instance" {
  instance_id = "bfc50c2e-d66d-4f37-9ccf-9713f8325b39"
}
```
{: codeblock}

After your data source is created, you can reference its attributes. For example, to get the public Vault API endpoint:

```
data.ibm_sm_instance.sm_instance.endpoints.0.public.0.vault_api
```
{: codeblock}

### Generating an admin token from the CLI
{: #generate-admin-token-cli}
{: cli}

#### Plain admin token
{: #generate-plain-admin-token-cli}

Generate a plain Vault admin token by using the {{site.data.keyword.cloud_notm}} CLI.

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id {instance_id}
```
{: pre}

The command returns the Vault admin token. The token is valid for an hour and is non-renewable. Store it securely and revoke it as soon as you no longer need it.

#### Wrapped admin token (for Vault Web UI login)
{: #generate-wrapped-admin-token-cli}

Generate a response-wrapped token. For example, to open the Vault Web UI from a script or automated workflow without exposing the real admin token, pass the `--response-wrapping` flag.

```sh
ibmcloud secrets-manager-instance-management admin-token-create \
  --id {instance_id} \
  --response-wrapping true
```
{: pre}

The command returns a short-lived wrapping token (valid for 30 seconds) instead of the real admin token. To open the Vault Web UI with this token, navigate to the following URL in your browser:

```
https://{vault_ui_endpoint}/ui/vault/auth?wrapped_token={wrapping_token}
```
{: codeblock}

Replace `{vault_ui_endpoint}` with the `vault_ui` value from your instance endpoints and `{wrapping_token}` with the value returned by the command. The Vault UI automatically unwraps the token and establishes an authenticated session. The wrapping token is single-use and expires after 30 seconds.

The Vault session is backed by a 1-hour, non-renewable admin token. The Vault Web UI automatically logs you out when this token expires. To start a new session, generate a new wrapped token and navigate to the URL again.
{: note}

### Generating an admin token with the API
{: #generate-admin-token-api}
{: api}

The `POST /v2/instances/{id}/admintokens` endpoint supports two modes controlled by the optional `response_wrapping` field in the request body.

#### Request
{: #generate-token-request}

```sh
POST /v2/instances/{id}/admintokens
```
{: codeblock}

#### Plain admin token
{: #generate-plain-admin-token-api}

Omit the request body, send an empty object `{}`, or set `response_wrapping` to `false` to receive the plain admin token. The token is valid for an hour and is non-renewable.

**Example request**

```sh
curl -X POST \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{}' \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

**Example response**

A successful request returns HTTP `201 Created`:

```json
{
  "token": "hvs.CAESIJ..."
}
```
{: codeblock}

#### Wrapped admin token (for Vault Web UI login)
{: #generate-wrapped-admin-token-api}

Set `response_wrapping` to `true` to receive a response-wrapped token instead of the plain admin token. The wrapping token is single-use and expires after 30 seconds.

**Example request**

```sh
curl -X POST \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"response_wrapping": true}' \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

**Example response**

A successful request returns HTTP `201 Created`:

```json
{
  "wrapped_token": "hvs.yyy..."
}
```
{: codeblock}

To open the Vault Web UI with this token, navigate to the following URL:

```
https://{vault_ui_endpoint}/ui/vault/auth?wrapped_token={wrapped_token}
```
{: codeblock}

The Vault UI automatically calls `POST /v1/sys/wrapping/unwrap`, exchanges the wrapping token for the real admin token, and establishes an authenticated session. The wrapping token is consumed on first use.

The Vault session is backed by a 1-hour, non-renewable admin token. The Vault Web UI automatically logs you out when this token expires. To start a new session, request a new wrapped token and navigate to the URL again.
{: note}

### Generating an admin token with Terraform
{: #generate-admin-token-terraform}
{: terraform}

To generate a Vault admin token with Terraform, use the `ibm_sm_admin_token` resource. The token is valid for 1 hour, and is automatically refreshed when it is close to expiry.

```terraform
resource "ibm_sm_admin_token" "sm_admin_token" {
  instance_id = "bfc50c2e-d66d-4f37-9ccf-9713f8325b39"
}
```
{: codeblock}

After the resource is created, the token is available in the `token` attribute.

**Using the admin token**

Use the `vault_api` endpoint from the instance details response to authenticate Vault API calls:

```sh
curl -X GET \
  -H "X-Vault-Token: hvs.CAESIJ..." \
  "{vault_api_endpoint}/v1/sys/health"
```
{: codeblock}

### Revoking all admin tokens from the CLI
{: #revoke-admin-tokens-cli}
{: cli}

To revoke all active Vault admin tokens by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id {instance_id}
```
{: pre}

This operation immediately invalidates all admin tokens, requiring new tokens to be generated for future administrative access.

### Revoking all admin tokens with the API
{: #revoke-admin-tokens-api}
{: api}

Revoke all active Vault admin tokens for your instance. This operation immediately invalidates all admin tokens, requiring new tokens to be generated for future administrative access.

#### Request
{: #revoke-tokens-request}

```sh
DELETE /v2/instances/{id}/admintokens
```
{: codeblock}

#### Example request
{: #revoke-tokens-example-request}

```sh
curl -X DELETE \
  -H "Authorization: Bearer {iam_token}" \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

#### Response
{: #revoke-tokens-response}

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
