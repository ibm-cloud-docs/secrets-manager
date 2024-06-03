---

copyright:
  years: 2024
lastupdated: "2024-06-03"

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

# {{site.data.keyword.mon_short}} operational metrics
{: #operational-metrics}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.mon_full_notm}} service to measure how users and applications interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

{{site.data.keyword.mon_full_notm}} records data on the operations that occur inside of {{site.data.keyword.cloud_notm}}. This service allows you to gain operational visibility into the performance and health of your applications, services, and platforms. You can use its advanced features to monitor and troubleshoot, define alerts based on API response codes, and design custom dashboards.

For more information regarding the {{site.data.keyword.mon_short}} service, check out [Getting started](/docs/monitoring?topic=monitoring-getting-started){: external}.

Enabling {{site.data.keyword.secrets-manager_short}} service metrics adds new metrics to your {{site.data.keyword.mon_short}} instance. For information on {{site.data.keyword.mon_short}} pricing, check out [Pricing](/docs/monitoring?topic=monitoring-pricing_plans){: external}.
{: tip}

## What metrics are available?
{: #metrics-available}

You can use {{site.data.keyword.mon_short}} to track the type of API requests being made in your service instance as well as the latency of the requests. The dashboard includes:

- Total requests being made in your {{site.data.keyword.secrets-manager_short}} instance, categorized by API type.
- Failed API requests categorized by error type.
- API request latency over time, including the average latency, highest latency, and lowest latency.
- Total amount of secrets and secret groups in the instance.

## Before you begin
{: #operational-metrics-considerations}

### Configure a {{site.data.keyword.mon_short}} instance for metrics
{: #configure-monitor}

Other {{site.data.keyword.cloud_notm}} users with `administrator` or `editor` permissions can manage the {{site.data.keyword.mon_short}} service in the {{site.data.keyword.cloud_notm}}. These users must also have platform permissions to create resources within the context of the resource group where they plan to provision the instance.
{: important}

To enable platform metrics in a region, complete the following steps:

1. [Provision an instance of {{site.data.keyword.mon_short}}](/docs/monitoring?topic=monitoring-provision){: external} in the region of the {{site.data.keyword.secrets-manager_short}} instance.

2. Go to the [Monitoring](/observe/monitoring) dashboard.

3. Click on **Configure platform metrics.**

4. Select the region where the {{site.data.keyword.secrets-manager_short}} instance was created.

5. Select the {{site.data.keyword.mon_short}} instance in which you would like to receive metrics.

6. Click **Configure.**

You can also reach this location by clicking on the **Actions** dropdown in your {{site.data.keyword.secrets-manager_short}} instance, followed by clicking on **Add monitoring**.

## {{site.data.keyword.secrets-manager_short}} metrics details
{: #sm-metrics}

You can use the metrics in your monitoring instance dashboard to measure the types of requests being made in your service instance as well as the latency of the requests.

### Resource count
{: #resource-count}

The total amount of secrets and secret groups in the instance

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|ibm_sm_secrets_count|Total amount of secrets|Gauge|None|
|ibm_sm_secret_groups_count|Total amount of secret groups|Gauge|None|
{: caption="Table 1. Describes the API Hits metrics." caption-side="bottom"}

### Total requests
{: #total-requests}

The type and amount of API requests being made to your {{site.data.keyword.secrets-manager_short}} instance. For example, you can track how many API requests have been made for read, write, or delete actions.

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|ibm_sm_delete_private_requests_count|Total amount of delete requests in private network|Gauge|None|
|ibm_sm_delete_public_requests_count|Total amount of delete requests in public network|Gauge|None|
|ibm_sm_read_private_requests_count|Total amount of read requests in private network|Gauge|None|
|ibm_sm_read_public_requests_count|Total amount of read requests in public network|Gauge|None|
|ibm_sm_write_private_requests_count|Total amount of write requests in private network|Gauge|None|
|ibm_sm_write_public_requests_count|Total amount of write requests in public network|Gauge|None|
{: caption="Table 1. Describes the API Hits metrics." caption-side="bottom"}

### Error count
{: #error-count}

This metric gathers the number of `4xx` and `5xx` errors encountered from all APIs.

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|ibm_sm_4xx_errors_count|Total amount of 4xx errors|Gauge|None|
|ibm_sm_5xx_errors_count|Total amount of 5xx errors|Gauge|None|
{: caption="Table 1. Describes the API Hits metrics." caption-side="bottom"}

## Latency
{: #latency}

This metric tracks amount of time it takes {{site.data.keyword.secrets-manager_short}} to receive an API request and respond to it.

The latency is calculated by getting the average of all requests of the same type that occur within 60 seconds.
{: note}

|Metric Name|Description|Metric Type|Value Type|
|--- |--- |--- |--- |
|ibm_sm_latency_delete_avg_ms|Delete operation average response time|Gauge|Milliseconds|
|ibm_sm_latency_delete_max_ms|Delete operation maximum response time|Gauge|Milliseconds|
|ibm_sm_latency_delete_min_ms|Delete operation minimum response time|Gauge|Milliseconds|
|ibm_sm_latency_read_avg_ms|Read operation average response time|Gauge|Milliseconds|
|ibm_sm_latency_read_max_ms|Read operation maximum response time|Gauge|Milliseconds|
|ibm_sm_latency_read_min_ms|Read operation minimum response time|Gauge|Milliseconds|
|ibm_sm_latency_write_avg_ms|Write operation average response time|Gauge|Milliseconds|
|ibm_sm_latency_write_max_ms|Write operation maximum response time|Gauge|Milliseconds|
|ibm_sm_latency_write_min_ms|Write operation minimum response time|Gauge|Milliseconds|
{: caption="Table 2. Describes the Latency metrics." caption-side="bottom"}

## Attributes for segmentation
{: #attributes-for-segmentation}

You can filter your metrics by using segmentation attributes.

|Attribute Name|Description|
|--- |--- |
|ibm_ctype|public, dedicated, or local.|
|ibm_location|Location of the {{site.data.keyword.secrets-manager_short}} service instance.|
|ibm_scope|The account, organization, or space GUID associated with the metric.|
|ibm_service_instance|{{site.data.keyword.secrets-manager_short}} service instance ID.|
|ibm_service_name|secrets-manager.|
{: caption="Table 3. Describes the attributes use for segmenting metrics." caption-side="bottom"}

## Metrics filter attributes
{: #metrics-filter-attributes}

You can scope down your metrics by using scope filters, which are more granular than the segmentation filters.

|Attribute Name|Description|
|--- |--- |
|ibm_scope|The account, organization, or space GUID associated with the metric.|
|ibm_location|The location of the instance.|
|ibm_service_instance|The service instance id associated with the metric.|
{: caption="Table 4. Describes the scope filters for {{site.data.keyword.secrets-manager_short}} metrics." caption-side="bottom"}

## Default dashboards
{: #default-dashboards}

### How to find the {{site.data.keyword.mon_short}} dashboard for {{site.data.keyword.secrets-manager_short}} using the Observability page
{: #find-observability}

After configuring your {{site.data.keyword.mon_short}} instance to receive platform metrics, follow the below steps:

1. Go to the [Monitoring](/observe/monitoring){: external} dashboard and find your monitoring instance that is configured to receive platform metrics.
2. Click on the **View {{site.data.keyword.mon_short}}** button in the **View Dashboard** column of the monitoring instance.
3. Once you are in the {{site.data.keyword.mon_short}} platform, click **Dashboards** to open up the side menu.
4. Select **{{site.data.keyword.secrets-manager_short}}** under the **IBM** section to view the dashboard.

To see metrics for one or more instances, select from the **ibm_service_instance** dropdown in the {{site.data.keyword.secrets-manager_short}} dashboard.
{: note}

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
