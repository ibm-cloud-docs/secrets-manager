---

copyright:
  years: 2026
lastupdated: "2026-08-31"

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

# Setting up your Vault Dedicated instance
{: #setting-up-vault-dedicated-instance}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager"}
{: toc-completion-time="10m"}

In this tutorial, you learn how to set up a `Vault Dedicated` plan instance of {{site.data.keyword.secrets-manager_full}} by provisioning the instance, retrieving its endpoints, generating an admin token, and signing in to the Vault UI for initial configuration.
{: shortdesc}

The `Vault Dedicated` plan delivers Vault Enterprise as a managed service in {{site.data.keyword.cloud_notm}}. After your instance is provisioned, you can use the instance dashboard to find connection details and open the Vault UI to begin configuring your Vault environment.

The Vault Dedicated plan is currently available as a public beta. Beta features are provided for evaluation and testing purposes and have limitations compared to generally available features.
{: beta}

## Vault Dedicated public beta limitations
{: #setting-up-vault-dedicated-beta-limitations}

During the public beta period, the [Vault Dedicated]{: tag-green} plan has the following temporary restrictions:

- **Instance limit**: Only 1 Vault Dedicated instance per account during beta.
- **No upgrade path**: Cannot upgrade from beta to GA. All beta instances will be deleted before general availability.
- **Regional availability**: Available in 2 regions (Dallas and Frankfurt).
- **Free during beta**: No charges apply during the beta period.
- **Beta to GA migration**: Data migration from beta instances to GA instances is not supported.

These limitations are temporary and apply only during the public beta period. It will be removed or modified when the [Vault Dedicated]{: tag-green} plan reaches general availability.
{: important}

## Before you begin
{: #setting-up-vault-dedicated-prereqs}

Before you begin, make sure that you have an {{site.data.keyword.cloud_notm}} account and the required IAM access to work with the service.

You need the following roles:
- The [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam) to provision an instance and generate an admin token.
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

   If you plan to use both `Standard` and `Vault Dedicated` plan instances, adopt a naming convention that makes the instance type easy to identify, such as including `-standard` or `-vault dedicated` in the instance name.
   {: tip}

5. Click **Create**.

Provisioning typically completes within a few minutes, but it can take up to 15 minutes.
{: note}

For more information about provisioning an instance, see [Creating an instance](/docs/secrets-manager?topic=secrets-manager-create-instance).

## Generate an admin token
{: #setting-up-vault-dedicated-admin-token}
{: step}

To access the Vault UI for initial configuration, generate an admin token from the instance dashboard.

1. In your {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Create token** in the **Create new admin token** section.
3. Copy the generated admin token and store it securely. You'll need this token to sign in to the Vault UI.

Admin tokens are intended for initial setup and emergency access only. Revoke them as soon as you complete your setup tasks.
{: important}

Alternatively, you can generate an admin token by calling the Broker API:

```bash
curl -X POST \
  "https://{region}.secrets-manager.cloud.ibm.com/api/v2/instances/{instance_id}/admintokens" \
  -H "Authorization: Bearer {iam_token}"
```
{: codeblock}

## Open the Vault UI
{: #setting-up-vault-dedicated-vault-ui}
{: step}

Use the Vault API endpoint to open the Vault UI and sign in with the admin token.

1. In the {{site.data.keyword.secrets-manager_short}} instance dashboard, click **Launch web UI** to access the Vault native UI.
2. On the Vault sign-in page, use the admin token that you created and complete your initial Vault configuration.

## Revoke the admin token
{: #setting-up-vault-dedicated-revoke-token}
{: step}

After you finish the initial setup, revoke any active admin tokens to help reduce the risk of unintended access.

1. In your {{site.data.keyword.secrets-manager_short}} instance dashboard, Click **Revoke** in the **Revoke all admin tokens** section.
3. Confirm the revocation when prompted.

Revoking the token immediately invalidates it and helps reduce the risk of unintended access.

Alternatively, you can revoke admin tokens by calling the Broker API:

```bash
curl -X DELETE \
  "https://{region}.secrets-manager.cloud.ibm.com/api/v2/instances/{instance_id}/admintokens" \
  -H "Authorization: Bearer {iam_token}"
```
{: codeblock}

## Next steps
{: #setting-up-vault-dedicated-next-steps}

After you sign in to Vault, you can continue with the initial configuration of your instance.

- To learn more about using the Vault UI and Vault workflows with the Vault Dedicated plan, see [By using the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-using-vault-dedicated).
- To configure the Vault CLI, see [Setting up the Vault CLI](/docs/secrets-manager?topic=secrets-manager-configure-vault-cli).
- To review Vault API capabilities, see [Vault API](/docs/secrets-manager?topic=secrets-manager-vault-api).
