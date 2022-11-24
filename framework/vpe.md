---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-24"

keywords: connect to {{site.data.keyword.secrets-manager_short}} on a VPC, virtual service endpoints, virtual private cloud, connect via VPC, connect through VPC, connect via VPE, connect through VPE

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

# Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.secrets-manager_short}}
{: #virtual-private-endpoint}

{{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC enables you to connect to {{site.data.keyword.secrets-manager_short}} from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC.
{: shortdesc}

VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance, basis (depending on the service operation model). The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud}} service on the private backbone. VPE for VPC gives you the experience of controlling all the private addressing within your cloud. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

## Before you begin
{: #vpe-prereqs}

Before you target a virtual private endpoint for {{site.data.keyword.secrets-manager_short}}, be sure that you already have a VPC in your {{site.data.keyword.cloud_notm}} account. To access {{site.data.keyword.secrets-manager_short}} from your private VPC network, you also need to [create an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for the {{site.data.keyword.secrets-manager_short}} service.

For more information about setting up VPC, check out [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started). 

## Setting up a VPE for {{site.data.keyword.secrets-manager_short}} with the API
{: #vpe-setup}
{: api}

You can update your existing {{site.data.keyword.secrets-manager_short}} instance to connect over a virtual private gateway by completing the following steps. 

1. Define the `VPE_ENDPOINTS` variable that corresponds with the region where your {{site.data.keyword.secrets-manager_short}} is located.

    Dallas:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.48.236\", \"zone\": \"dal10\"},{\"ip_address\": \"166.9.51.178\", \"zone\": \"dal12\"},{\"ip_address\": \"166.9.58.178\", \"zone\": \"dal13\"}]"
    ```
    {: codeblock}

    Frankfurt:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.30.75\", \"zone\": \"fra04\"},{\"ip_address\": \"166.9.32.95\", \"zone\": \"fra05\"},{\"ip_address\": \"166.9.28.77\", \"zone\": \"fra02\"}]"
    ```
    {: codeblock}

    London:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.36.128\", \"zone\": \"lon04\"},{\"ip_address\": \"166.9.34.111\", \"zone\": \"lon05\"},{\"ip_address\": \"166.9.38.116\", \"zone\": \"lon06\"}]"
    ```
    {: codeblock}

    Osaka:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.70.57\", \"zone\": \"osa21\"},{\"ip_address\": \"166.9.71.53\", \"zone\": \"osa22\"},{\"ip_address\": \"166.9.72.55\", \"zone\": \"osa23\"}]"
    ```
    {: codeblock}

    Sao Paulo:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.84.65\", \"zone\": \"sao05\"},{\"ip_address\": \"166.9.83.66\", \"zone\": \"sao04\"},{\"ip_address\": \"166.9.82.71\", \"zone\": \"sao01\"}]"
    ```
    {: codeblock}

    Sydney:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.52.52\", \"zone\": \"syd01\"},{\"ip_address\": \"166.9.54.60\", \"zone\": \"syd04\"},{\"ip_address\": \"166.9.56.57\", \"zone\": \"syd05\"}]"
    ```
    {: codeblock}

    Tokyo:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.44.54\", \"zone\": \"tok05\"},{\"ip_address\": \"166.9.40.53\", \"zone\": \"tok02\"},{\"ip_address\": \"166.9.42.68\", \"zone\": \"tok04\"}]"
    ```
    {: codeblock}

    Toronto:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.78.62\", \"zone\": \"tor05\"},{\"ip_address\": \"166.9.77.62\", \"zone\": \"tor04\"},{\"ip_address\": \"166.9.76.65\", \"zone\": \"tor01\"}]"
    ```
    {: codeblock}
  
    Washington DC:
    ```sh
    VPE_ENDPOINTS="[{\"ip_address\": \"166.9.22.61\", \"zone\": \"wdc06\"},{\"ip_address\": \"166.9.20.170\", \"zone\": \"wdc04\"},{\"ip_address\": \"166.9.24.62\", \"zone\": \"wdc07\"}]"
    ```
    {: codeblock}

2. Define the `INSTANCE_ID` variable with your {{site.data.keyword.secrets-manager_short}} instance ID.

    ```sh
    INSTANCE_ID=<instance_id>
    ```
    {: codeblock}

    Replace `<instance_id>` with the unique identifier for your {{site.data.keyword.secrets-manager_short}} service instance.

3. Define the `REGION` variable that corresponds with the region where your {{site.data.keyword.secrets-manager_short}} is located.

    ```sh
    REGION=<region>
    ```
    {: codeblock}

    Replace `<region>` with the region abbreviation that represents the location where your {{site.data.keyword.secrets-manager_short}} service instance resides. Supported values include: `us-south`, `us-east`, `eu-gb`, `eu-de`, `jp-tok`, `au-syd`. For more information, see [Available regions](/docs/secrets-manager?topic=secrets-manager-endpoints#supported-regions).

4. Update your instance configuration by calling the [Resource Controller V2 API](/apidocs/resource-controller/resource-controller#update-resource-instance).

    Before you run the command, be sure to [generate an IAM token](/docs/account?topic=account-iamtoken_from_apikey).

    ```sh
    curl -X PATCH https://resource-controller.cloud.ibm.com/v2/resource_instances/"$INSTANCE_ID" -H "Authorization: Bearer <IAM_token>" -H 'Content-Type: application/json' -d "{\"extensions\": {\"virtual_private_endpoints\": {\"dns_domain\": \"private.$REGION.secrets-manager.appdomain.cloud\",\"dns_hosts\": [\"$INSTANCE_ID\"],\"endpoints\": $VPE_ENDPOINTS,\"origin_type\": \"cse\",\"ports\": [{\"port_max\": 443, \"port_min\": 443}]}}}"
    ```
    {: pre}

    Replace `<IAM_token>` with your access token.

5. Verify that your instance was configured successfully.

    ```sh
    curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances/"$INSTANCE_ID" -H "Authorization: Bearer <IAM_token>"
    ```
    {: pre}

    Replace `<IAM_token>` with your access token. The `extensions` object is updated to with your new virtual service endpoint configurations:

    ```json
    {
        "extensions": {
        "virtual_private_endpoints": {
          "dns_domain": "private.eu-de.secrets-manager.appdomain.cloud",
          "dns_hosts": [
            "1543356j-4f19-4e4j-9b36-477a7fc2b32e"
          ],
          "endpoints": [
            {
              "ip_address": "166.9.30.75",
              "zone": "fra04"
            },
            {
              "ip_address": "166.9.32.95",
              "zone": "fra05"
            },
            {
              "ip_address": "166.9.28.77",
              "zone": "fra02"
            }
          ],
          "origin_type": "cse",
          "ports": [
            {
              "port_max": 443,
              "port_min": 443
            }
          ]
        }
        }
    }
    ```
    {: screen}


