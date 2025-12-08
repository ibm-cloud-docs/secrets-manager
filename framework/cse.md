---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-08"

keywords: isolation for {{site.data.keyword.secrets-manager_short}}, service endpoints for {{site.data.keyword.secrets-manager_short}}, private network for {{site.data.keyword.secrets-manager_short}}, network isolation in {{site.data.keyword.secrets-manager_short}}, non-public routes for {{site.data.keyword.secrets-manager_short}}, private connection for {{site.data.keyword.secrets-manager_short}}

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

# Using service endpoints to privately connect to {{site.data.keyword.secrets-manager_short}}
{: #service-connection}

To ensure that you have enhanced control and security over your data when you use {{site.data.keyword.secrets-manager_short}}, you have the option of using private routes to {{site.data.keyword.cloud}} service endpoints. Private routes are not accessible or reachable over the internet. By using the {{site.data.keyword.cloud_notm}} private service endpoints feature, you can protect your data from threats from the public network and logically extend your private network.
{: shortdesc}


## Before you begin
{: #prereq-service-endpoint}

You must first enable virtual routing and forwarding in your account, and then you can enable the use of {{site.data.keyword.cloud_notm}} private service endpoints. For more information about setting up your account to support the private connectivity option, see [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).

Keep in mind the following considerations:

- You can select a service endpoint option for a {{site.data.keyword.secrets-manager_short}} instance only at its creation.

## Setting up private endpoints for {{site.data.keyword.secrets-manager_short}} in the UI
{: #endpoint-setup-ui}
{: ui}

After your account is enabled for VRF and service endpoints, you can provision a {{site.data.keyword.secrets-manager_short}} service instance to connect over a private service endpoint.

1. In the {{site.data.keyword.cloud_notm}} console, go to the [{{site.data.keyword.secrets-manager_short}} offering details page](/catalog/services/secrets-manager){: external}.
2. In the **Create** tab, select the region where you want provision your instance.
3. Review and select a pricing plan.
4. Provide a name for your instance.
5. Select a resource group.
6. Determine an option for managing encryption for your instance.

    You can enhance the security of your secrets at rest by integrating with a key management service. For more information about customer-managed encryption, check out [Protecting your sensitive data in {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-mng-data#data-encryption).
7. By default, newly provisioned {{site.data.keyword.secrets-manager_short}} instances accept API requests from **private-only** endpoints
8. Click **Create**.

    Provisioning a {{site.data.keyword.secrets-manager_short}} instance can take 5 - 15 minutes to complete.
    {: note}

## Setting up private endpoints for {{site.data.keyword.secrets-manager_short}} from the CLI
{: #endpoint-setup-cli}
{: cli}

After your account is enabled for VRF and service endpoints, you can provision a {{site.data.keyword.secrets-manager_short}} service instance to connect over a private service endpoint.

1. In a terminal window, log in to {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
    {: note}

2. Select the account, region, and resource group where you want to create a {{site.data.keyword.secrets-manager_short}} service instance.

    You can use the following command to set your target region and resource group.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. (Optional) Check whether your account is enabled for VRF and service endpoints.

    ```sh
    ibmcloud account show
    ```
    {: pre}

    The following CLI output shows the account details of a VRF and service
    endpoint-enabled account.

    ```plaintext
    Retrieving account John Doe's Account of john.doe@email.com...
    OK

    Account ID:                   d154dfbd0bc2edefthyufffc9b5ca318
    Currently Targeted Account:   true
    Linked Softlayer Account:     1008967
    VRF Enabled:                  true
    Service Endpoint Enabled:     true
    ```
    {: screen}

    For more information about enabling VRF and service endpoints in your account, see
    [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).
    {: tip}

4. Create a private {{site.data.keyword.secrets-manager_short}} service instance by running the following command.

    ```sh
    ibmcloud resource service-instance-create <instance_name> secrets-manager trial <region> -p '{"allowed_network":"<connectivity-option>"}'
    ```
    {: pre}

    | Variable | Description |
    | -------- | ----------- |
    | `region` | The region abbreviation, such as `us-south` that represents the geographic area where you want your {{site.data.keyword.secrets-manager_short}} to be handled and processed. For a complete list of supported regions, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints). |
    | `connectivity-option` | The network connectivity option that you want to allow for your instance. To allow access to the instance over both public and private service endpoints, use `public-and-private`. To limit API requests to the instance to take place only through a private network, use `private-only`, this is also the default option. |
    {: caption="Variable descriptions" caption-side="top"}


5. (Optional) Verify that that the service instance was created successfully.

    ```sh
    ibmcloud resource service-instance <instance_name>
    ```
    {: pre}

    Provisioning a {{site.data.keyword.secrets-manager_short}} instance can take 5 - 15 minutes to complete.
    {: note}

## Connect to a private-only instance from outside of the IBM Cloud internal network
{: #connect-to-sm-privately}

When using a {{site.data.keyword.secrets-manager_short}} instance with a private-only endpoint, connecting to the instance is possible only through the IBM Cloud internal network, using existing service integrations or VMs.  
To connect to a private-only instance for example from a local workstation, select one of the following available options.

### Connect using a VM
{: #connect-to-sm-privately-vm}

Use this option for {{site.data.keyword.secrets-manager_short}} instances provisioned in IBM Cloud Classic data centers, such as Dallas, Washington, Sao Paulo, London, Frankfurt, Madrid, Sydney, Tokyo, and Osaka.

1. From the [IBM Cloud catalog](https://cloud.ibm.com/catalog), search for "Virtual Server" and provision a virtual machine matching your requirements
2. From your local workstation, login to the VM using its generated credentials
3. From within the VM, login to IBM Cloud and use your {{site.data.keyword.secrets-manager_short}} instance via API or CLI

### Connect using a VPE gateway
{: #connect-to-sm-privately-vpe-gateway}

Use of this option is required for {{site.data.keyword.secrets-manager_short}} instances provisioned in IBM Cloud Next Generation data centers. It can be used for other regions as well.

1. [Create a VPE gateway](https://cloud.ibm.com/infrastructure/network/endpointGateways)
2. Attach the VPE to a new or existing VPC
3. Add your {{site.data.keyword.secrets-manager_short}} instance to the VPC
4. Create a VSI in the same VPC and set it up
5. From your local workstation, login to the VSI
6. From within the VSI, login to IBM Cloud and use your {{site.data.keyword.secrets-manager_short}} instance via API or CLI 
