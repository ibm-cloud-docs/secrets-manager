---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: Secrets Manager availability, regions, Secrets Manager endpoints, endpoint access

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

# Managing public endpoint access for Secrets Manager
{: #manage-endpoint-access}

You can enable or disable the public internet endpoint of your {{site.data.keyword.secrets-manager_short}} instance at any time. When the public endpoint is disabled, your instance is accessible only through the private network endpoint. You can re-enable the public endpoint whenever needed.
{: shortdesc}

## Before you begin
{: #manage-endpoint-access-prereqs}

Before you get started, make sure that you meet the following requirements:

- You must have the **Administrator**, **Editor**, or **Operator** platform role on the {{site.data.keyword.secrets-manager_short}} instance.
- The instance must exist. Endpoint access can be changed on existing instances only.

## Disabling the public endpoint
{: #disable-public-endpoint}

When the public endpoint is disabled, your instance is accessible only through the private network endpoint:

- For the [Trial and Standard]{: tag-blue} plans, you can connect by using Virtual Private Endpoint (VPE) gateways or through Cloud Service Endpoints (CSE). For more information, see [Securing your connection to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
- For the [Vault Dedicated]{: tag-green} plan, **only VPE gateways are supported** — CSE is not available. API access requires a VPE gateway, and accessing the Vault UI from a browser requires a Client-to-Site VPN that routes traffic through the VPE. For more information, see [Service endpoints for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-endpoints#vault-dedicated-service-endpoints) and [Using Client-to-Site VPN to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-vpn-connection).

### Disabling the public endpoints by using the console
{: #disable-public-endpoint-ui}
{: ui}

Use the {{site.data.keyword.cloud_notm}} console to disable the public endpoint for your instance.

1. In the {{site.data.keyword.cloud_notm}} console, navigate to the **Resource list** and select your {{site.data.keyword.secrets-manager_short}} instance.
2. Locate the endpoint controls based on your plan:
   - For the [Trial and Standard]{: tag-blue} plans: **From the Resource list**, open an instance and click **Endpoints** from the navigation menu. The private endpoint and public endpoint access controls are displayed. For more information on navigating your instance, see [Getting started with {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started).
   - For the [Vault Dedicated]{: tag-green} plan: Click **Overview** and go to the **Endpoints** section. For more information on navigating your instance dashboard, see [Setting up your Vault Dedicated instance](/docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance).
3. In the **Public endpoint access** section, click **Disable public endpoint**.
4. Wait for the operation to complete. A confirmation message is displayed when the public endpoint is successfully disabled.

After the operation completes, the public endpoint URL is no longer accessible. Your instance can still be reached through the private endpoint.

### Disabling the public endpoint by using the CLI
{: #disable-public-endpoint-cli}
{: cli}

Run the following command, replacing `<INSTANCE_NAME_OR_ID>` with the name or ID of your {{site.data.keyword.secrets-manager_short}} instance.

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME_OR_ID> \
  --service-endpoints private-only
```
{: pre}

You can update both the pricing plan and endpoint access in a single CLI command by passing the `-p` or `--plan-id` option alongside `--service-endpoints`. For more information on plan options, see [Secrets Manager plans and features](/docs/secrets-manager?topic=secrets-manager-feature-overview).
{: tip}

The update is asynchronous. To check the status of the operation, run:

```sh
ibmcloud resource service-instance <INSTANCE_NAME_OR_ID>
```
{: pre}

The **State** field shows `active` for all instances after provisioning and does not indicate whether the update is complete. Check the **Last Operation** section instead.

While the update is in progress, the output looks similar to:

```
...
State:                  active
Last Operation:
                        Status    update in progress
                        Message   Configuring endpoint and plan 66%
```
{: codeblock}

When the operation is complete, the **Last Operation** status changes to `update succeeded`:

```
...
State:                  active
Last Operation:
                        Status    update succeeded
                        Message   Configuring endpoint and plan
```
{: codeblock}

### Disabling the public endpoints by using the API
{: #disable-public-endpoint-api}
{: api}

Send a `PATCH` request to the Resource Controller API.

```sh
curl -X PATCH \
  "https://resource-controller.cloud.ibm.com/v2/resource_instances/<INSTANCE_ID>" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "parameters": {
      "service-endpoints": "private-only"
    }
  }'
```
{: codeblock}

You can also include `resource_plan_id` in the request body to update your pricing plan and endpoint access simultaneously.
{: tip}

## Enabling the public endpoint
{: #enable-public-endpoint}

You can re-enable the public endpoint at any time to restore public internet access to your {{site.data.keyword.secrets-manager_short}} instance.

### Enabling the public endpoints by using the console
{: #enable-public-endpoint-ui}
{: ui}

Use the {{site.data.keyword.cloud_notm}} console to enable the public endpoint for your instance.

1. In the {{site.data.keyword.cloud_notm}} console, navigate to the **Resource list** and select your {{site.data.keyword.secrets-manager_short}} instance.
2. Locate the endpoint controls based on your plan:
   - For the [Trial and Standard]{: tag-blue} plans: **From the Resource list**, open an instance and click **Endpoints** from the navigation menu. For more information on navigating your instance, see [Getting started with {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started).
   - For the [Vault Dedicated]{: tag-green} plan: Click **Overview** and go to the **Endpoints** section. For more information on navigating your instance dashboard, see [Setting up your Vault Dedicated instance](/docs/secrets-manager?topic=secrets-manager-setting-up-vault-dedicated-instance).
3. In the **Public endpoint access** section, click **Enable public endpoint**.
4. Wait for the operation to complete. A confirmation message is displayed when the public endpoint is successfully enabled.

After you enable a public endpoint, DNS changes can take up to 30 minutes to propagate. During this time, attempts to access the endpoint might fail with a host resolution error, which resolves automatically once propagation is complete.
{: note}

### Enabling the public endpoints by using the CLI
{: #enable-public-endpoint-cli}
{: cli}

Run the following command, replacing `<INSTANCE_NAME_OR_ID>` with the name or ID of your {{site.data.keyword.secrets-manager_short}} instance.

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME_OR_ID> \
  --service-endpoints public-and-private
```
{: pre}

After you enable a public endpoint, DNS changes can take up to 30 minutes to propagate. During this time, attempts to access the endpoint might fail with a host resolution error, which resolves automatically once propagation is complete.
{: note}

### Enabling the public endpoints by using the API
{: #enable-public-endpoint-api}
{: api}

Send a `PATCH` request to the Resource Controller API.

```sh
curl -X PATCH \
  "https://resource-controller.cloud.ibm.com/v2/resource_instances/<INSTANCE_ID>" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "parameters": {
      "service-endpoints": "public-and-private"
    }
  }'
```
{: codeblock}

After you enable a public endpoint, DNS changes can take up to 30 minutes to propagate. During this time, attempts to access the endpoint might fail with a host resolution error, which resolves automatically once propagation is complete.
{: note}

## Supported endpoint access values
{: #service-endpoints-values}

The CLI and API commands for managing endpoint access are uniform across all plans (`Trial`, `Standard`, and `Vault Dedicated`). The following table lists the accepted values for the `--service-endpoints` parameter and their effect on endpoint access.

| Value | Result |
|-------|--------|
| `private-only` | Disables the public endpoint. Private access only. |
| `public-and-private` | Enables both public and private endpoints. |
{: caption="Accepted values for --service-endpoints" caption-side="bottom"}

The private endpoint is always active and cannot be disabled.
{: note}

## Connecting to a private-only instance
{: #connect-private-only}

When the public endpoint is disabled, you must connect to your instance through the {{site.data.keyword.cloud_notm}} private network. For detailed instructions on how to set up private connectivity for your instance, see the following topics:

- [Using service endpoints to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection) for connecting from within an {{site.data.keyword.cloud_notm}} VPC using Virtual Private Endpoint (VPE) gateways or Cloud Service Endpoints (CSE).
- [Using Client-to-Site VPN to privately connect to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-vpn-connection) for connecting from a local client workstation or accessing the Vault UI of a private-only Vault-Dedicated instance.

## What to expect
{: #manage-endpoint-access-behavior}

When you update the public endpoint configuration of your instance, keep in mind the following behaviors:

- The operation is asynchronous and typically completes within a few minutes.
- While the operation is in progress, the instance remains available through its existing endpoints.
- Existing secrets and configurations are not affected by changing the endpoint access.
- Activity Tracker records an event every time the public endpoint is enabled or disabled.

## Known limitations
{: #manage-endpoint-access-limitations}

Review the following limitations before managing public endpoint access for your instance.

### Private Certificate Engine: AIA URLs in issued certificates
{: #limitation-aia-urls}

When you configure the Private Certificate Engine, {{site.data.keyword.secrets-manager_short}} embeds Authority Information Access (AIA) URLs directly into every leaf certificate it issues. These URLs point to the locations where clients can download the CA chain and Certificate Revocation Lists (CRLs) to validate and check the revocation status of certificates.

If your instance was originally provisioned with the public endpoint enabled, the AIA URLs embedded in already-issued certificates reference the public endpoint. If you later disable the public endpoint, those URLs become unreachable, which can cause:

- Certificate path validation failures in clients that strictly follow AIA extensions.
- CRL and OCSP check to fail, which might result in revocation-checking errors, depending on the client's policy (fail-open versus fail-closed).

To avoid these issues, follow these recommendations:

- If you plan to disable the public endpoint, configure the Private Certificate Engine with AIA URLs that reference the private endpoint before issuing any certificates.
- Certificates issued before the AIA configuration is updated to use the private endpoint URL must be reissued.
- Clients that do not strictly enforce CRL or OCSP validation are not affected.

### Event notification URL fields
{: #limitation-event-notifications}

Event notifications sent by {{site.data.keyword.secrets-manager_short}} include two URL fields: `source_instance_api_private_url` and `source_instance_api_public_url`.

| Field | Description |
|-------|-------------|
| `source_instance_api_private_url` | The private endpoint URL of the instance. |
| `source_instance_api_public_url` | Intended to be the public endpoint URL; however, this field currently always returns the private endpoint URL regardless of whether the public endpoint is enabled or disabled. |
{: caption="Event notification URL fields" caption-side="bottom"}

Due to a known limitation, the `source_instance_api_public_url` field is always present in event notification payloads. If your instance has the public endpoint that is disabled, do not rely on this field to reach your instance; use `source_instance_api_private_url` instead.
{: note}

## Next steps
{: #manage-endpoint-next-steps}

- To learn more about the available endpoint options and how to secure your connection, see [Securing your connection to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
- To learn more about managing service endpoints and private connectivity, see [Changing service endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints#changing-service-endpoints-cli) and [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).
- To connect directly from your browser without another network setup, use {{site.data.keyword.cloud_notm}} Shell, which runs inside the private network. For more information, see [Getting started with {{site.data.keyword.cloud_notm}} Shell](/docs/cloud-shell?topic=cloud-shell-getting-started).
