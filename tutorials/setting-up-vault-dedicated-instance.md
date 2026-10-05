---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: Vault Dedicated, Vault as a Service, setting up vault dedicated, admin token, Vault UI, vault dedicated setup

subcollection: secrets-manager

content-type: tutorial
services: secrets-manager
account-plan: paid
completion-time: 10m

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

# Setting up your Vault Dedicated instance
{: #setting-up-vault-dedicated-instance}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager"}
{: toc-completion-time="10m"}

In this tutorial, you learn how to set up a `Vault Dedicated` plan instance of {{site.data.keyword.secrets-manager_full}} by provisioning the instance, retrieving its endpoints, and launching the Vault Web UI for initial configuration.
{: shortdesc}

The `Vault Dedicated` plan delivers Vault Enterprise as a managed service in {{site.data.keyword.cloud_notm}}. After your instance is provisioned, you can use the instance dashboard to find connection details and open the Vault UI to begin configuring your Vault environment.

The Vault Dedicated plan is currently available as a public beta. Beta features are provided for evaluation and testing purposes and have limitations compared to generally available features.
{: beta}

## Vault Dedicated public beta limitations
{: #setting-up-vault-dedicated-beta-limitations}

During the public beta period, the [Vault Dedicated]{: tag-green} plan has the following temporary restrictions:

- **Instance limit**: Only 1 Vault Dedicated instance per account during beta.
- **No upgrade path**: Cannot upgrade from beta to GA. All beta instances will be deleted before general availability.
- **Regional availability**: Available in Dallas and Frankfurt.
- **Free during beta**: No charges apply during the beta period.
- **Beta to GA migration**: Data migration from beta instances to GA instances is not supported.

These limitations are temporary and apply only during the public beta period. It will be removed or modified when the [Vault Dedicated]{: tag-green} plan reaches general availability.
{: important}

## Before you begin
{: #setting-up-vault-dedicated-prereqs}

Before you begin, make sure that you have an {{site.data.keyword.cloud_notm}} account and the required IAM access to work with the service.

You need the following roles:
- The [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to provision an instance and launch the Vault Web UI with an authenticated session.
- The [**Writer**, **Reader**, or **Viewer** service role](/docs/secrets-manager?topic=secrets-manager-iam) to view instance details.

## Provision a Vault Dedicated plan instance
{: #setting-up-vault-dedicated-provision}
{: step}

Create a `Vault Dedicated` plan instance from the {{site.data.keyword.cloud_notm}} catalog.

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Catalog**.
2. Select **Secrets Manager**.
3. Choose the **Vault Dedicated** plan.
4. Configure the instance:
   - Select a deployment region.
   - Enter a unique instance name.
   - Select a resource group.
   - Choose either service-managed encryption or customer-managed encryption by using Key Protect.
   - Select private-only endpoints, or enable a public endpoint if required.

   If you select **private-only** endpoints, note the following requirements before you try to connect:
   - **API access**: You must create a VPE (Virtual Private Endpoint) gateway that targets your Vault Dedicated instance. Cloud Service Endpoints (CSE) are not supported for this plan. See [Using service endpoints to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
   - **Vault UI access**: You must use a Client-to-Site VPN that routes traffic through the VPE. The private Vault UI URL is not reachable from a browser outside the {{site.data.keyword.cloud_notm}} private network without a VPN. See [Using Client-to-Site VPN to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-vpn-connection).
   {: important}

   If you plan to use both `Standard` and `Vault Dedicated` plan instances, adopt a naming convention that makes the instance type easy to identify, such as including `-standard` or `-vault dedicated` in the instance name.
   {: tip}

5. Click **Create**.

Provisioning typically completes within a few minutes, but it can take up to 15 minutes.
{: note}

For more information about provisioning an instance, see [Creating an instance](/docs/secrets-manager?topic=secrets-manager-create-instance).

## Open the Vault Web UI
{: #setting-up-vault-dedicated-vault-ui}
{: step}

Open the native Vault Web UI directly from the instance dashboard to begin your initial configuration. The experience depends on your IAM role.

1. In the {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Launch Vault Web UI**.

If you have the IAM Manager role, your browser redirects you directly to an authenticated Vault Web UI session. You can immediately begin configuring your Vault environment.

If you do not have the IAM Manager role, your browser redirects you to the native Vault Web UI login screen. Authenticate by using a Vault token or another authentication method that your Vault administrator has provisioned for you.

The Vault session is backed by a 1-hour, non-renewable admin token. The Vault Web UI automatically logs you out when the token expires. To start a new session, return to the instance dashboard and click **Launch Vault Web UI** again.
{: note}

### Generating an admin token for CLI or API access
{: #setting-up-vault-dedicated-admin-token-cli}
{: cli}

You can generate a plain admin token for direct API or CLI use, or a wrapped token for secure, single-use access to the Vault Web UI.

#### Plain admin token
{: #setting-up-vault-dedicated-plain-admin-token-cli}

To generate a plain Vault admin token, run the following command. The token is valid for an hour and is non-renewable. Store it securely and revoke it as soon as you no longer need it.

```sh
ibmcloud secrets-manager-instance-management admin-token-create --id {instance_id}
```
{: pre}

#### Wrapped admin token (for Vault Web UI login)
{: #setting-up-vault-dedicated-wrapped-admin-token-cli}

To open the Vault Web UI without exposing the real admin token, generate a wrapped token and navigate to the Vault UI URL in your browser.

```sh
ibmcloud secrets-manager-instance-management admin-token-create \
  --id {instance_id} \
  --response-wrapping true
```
{: pre}

Navigate to the following URL, replacing `{vault_ui_endpoint}` with the `vault_ui` value from your instance endpoints and `{wrapping_token}` with the value returned by the command:

```
https://{vault_ui_endpoint}/ui/vault/auth?wrapped_token={wrapping_token}
```
{: codeblock}

The Vault UI automatically unwraps the token and establishes an authenticated session. The wrapping token is single-use and expires after 30 seconds.

The Vault session is backed by a 1-hour, non-renewable admin token. The Vault Web UI automatically logs you out when the token expires. To start a new session, generate a new wrapped token and navigate to the URL again.
{: note}

### Generating an admin token for CLI or API access
{: #setting-up-vault-dedicated-admin-token-api}
{: api}

#### Plain admin token
{: #setting-up-vault-dedicated-plain-admin-token-api}

To generate a plain Vault admin token, omit the request body, send an empty object `{}`, or set `response_wrapping` to `false`. The token is valid for 1 hour and is non-renewable.

```sh
curl -X POST \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{}' \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

A successful request returns HTTP `201 Created`:

```json
{
  "token": "hvs.CAESIJ..."
}
```
{: codeblock}

#### Wrapped admin token (for Vault Web UI login)
{: #setting-up-vault-dedicated-wrapped-admin-token-api}

To open the Vault Web UI without exposing the real admin token, generate a wrapped token.

```sh
curl -X POST \
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"response_wrapping": true}' \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

A successful request returns HTTP `201 Created` with a `wrapped_token` field. Navigate to the following URL in your browser to open an authenticated Vault Web UI session:

```
https://{vault_ui_endpoint}/ui/vault/auth?wrapped_token={wrapped_token}
```
{: codeblock}

The wrapping token is single-use and expires after 30 seconds.

The Vault session is backed by a 1-hour, non-renewable admin token. The Vault Web UI automatically logs you out when the token expires. To start a new session, request a new wrapped token and navigate to the URL again.
{: note}

### Generating an admin token with Terraform
{: #setting-up-vault-dedicated-admin-token-terraform}
{: terraform}

To generate a Vault admin token with Terraform, use the `ibm_sm_admin_token` resource. The token is valid for 1 hour and is automatically refreshed when it is close to expiry. Use the token to authenticate directly to the Vault API.

```terraform
resource "ibm_sm_admin_token" "sm_admin_token" {
  instance_id = "bfc50c2e-d66d-4f37-9ccf-9713f8325b39"
}
```
{: codeblock}

After the resource is created, the Vault admin token is available in the `token` attribute.

## Revoke the admin token
{: #setting-up-vault-dedicated-revoke-token}
{: step}

After you finish the initial setup, revoke any active admin tokens to help reduce the risk of unintended access.

### Revoking admin tokens in the UI
{: #setting-up-vault-dedicated-revoke-token-ui}
{: ui}

1. In your {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Revoke** in the **Revoke all admin tokens** section.
2. Confirm the revocation when prompted.

Revoking the token immediately invalidates it and helps reduce the risk of unintended access.

### Revoking admin tokens from the CLI
{: #setting-up-vault-dedicated-revoke-token-cli}
{: cli}

To revoke all active Vault admin tokens by using the {{site.data.keyword.cloud_notm}} CLI, run the following command.

```sh
ibmcloud secrets-manager-instance-management admin-tokens-delete --id {instance_id}
```
{: pre}

This operation immediately invalidates all admin tokens, requiring new tokens to be generated for future administrative access.

### Revoking admin tokens with the API
{: #setting-up-vault-dedicated-revoke-token-api}
{: api}

```sh
curl -X DELETE \
  -H "Authorization: Bearer {iam_token}" \
  "https://{region}.secrets-manager.cloud.ibm.com/v2/instances/{id}/admintokens"
```
{: codeblock}

A successful revocation returns a `204 No Content` status code.

## Next steps
{: #setting-up-vault-dedicated-next-steps}

After you sign in to Vault, you can continue with the initial configuration of your instance.

- To configure authentication methods for your teams and applications, see [Configuring authentication methods](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-auth-methods).
- To integrate with your apps by using the Vault API, CLI, or SDKs, see [Integrating with your apps](/docs/secrets-manager?topic=secrets-manager-integrate-with-apps-vault-dedicated).
- To review Vault API and CLI capabilities, see [API and CLI reference overview](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-reference-overview).
