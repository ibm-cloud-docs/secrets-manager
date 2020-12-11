---

copyright:
  years: 2020
lastupdated: "2020-12-11"

keywords: Secrets Manager availability, regions, Secrets Manager endpoints, Vault endpoint

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# Regions and endpoints
{: #endpoints}

Review region and connectivity options for interacting with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

## Available regions
{: #available-regions}

You can create {{site.data.keyword.secrets-manager_short}} resources in one of the supported {{site.data.keyword.cloud_notm}} regions, which represent the geographic area where your {{site.data.keyword.secrets-manager_short}} requests are handled and processed.

- Dallas (`us-south`)
- Frankfurt (`eu-de`)
- Sydney (`au-syd`)



## Service endpoints
{: #service-endpoints}

If you are managing your {{site.data.keyword.secrets-manager_short}} resources programmatically, see the following table to determine the API endpoints to use when you connect to the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager){: external}.

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud/api` |
| Frankfurt     | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud/api`    |
| Sydney        | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud/api`   |
{: caption="Table 1. Endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using standard {{site.data.keyword.cloud_notm}} APIs" caption-side="top"}
{: #table-1}
{: tab-title="Service API"}
{: class="comparison-tab-table"}
{: row-headers}

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt     | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud`    |
| Sydney        | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud`   |
{: caption="Table 2. Endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #table-2}
{: tab-title="Vault API"}
{: class="comparison-tab-table"}
{: row-headers}

Ready to try the APIs? To interact with a Swagger UI from your browser, remove `/api` from your service endpoint URL and replace it with `/swagger-ui`. For example, `https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/swagger-ui`.
{: tip}

### Retrieving your service endpoint URLs
{: #retrieve-service-endpoints}

You can find your service endpoint URLs in the **Endpoints** section of the {{site.data.keyword.secrets-manager_short}} UI. If you need to retrieve your service endpoint URLs programmatically, you can also call the following API to retrieve the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

```sh
curl -X GET "https://{region}.secrets-manager.cloud.ibm.com/api/v1/instances/{url_encoded_instance_CRN}/endpoints" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer {IAM_token}"
```
{: pre}

Replace the variables in the example request according to the following table.

| Parameter | Description |
| --- | --- |
| `{region}` | The region abbreviation that represents the geographic area where your {{site.data.keyword.secrets-manager_short}} resides. For example, `us-south` or `eu-de`. |
| `{url_encoded_instance_CRN}` | The Cloud Resource Name (CRN) that uniquely identifies your {{site.data.keyword.secrets-manager_short}} service instance. The value must be URL encoded. |
| `{IAM_token}` | Your {{site.data.keyword.cloud_notm}} IAM access token. |
{: caption="Table 3. Required parameters for retrieving service endpoints with the API" caption-side="top"}

A successful request returns the endpoint URLs that are associated with the region and service instance CRN that you specify. The following JSON snippet shows an example response.

```json
{
  "public_endpoints": {
    "service_api": "https://927fb8ae-1ddd-4483-a21f-7d3c0fc845f5.us-south.secrets-manager.appdomain.cloud/api",
    "vault_api": "https://927fb8ae-1ddd-4483-a21f-7d3c0fc845f5.us-south.secrets-manager.appdomain.cloud"
  }
}
```
{: screen}

To try this API, you can interact with the following Swagger UI from your browser: `https://{region}.secrets-manager.cloud.ibm.com/swagger-ui/`.
{: tip}

