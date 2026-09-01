---

copyright:
  years: 2026
lastupdated: "2026-09-01"

keywords: monitoring, metrics, operational metrics

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

# Monitoring operational metrics
{: #operational-metrics}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.mon_full_notm}} service to measure how users and applications interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

{{site.data.keyword.mon_full_notm}} records data on the operations that occur inside of {{site.data.keyword.cloud_notm}}. This service allows you to gain operational visibility into the performance and health of your applications, services, and platforms. You can use its advanced features to monitor and troubleshoot, define alerts based on API response codes, and design custom dashboards.

For more information regarding the {{site.data.keyword.mon_short}} service, check out [Getting started](/docs/monitoring?topic=monitoring-getting-started){: external}.

Enabling {{site.data.keyword.secrets-manager_short}} service metrics adds new metrics to your {{site.data.keyword.mon_short}} instance. For information on {{site.data.keyword.mon_short}} pricing, check out [Pricing](/docs/monitoring?topic=monitoring-pricing_plans){: external}.
{: tip}

## What metrics are available?
{: #metrics-available}

The metrics available depend on which plan your {{site.data.keyword.secrets-manager_short}} instance uses.

For the [Trial and Standard]{: tag-blue} plans, you can track the type of API requests being made in your service instance as well as the latency of the requests. The dashboard includes:

- Total requests being made in your {{site.data.keyword.secrets-manager_short}} instance, categorized by API type.
- Failed API requests categorized by error type.
- API request latency over time, including the average latency, highest latency, and lowest latency.
- Total amount of secrets and secret groups in the instance.

For the [Vault Dedicated]{: tag-green} plan, you can track a broader set of operational metrics that reflect the internals of your Vault Enterprise cluster. The dashboard includes:

- Token operations, including creation, lookup, and revocation counts and latency.
- Lease and expiration management, including active lease counts, renewals, and expiration processing.
- Secret engine route operations across all mounts, including create, read, update, and delete activity.
- Policy operations, including get, set, list, and delete actions on access control policies.
- Identity and entity management, including active entity counts and alias tracking.
- KV secrets count and dynamic secret lease creation.
- Database secrets engine activity, including credential creation, renewal, and revocation.

## Before you begin
{: #operational-metrics-considerations}

### Configure a {{site.data.keyword.mon_short}} instance for metrics
{: #configure-monitor}

Other {{site.data.keyword.cloud_notm}} users with `administrator` or `editor` permissions can manage the {{site.data.keyword.mon_short}} service in the {{site.data.keyword.cloud_notm}}. These users must also have platform permissions to create resources within the context of the resource group where they plan to provision the instance.
{: important}

To enable platform metrics in a region, complete the following steps:

1. [Provision an instance of {{site.data.keyword.mon_short}}](/docs/monitoring?topic=monitoring-provision){: external} in the region of the {{site.data.keyword.secrets-manager_short}} instance.

2. Go to the [Monitoring](/observe/monitoring) dashboard.

3. Click **Configure platform metrics.**

4. Select the region where the {{site.data.keyword.secrets-manager_short}} instance was created.

5. Select the {{site.data.keyword.mon_short}} instance in which you would like to receive metrics.

6. Click **Configure.**

## Trial and Standard plan metrics
{: #sm-metrics-trial-standard}

For the [Trial and Standard]{: tag-blue} plans, you can use the metrics in your monitoring instance dashboard to measure the types of requests being made in your service instance as well as the latency of the requests. All metrics use metric type **Gauge**.

### Resource count
{: #resource-count}

The total amount of secrets and secret groups in the instance.

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|`ibm_sm_secrets_count`|Total amount of secrets|Gauge|None|
|`ibm_sm_secret_groups_count`|Total amount of secret groups|Gauge|None|
{: caption="Describes the API Hits metrics." caption-side="bottom"}

### Total requests
{: #total-requests}

The type and amount of API requests being made to your {{site.data.keyword.secrets-manager_short}} instance. For example, you can track how many API requests have been made for read, write, or delete actions.

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|`ibm_sm_delete_private_requests_count`|Total amount of delete requests in private network|Gauge|None|
|`ibm_sm_delete_public_requests_count`|Total amount of delete requests in public network|Gauge|None|
|`ibm_sm_read_private_requests_count`|Total amount of read requests in private network|Gauge|None|
|`ibm_sm_read_public_requests_count`|Total amount of read requests in public network|Gauge|None|
|`ibm_sm_write_private_requests_count`|Total amount of write requests in private network|Gauge|None|
|`ibm_sm_write_public_requests_count`|Total amount of write requests in public network|Gauge|None|
{: caption="Describes the API Hits metrics." caption-side="bottom"}

### Error count
{: #error-count}

This metric gathers the number of `4xx` and `5xx` errors encountered from all APIs.

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|`ibm_sm_4xx_errors_count`|Total amount of 4xx errors|Gauge|None|
|`ibm_sm_5xx_errors_count`|Total amount of 5xx errors|Gauge|None|
{: caption="Describes the API Hits metrics." caption-side="bottom"}

## Latency
{: #latency}

This metric tracks amount of time it takes {{site.data.keyword.secrets-manager_short}} to receive an API request and respond to it.

The latency is calculated by getting the average of all requests of the same type that occur within 60 seconds.
{: note}

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|`ibm_sm_latency_delete_avg_ms`|Delete operation average response time|Gauge|Milliseconds|
|`ibm_sm_latency_delete_max_ms`|Delete operation maximum response time|Gauge|Milliseconds|
|`ibm_sm_latency_delete_min_ms`|Delete operation minimum response time|Gauge|Milliseconds|
|`ibm_sm_latency_read_avg_ms`|Read operation average response time|Gauge|Milliseconds|
|`ibm_sm_latency_read_max_ms`|Read operation maximum response time|Gauge|Milliseconds|
|`ibm_sm_latency_read_min_ms`|Read operation minimum response time|Gauge|Milliseconds|
|`ibm_sm_latency_write_avg_ms`|Write operation average response time|Gauge|Milliseconds|
|`ibm_sm_latency_write_max_ms`|Write operation maximum response time|Gauge|Milliseconds|
|`ibm_sm_latency_write_min_ms`|Write operation minimum response time|Gauge|Milliseconds|
{: caption="Describes the Latency metrics." caption-side="bottom"}

## Vault Dedicated plan metrics
{: #vault-dedicated-sm-metrics}

All [Vault Dedicated]{: tag-green} plan metrics use metric type **Gauge** and are emitted every 60 seconds. The metrics are organized into the following categories: 
- Core request handling
- Secret engine route operations
- Token operations
- Lease and expiration management
- Policy operations
- Identity and entity management
- KV secrets
- Database secrets engine activity

### Core request handling
{: #vault-dedicated-core-requests}

These metrics track the volume and latency of requests processed by the Vault core, including general requests, login requests, token checks, and ACL fetch operations.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_core_handle_request_count` | Total number of requests handled by Vault core | Gauge | None |
| `ibm_sm_vault_dedicated_core_handle_request_avg_ms` | Average latency of request handling by Vault core in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_core_handle_login_request_count` | Total number of login requests handled | Gauge | None |
| `ibm_sm_vault_dedicated_core_handle_login_request_avg_ms` | Average latency of login request handling in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_core_check_token_count` | Total number of token validation operations | Gauge | None |
| `ibm_sm_vault_dedicated_core_check_token_avg_ms` | Average latency of token validation operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_core_fetch_acl_and_token_count` | Total number of ACL and token fetch operations | Gauge | None |
| `ibm_sm_vault_dedicated_core_fetch_acl_and_token_avg_ms` | Average latency of ACL and token fetch operations in milliseconds | Gauge | Milliseconds |
{: caption=“Describes the Vault Dedicated core request handling metrics.” caption-side=“bottom”}

### Secret engine route operations
{: #vault-dedicated-route-operations}

These metrics track operations across all secret engine mounts and include a mount label identifying the specific secret engine.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_route_create_count` | Total number of secret create operations across all secret engine mounts | Gauge | None |
| `ibm_sm_vault_dedicated_route_create_avg_ms` | Average latency of secret create operations across all secret engine mounts in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_route_read_count` | Total number of secret read operations across all secret engine mounts | Gauge | None |
| `ibm_sm_vault_dedicated_route_read_avg_ms` | Average latency of secret read operations across all secret engine mounts in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_route_update_count` | Total number of secret write/update operations across all secret engine mounts | Gauge | None |
| `ibm_sm_vault_dedicated_route_update_avg_ms` | Average latency of secret write/update operations across all secret engine mounts in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_route_delete_count` | Total number of secret delete operations across all secret engine mounts | Gauge | None |
| `ibm_sm_vault_dedicated_route_delete_avg_ms` | Average latency of secret delete operations across all secret engine mounts in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_route_rollback_count` | Total number of secret engine rollback operations across all mounts | Gauge | None |
| `ibm_sm_vault_dedicated_route_rollback_avg_ms` | Average latency of secret engine rollback operations across all mounts in milliseconds | Gauge | Milliseconds |
{: caption=“Describes the Vault Dedicated route operation metrics.” caption-side=“bottom”}

### Token operations
{: #vault-dedicated-token-operations}

These metrics track the creation, lookup, revocation, and active count of Vault tokens, including breakdowns by auth method, policy, and TTL.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_token_creation_count` | Total number of Vault tokens generated | Gauge | None |
| `ibm_sm_vault_dedicated_token_create_count` | Total number of token creation operations | Gauge | None |
| `ibm_sm_vault_dedicated_token_create_avg_ms` | Average latency of token creation operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_token_create_max_ms` | Maximum latency of token creation operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_token_lookup_count` | Total number of token lookup operations | Gauge | None |
| `ibm_sm_vault_dedicated_token_lookup_avg_ms` | Average latency of token lookup operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_token_revoke_count` | Total number of token revocation operations | Gauge | None |
| `ibm_sm_vault_dedicated_token_store_count` | Total number of token store operations | Gauge | None |
| `ibm_sm_vault_dedicated_token_count` | Total number of active service tokens in Vault | Gauge | None |
| `ibm_sm_vault_dedicated_token_count_by_auth` | Number of active tokens broken down by auth method | Gauge | None |
| `ibm_sm_vault_dedicated_token_count_by_policy` | Number of active tokens broken down by attached policy | Gauge | None |
| `ibm_sm_vault_dedicated_token_count_by_ttl` | Number of active tokens broken down by TTL bucket | Gauge | None |
{: caption=“Describes the Vault Dedicated token operation metrics.” caption-side=“bottom”}

### Lease and expiration management
{: #vault-dedicated-leases}

These metrics track the lifecycle of Vault leases, including active lease counts, registration, renewal, revocation, and expiration processing.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_expire_num_leases` | Total number of active leases in the Vault instance | Gauge | None |
| `ibm_sm_vault_dedicated_expire_num_irrevocable_leases` | Number of leases that cannot be revoked | Gauge | None |
| `ibm_sm_vault_dedicated_expire_revoke_count` | Total number of lease revocation operations | Gauge | None |
| `ibm_sm_vault_dedicated_expire_revoke_avg_ms` | Average latency of lease revocation operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_expire_revoke_with_token_count` | Total number of revoke-all-leases-by-token operations | Gauge | None |
| `ibm_sm_vault_dedicated_expire_register_count` | Total number of new lease registrations | Gauge | None |
| `ibm_sm_vault_dedicated_expire_register_avg_ms` | Average latency of lease registration operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_expire_register_auth_count` | Total number of auth lease registration operations | Gauge | None |
| `ibm_sm_vault_dedicated_expire_renew_count` | Total number of lease renewal operations | Gauge | None |
| `ibm_sm_vault_dedicated_expire_renew_avg_ms` | Average latency of lease renewal operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_expire_job_manager_pending_jobs` | Number of pending lease revocation jobs in the job manager queue | Gauge | None |
| `ibm_sm_vault_dedicated_expire_job_manager_queue_length` | Current number of pending jobs in the lease expiration job manager queue | Gauge | None |
| `ibm_sm_vault_dedicated_expire_fetch_count` | Number of lease fetch operations by the expiration manager | Gauge | None |
| `ibm_sm_vault_dedicated_expire_fetch_avg_ms` | Average latency of lease fetch operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_expire_lease_expiration_count` | Number of leases that have expired and been processed | Gauge | None |
| `ibm_sm_vault_dedicated_expire_lease_expiration_error_count` | Number of errors encountered while processing expired leases | Gauge | None |
| `ibm_sm_vault_dedicated_expire_lease_expiration_time_in_queue_avg_ms` | Average time a lease spent in the expiration queue before being processed | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_expire_leases_by_expiration` | Number of active leases bucketed by expiration time | Gauge | None |
{: caption=“Describes the Vault Dedicated lease and expiration metrics.” caption-side=“bottom”}

### Policy operations
{: #vault-dedicated-policy-operations}

These metrics track read, write, list, and delete operations against Vault access control policies.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_policy_get_count` | Number of policy lookup (get_policy) operations | Gauge | None |
| `ibm_sm_vault_dedicated_policy_get_avg_ms` | Average latency of policy lookup (get_policy) operations in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_policy_set_count` | Number of policy write (set_policy) operations | Gauge | None |
| `ibm_sm_vault_dedicated_policy_list_count` | Number of policy list (list_policies) operations | Gauge | None |
| `ibm_sm_vault_dedicated_policy_delete_count` | Number of policy delete (delete_policy) operations | Gauge | None |
{: caption=“Describes the Vault Dedicated policy operation metrics.” caption-side=“bottom”}

### Identity and entity management
{: #vault-dedicated-identity}

These metrics track the creation and activity of identity entities, entity aliases, and group operations within the Vault identity system.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_identity_entity_active_monthly` | Number of monthly active identity entities in the Vault instance | Gauge | None |
| `ibm_sm_vault_dedicated_identity_nonentity_active_monthly` | Number of monthly active non-entity clients in the Vault instance | Gauge | None |
| `ibm_sm_vault_dedicated_identity_entity_alias_count` | Total number of entity aliases per authentication mount | Gauge | None |
| `ibm_sm_vault_dedicated_identity_entity_count` | Total number of identity entities stored in Vault | Gauge | None |
| `ibm_sm_vault_dedicated_identity_num_entities` | Number of identity entities tracked in memory | Gauge | None |
| `ibm_sm_vault_dedicated_identity_entity_creation_count` | Number of new identity entities created | Gauge | None |
| `ibm_sm_vault_dedicated_identity_upsert_entity_avg_ms` | Average latency of identity entity upsert transactions in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_identity_upsert_group_avg_ms` | Average latency of identity group upsert transactions in milliseconds | Gauge | Milliseconds |
{: caption=“Describes the Vault Dedicated identity and entity metrics.” caption-side=“bottom”}

### Secrets — KV and dynamic leases
{: #vault-dedicated-secrets-kv}

These metrics track the total number of KV secrets stored and the creation of dynamic secret leases across your Vault Dedicated instance.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_secret_kv_count` | Total number of KV secrets stored across all KV mounts | Gauge | None |
| `ibm_sm_vault_dedicated_secret_lease_creation_count` | Number of dynamic secret leases created | Gauge | None |
{: caption=“Describes the Vault Dedicated KV and dynamic secret metrics.” caption-side=“bottom”}

### Database secrets engine
{: #vault-dedicated-database}

These metrics track credential creation, renewal, revocation, and connection verification operations performed by the database secrets engine.

| Metric Name | Description | Metric Type | Value Type |
|---|---|---|---|
| `ibm_sm_vault_dedicated_database_create_user_count` | Total number of dynamic database credential creation operations | Gauge | None |
| `ibm_sm_vault_dedicated_database_create_user_avg_ms` | Average latency of dynamic database credential creation in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_database_renew_user_count` | Total number of database credential renewal operations | Gauge | None |
| `ibm_sm_vault_dedicated_database_revoke_user_count` | Total number of database credential revocation operations | Gauge | None |
| `ibm_sm_vault_dedicated_database_revoke_user_avg_ms` | Average latency of database credential revocation in milliseconds | Gauge | Milliseconds |
| `ibm_sm_vault_dedicated_database_verify_connection_count` | Total number of database connection verification operations | Gauge | None |
| `ibm_sm_vault_dedicated_database_verify_connection_avg_ms` | Average latency of database connection verification in milliseconds | Gauge | Milliseconds |
{: caption=“Describes the Vault Dedicated database secrets engine metrics.” caption-side=“bottom”}

## Attributes for segmentation
{: #attributes-for-segmentation}

You can filter your metrics by using segmentation attributes.

|Attribute Name|Description|
|--- |--- |
|`ibm_ctype`|public, dedicated, or local.|
|`ibm_location`|Location of the {{site.data.keyword.secrets-manager_short}} service instance.|
|`ibm_scope`|The account, organization, or space GUID associated with the metric.|
|`ibm_service_instance`|{{site.data.keyword.secrets-manager_short}} service instance ID.|
|`ibm_service_name`|secrets-manager.|
{: caption="Describes the attributes use for segmenting metrics." caption-side="bottom"}

## Metrics filter attributes
{: #metrics-filter-attributes}

You can scope down your metrics by using scope filters, which are more granular than the segmentation filters.

The first three attributes apply to all metrics. The remaining attributes are additional labels carried only by specific [Vault Dedicated]{: tag-green} metrics.

|Attribute Name|Applies to|Description|
|--- |--- |--- |
|`ibm_scope`|All metrics|The account, organization, or space GUID associated with the metric.|
|`ibm_location`|All metrics|The location of the instance.|
|`ibm_service_instance`|All metrics|The service instance id associated with the metric.|
|`mount`|`ibm_sm_vault_dedicated_route_*` [Vault Dedicated]{: tag-green}|The secret engine mount name the operation was routed to.|
|`mount_point`|`ibm_sm_vault_dedicated_identity_entity_alias_count`, `ibm_sm_vault_dedicated_secret_lease_creation_count` [Vault Dedicated]{: tag-green}|The mount path of the auth or secrets engine associated with the entity alias or lease.|
|`auth_method`|`ibm_sm_vault_dedicated_identity_entity_alias_count`, `ibm_sm_vault_dedicated_token_count_by_auth` [Vault Dedicated]{: tag-green}|The authentication method (e.g. token, approle) associated with the entity alias or token.|
|`policy`|`ibm_sm_vault_dedicated_token_count_by_policy` [Vault Dedicated]{: tag-green}|The Vault policy name attached to the tokens being counted.|
|`creation_ttl`|`ibm_sm_vault_dedicated_token_count_by_ttl` [Vault Dedicated]{: tag-green}|The TTL bucket for token creation time-to-live (e.g. 1h, 24h).|
|`expiring`|`ibm_sm_vault_dedicated_expire_leases_by_expiration` [Vault Dedicated]{: tag-green}|The expiration time bucket for active leases.|
{: caption="Describes the scope filters for {{site.data.keyword.secrets-manager_short}} metrics." caption-side="bottom"}

## Default dashboards
{: #default-dashboards}

### How to find the {{site.data.keyword.mon_short}} dashboard for {{site.data.keyword.secrets-manager_short}} using the Observability page
{: #find-observability}

After configuring your {{site.data.keyword.mon_short}} instance to receive platform metrics, follow these steps:

1. Go to the [Monitoring](/observe/monitoring){: external} dashboard and find your monitoring instance that is configured to receive platform metrics.
2. Click the **View {{site.data.keyword.mon_short}}** button in the **View Dashboard** column of the monitoring instance.
3. Once you are in the {{site.data.keyword.mon_short}} platform, click **Dashboards** to open up the side menu.
4. Select **{{site.data.keyword.secrets-manager_short}}** under the **IBM** section to view the dashboard.

### Opening the {{site.data.keyword.mon_short}} dashboard from {{site.data.keyword.secrets-manager_short}}
{: #open-from-sm}

After configuring your {{site.data.keyword.mon_short}} instance to receive platform metrics, you can open the dashboard directly from your {{site.data.keyword.secrets-manager_short}} instance.

1. Click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg).
2. Click the **Monitoring** option to open the dashboard.

## Setting alerts
{: #set-monitor-alerts}

You can set alerts on your {{site.data.keyword.mon_short}} dashboard to notify you of certain metrics. To setup a metric:

1. Click **Alerts** on the side menu.
2. Click **Add Alert** at the top of the page.
3. Select **Metric** as the alert type.
4. Select the aggregation and the appropriate metric.
5. Select the scope filter, if applicable.
6. Set the metric and time requirements for the alert to trigger.
7. Configure the notification channel and notification interval.
8. Click **Create**.
