---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-23"

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
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}
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

# Regions and endpoints
{: #endpoints}

Review region and connectivity options for interacting with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

## Available regions
{: #available-regions}

You can create {{site.data.keyword.secrets-manager_short}} resources in one of the supported {{site.data.keyword.cloud_notm}} regions, which represents the geographic area where your {{site.data.keyword.secrets-manager_short}} requests are handled and processed.

- Dallas (`us-south`)
- Frankfurt (`eu-de`)
- London (`eu-gb`)
- Sydney (`au-syd`)
- Tokyo (`jp-tok`)
- Washington DC (`us-east`)

## Service endpoints
{: #service-endpoints}

You can use the {{site.data.keyword.secrets-manager_short}} APIs to manage your secrets programmatically. {{site.data.keyword.secrets-manager_short}} offers two connectivity options for interacting with its service APIs.

Public endpoints
:   By default, you can connect to resources in your account over the {{site.data.keyword.cloud_notm}} public network. Your data is encrypted in transit by using the Transport Security Layer (TLS) 1.2 protocol.

Private endpoints
:   To further secure your connection, you can also enable [virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint) for your infrastructure account. When you enable VRF for your account, you can [connect to {{site.data.keyword.secrets-manager_short}} by using a private IP](/docs/secrets-manager?topic=secrets-manager-service-connection) that is accessible only through the {{site.data.keyword.cloud_notm}} private network.

### Public endpoints
{: #public-endpoints}

If you are managing your {{site.data.keyword.secrets-manager_short}} resources over a public network, see the following table to determine the API endpoints to use when you connect to the {{site.data.keyword.secrets-manager_short}} API.

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud/api` |
| Frankfurt     | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud/api`    |
| London        | `https://{instance_ID}.eu-gb.secrets-manager.appdomain.cloud/api`    |
| Sydney        | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud/api`   |
| Tokyo         | `https://{instance_ID}.jp-tok.secrets-manager.appdomain.cloud/api`   |
| Washington DC | `https://{instance_ID}.us-east.secrets-manager.appdomain.cloud/api`  |
{: caption="Table 1. Public endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using standard {{site.data.keyword.cloud_notm}} APIs" caption-side="top"}
{: #public-endpoints-secrets-manager}
{: tab-title="Service API"}
{: tab-group="public-endpoints"}
{: class="simple-tab-table"}

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt     | `https://{instance_ID}.eu-de.secrets-manager.appdomain.cloud`    |
| London        | `https://{instance_ID}.eu-gb.secrets-manager.appdomain.cloud`    |
| Sydney        | `https://{instance_ID}.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_ID}.jp-tok.secrets-manager.appdomain.cloud`   |
| Washington DC | `https://{instance_ID}.us-east.secrets-manager.appdomain.cloud`  |
{: caption="Table 1. Public endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #public-endpoints-vault}
{: tab-title="Vault API"}
{: tab-group="public-endpoints"}
{: class="simple-tab-table"}

Ready to try the APIs? To interact with a Swagger UI from your browser, remove `/api` from your service endpoint URL and replace it with `/swagger-ui`. For example, `https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/swagger-ui`.
{: tip}

### Private endpoints
{: #private-endpoints}

If you need to manage your {{site.data.keyword.secrets-manager_short}} resources over a private network, see the following table to determine the API endpoints to use when you connect to the {{site.data.keyword.secrets-manager_short}} API.

To learn how to configure your {{site.data.keyword.secrets-manager_short}} instance to use private endpoints, see [Securing your connection to {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-service-connection).
{: note}

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.private.us-south.secrets-manager.appdomain.cloud/api` |
| Frankfurt     | `https://{instance_ID}.private.eu-de.secrets-manager.appdomain.cloud/api`    |
| London        | `https://{instance_ID}.private.eu-gb.secrets-manager.appdomain.cloud/api`    |
| Sydney        | `https://{instance_ID}.private.au-syd.secrets-manager.appdomain.cloud/api`   |
| Tokyo         | `https://{instance_ID}.private.jp-tok.secrets-manager.appdomain.cloud/api`   |
| Washington DC | `https://{instance_ID}.private.us-east.secrets-manager.appdomain.cloud/api`  |
{: caption="Table 2. Private endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using standard {{site.data.keyword.cloud_notm}} APIs" caption-side="top"}
{: #private-endpoints-secrets-manager}
{: tab-title="Service API"}
{: tab-group="private-endpoints"}
{: class="simple-tab-table"}

| Region        | Endpoint URL             |
| ------------- | ---------------------------- |
| Dallas        | `https://{instance_ID}.private.us-south.secrets-manager.appdomain.cloud` |
| Frankfurt     | `https://{instance_ID}.private.eu-de.secrets-manager.appdomain.cloud`    |
| London        | `https://{instance_ID}.private.eu-gb.secrets-manager.appdomain.cloud`    |
| Sydney        | `https://{instance_ID}.private.au-syd.secrets-manager.appdomain.cloud`   |
| Tokyo         | `https://{instance_ID}.private.jp-tok.secrets-manager.appdomain.cloud`   |
| Washington DC | `https://{instance_ID}.private.us-east.secrets-manager.appdomain.cloud`  |
{: caption="Table 2. Private endpoints for interacting with {{site.data.keyword.secrets-manager_short}} by using the native Vault APIs" caption-side="top"}
{: #private-endpoints-vault}
{: tab-title="Vault API"}
{: tab-group="private-endpoints"}
{: class="simple-tab-table"}


### Viewing your endpoint URLs
{: #view-endpoint-urls}

You can find your service endpoint URLs in the **Endpoints** page of the {{site.data.keyword.secrets-manager_short}} UI. If you need to retrieve your service endpoint URLs programmatically, you can also call the following API to retrieve the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

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
    },
    "private_endpoints": {
        "service_api": "https://927fb8ae-1ddd-4483-a21f-7d3c0fc845f5.private.us-south.secrets-manager.appdomain.cloud/api",
    "vault_api": "https://927fb8ae-1ddd-4483-a21f-7d3c0fc845f5.private.us-south.secrets-manager.appdomain.cloud"
    }
}
```
{: screen}

To try this API, you can interact with the following Swagger UI from your browser: `https://{region}.secrets-manager.cloud.ibm.com/swagger-ui/`.
{: tip}

If your instance is configured with the **Private only** option, this API returns only the `private_endpoints` object in the response.
{: note}




