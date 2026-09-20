---

copyright:
  years: 2026
lastupdated: "2026-09-20"

keywords: known issues for {{site.data.keyword.secrets-manager_short}}, known limitations for {{site.data.keyword.secrets-manager_short}}, vault dedicated plan

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

# Known issues and limits for Vault Dedicated plan
{: #known-issues-and-limits-vault-dedicated}

{{site.data.keyword.secrets-manager_full}} includes the following known issues and limits that might impact your experience.
{: shortdesc}

## Known issues
{: #issues-and-limitations}

Review the following known issues that you might encounter as you use {{site.data.keyword.secrets-manager_short}}.

| Issue | Workaround |
| --- | --- |
| Storage-path encryption in transit between Vault pods and the mounted File Storage for VPC path is not enabled in initial GA. | The residual risk is mitigated through Vault encryption of persisted data, managed auto-unseal with IBM Cloud Key Protect, private managed infrastructure boundaries, workload isolation controls, and service-managed operational controls. |
| Customer control to enable or disable public endpoint access after provisioning is not supported in initial GA. | Determine the required endpoint posture at provisioning time. To change the endpoint posture, you must provision a new instance with the required configuration. |
| Cross-region failover and replication-based recovery are not part of initial GA. | Design workloads with awareness that cross-region failover is not available. When more than one availability zone becomes unavailable, the region is considered to be in a disaster state. |
| Vault data-plane operations do not inherit IBM Cloud IAM Context-Based Restrictions automatically. | Use Vault-native policies and auth method configuration to apply access restrictions to Vault data-plane operations. |
| Admin tokens are not retained by the service after generation. If an admin token is lost before use, it cannot be recovered. | Generate a new admin token using the control-plane API or UI. Revoke all active admin tokens before generating a new one if required. |
| Destination provisioning is asynchronous and may remain in a pending or failed state if managed network artifacts cannot be successfully created or validated. | Monitor the destination lifecycle state through the control-plane API or UI. Delete and recreate the destination resource if it remains in a failed state. |
| A configured destination enables bounded network reachability only and does not validate customer Vault plugin configuration or remote service authorization correctness. | Validate Vault plugin configuration and remote service authorization independently after destination activation. |
| Destination resources are intended to be immutable after creation except for limited metadata updates. Changes to the connectivity target require creating a new destination resource and deleting the old one. | Create a new destination resource with the updated target and delete the previous one. |
| Activity tracking events from the Vault root namespace are not visible to customers. Only events from customer-managed namespaces are forwarded to IBM Cloud Logs. Root-namespace events are delivered exclusively to the service-managed audit log. | To audit all activity in your namespaces, query IBM Cloud Logs with `host:secrets-manager` scoped to your instance CRN. Root-namespace service operations are not included in customer event streams by design. |
| The `reason.reasonType` field in Vault Dedicated activity tracking failure events is emitted as `unauthorized` (all lowercase). IBM Cloud Logs queries that use `reason.reasonType:Unauthorized` (capital U) return no results. | Use `reason.reasonType:unauthorized` (all lowercase) when writing alert rules or queries for unauthorized access attempts. |
| Vault Dedicated activity tracking failure events include a second failure type with `reasonCode: 400` and `reasonType: bad request` for malformed or invalid requests. This failure type is not documented in the activity tracking events reference for the Vault Dedicated plan. | When writing alert rules that cover all failure types, include both `reason.reasonCode:401` and `reason.reasonCode:400` in your query to capture both unauthorized and bad-request failures. |
{: caption="Known issues and limitations that apply to the Vault Dedicated plan" caption-side="bottom"}

## Known limitations
{: #manage-endpoint-access-limitations}

Review the following limitations before managing public endpoint access for your instance.

### PKI secrets engine: CRL distribution points in issued certificates
{: #limitation-crl-urls}

When a PKI secrets engine certificate authority issues certificates, it embeds certificate revocation list (CRL) distribution point URLs directly into each certificate. These URLs are determined by the instance endpoint at the time that the certificate authority is configured, and they cannot be changed.

If your instance was originally configured with the public endpoint enabled, certificates issued by that certificate authority contain CRL URLs that reference the public endpoint. If you later disable the public endpoint, those CRL URLs become unreachable, which might cause revocation checks to fail for clients that strictly enforce CRL validation.

Customer control to enable or disable public endpoint access after provisioning is not supported in initial GA. If you require a specific endpoint posture, determine it before configuring any PKI certificate authorities.
{: note}

**Recommendation**: If you plan to disable the public endpoint, reconfigure or recreate the PKI certificate authority after you disable the public endpoint, and then reissue any certificates that were signed by the previous certificate authority. Clients that do not enforce CRL validation are not affected.

## Limits
{: #limits}

Consider the following service limits as you use {{site.data.keyword.secrets-manager_short}}.

### Account limits
{: #general-limits}

The following limits apply per {{site.data.keyword.cloud_notm}} account.

| Resource | Limit |
| --- | --- |
| Vault Dedicated service instances | No limit on the number of instances per account |
{: caption="Vault Dedicated limits per account" caption-side="bottom"}

### Instance limits
{: #instance-limits}

The following limits apply to Vault Dedicated service instances.

| Resource | Limit |
| --- | --- |
| Vault cluster nodes | 3 nodes per instance (multi-zone, service-managed) |
| Admin token TTL | 1 hour maximum |
| Vault namespaces | Limited to customer-managed administrative scope. Root namespace access is reserved for service operations. |
| Outbound destinations | Subject to per-instance quota and rate limiting. Only explicitly approved destination types are supported in initial GA. |
| Supported auth methods | Token, AppRole, Userpass. JWT with static local verification material is supported with limitations. |
| Supported secrets engines | KV v2, Transit, PKI (self-contained), Transform, TOTP, and related core Vault workflows. |
| Customer-provided Vault Enterprise licenses (BYOL) | Not supported in initial GA. |
| Customer-managed native audit devices | Not supported in initial GA. Audit device lifecycle is service-managed. |
| External plugin installation | Not supported in initial GA. Plugin lifecycle is service-managed. |
| Customer-controlled backup and restore | Not supported in initial GA. Backup lifecycle is service-managed. |
| Customer-controlled scaling or upgrade scheduling | Not supported in initial GA. Cluster topology and Vault version lifecycle are service-managed. |
| Root namespace access | Not available to customers. Reserved for service operations. |
| General-purpose outbound connectivity | Not supported. Outbound connectivity is blocked by default and enabled only for explicitly approved destination types. |
{: caption="Vault Dedicated limits per instance" caption-side="bottom"}

### Service-managed controls
{: #service-managed-limits}

The following capabilities remain under service control and are not customer-operated in initial GA.

| Capability | Notes |
| --- | --- |
| Seal and unseal operations | Service-managed using IBM Cloud Key Protect for managed auto-unseal. |
| Root token management | Root tokens are not exposed as persistent customer credentials. |
| Root namespace access | Reserved for service operations. |
| Integrated storage management | Vault Raft storage is service-managed. |
| Vault version lifecycle | Vault software upgrades are service-managed using a rolling upgrade strategy. |
| Vault licensing and metering | License procurement, installation, renewal, and compliance are service-managed. |
| Backup lifecycle | Backup handling is service-managed. |
| Audit device lifecycle | Native Vault audit device enablement and configuration are service-managed. |
| Plugin lifecycle | Plugin installation and management are service-managed. |
| Recovery key handling | Recovery material is handled by the service as part of provisioning and recovery operations. |
| Cluster topology management | Node count, zone distribution, and cluster infrastructure are service-managed. |
{: caption="Service-managed controls for Vault Dedicated" caption-side="bottom"}
