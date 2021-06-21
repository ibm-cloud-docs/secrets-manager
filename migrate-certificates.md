---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-21"

keywords: 

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

# Migrating certificates from {{site.data.keyword.cloudcerts_short}}
{: #migrate-from-certificate-manager}

With {{site.data.keyword.secrets-manager_full}}, you can centralize your application secrets in a single service, including the TLS certificates that you might already store and manage in {{site.data.keyword.cloudcerts_long_notm}}. Learn about suggested guidelines for moving your certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}.
{: shortdesc}

## Comparison between {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}}
{: #migrate-differences}

Both {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} provide a secure repository for storing and managing certificates. The following table compares and contrasts some common characteristics between the services.


| Characteristics | {{site.data.keyword.secrets-manager_short}} | {{site.data.keyword.cloudcerts_short}} |
| --- | --- | --- |
| Secrets or certificates management experience in the {{site.data.keyword.cloud_notm}} console | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Worldwide availability in multizone regions | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Data isolation through single-tenancy | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to integrate a key management service| ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources through private service endpoints | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage resources in an {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Ability to import SSL or TLS certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to order public SSL or TLS certificates from Let's Encrypt | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Ability to manage secrets of various types | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Notifications |  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Go, Python, Node.js, and Java SDKs | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| CLI plug-in | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Logging and monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison between the {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.cloudcerts_short}} offerings" caption-side="top"}


## Migrating {{site.data.keyword.cloudcerts_short}} resources to {{site.data.keyword.secrets-manager_short}}
{: #migrate-process}

You can take advantage of the data isolation benefits of a single-tenant secrets management service by migrating your existing certificates in {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}. 

### {{site.data.keyword.secrets-manager_short}} limits and considerations
{: #migrate-prepare}

Before you begin, consider the following items and service limitations that might impact your experience as you integrate to {{site.data.keyword.secrets-manager_short}}.

- **{{site.data.keyword.secrets-manager_short}} instances are limited to 1 per account.** Currently, {{site.data.keyword.secrets-manager_short}} is a free service that enforces a limit of one instance per {{site.data.keyword.cloud_notm}} account. Pricing and service limits for {{site.data.keyword.secrets-manager_short}} are subject to change. During the transition period, you can create a {{site.data.keyword.secrets-manager_short}} service instance to start migrating your existing certificates from {{site.data.keyword.cloudcerts_short}}. {{site.data.keyword.secrets-manager_short}} does not enforce a limit on the total number of secrets or certificates you can store per instance.
- **Provisioning a {{site.data.keyword.secrets-manager_short}} instance takes 5 - 8 minutes to complete.** Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} is a single-tenant offering. During instance provisioning, {{site.data.keyword.secrets-manager_short}} creates various dedicated resources that are assigned to your service instance only. If you dynamically provision instances of {{site.data.keyword.cloudcerts_short}} and you plan to do the same with {{site.data.keyword.secrets-manager_short}} instances, keep in mind that {{site.data.keyword.secrets-manager_short}} provisioning is asynchronous and takes 5 - 8 minutes to complete.
- **Secret groups in {{site.data.keyword.secrets-manager_short}} are used to enforce granular access to secrets.** In {{site.data.keyword.cloudcerts_short}}, you can create access policies on individual certificates. In {{site.data.keyword.secrets-manager_short}}, you can set access policies on secret groups that contain one or more certificates. Additionally, {{site.data.keyword.secrets-manager_short}} supports a **SecretsReader** IAM role that provides read-only access to download certificates. 
- **{{site.data.keyword.secrets-manager_short}} provides a unique endpoint URL for each service instance.** Unlike {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.secrets-manager_short}} constructs a unique endpoint URL for your service instance. {{site.data.keyword.secrets-manager_short}} endpoints uses the `appdomain.cloud` domain, whereas {{site.data.keyword.cloudcerts_short}} uses the `cloud.ibm.com` domain. For more information, review the {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} API docs.
- **{{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} APIs are different in structure.** If you use {{site.data.keyword.cloudcerts_short}} to manage your certificates programmatically, be sure to review the [{{site.data.keyword.secrets-manager_short}} API docs](/apidocs/secrets-manager) to understand how moving your certificates impacts your current experience. 


### Migration guidelines
{: #migrate-guidelines}

If you're ready to start your transition to {{site.data.keyword.secrets-manager_short}}, you can use a combination of {{site.data.keyword.cloud_notm}} platform and service APIs to create an inventory of your existing {{site.data.keyword.cloudcerts_short}} resources, retrieve your certificate data, and copy the data over into a test {{site.data.keyword.secrets-manager_short}} service instance. 

1. [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance).

2. Create an inventory of {{site.data.keyword.cloudcerts_short}} resources.

    Before you can migrate your {{site.data.keyword.cloudcerts_short}} data, you need to determine the resource configurations that are currently used for {{site.data.keyword.cloudcerts_short}} instances in your accounts. If you're managing multiple {{site.data.keyword.cloudcerts_short}} instances, you can use the [Resource Controller](/apidocs/resource-controller/resource-controller#list-resource-instances) and [{{site.data.keyword.cloudcerts_short}}](/apidocs/certificate-manager#list) APIs. You can use these APIs to retrieve the properties of all {{site.data.keyword.cloudcerts_short}} instances in your account programmatically, such as the region, resource group ID, service name, and CRN.

    For example, you can run the following cURL command to retrieve information about {{site.data.keyword.cloudcerts_short}} instances in your account. 

    ```sh
    curl -X GET "https://resource-controller.cloud.ibm.com/v2/resource_instances?resource_plan_id=14edb41b-ebd6-46b1-a39a-92df79906ca7" \
      -H 'Authorization: Bearer {IAM_token}'
    ```
    {: codeblock}

    `14edb41b-ebd6-46b1-a39a-92df79906ca7` is the plan ID that is unique to {{site.data.keyword.cloudcerts_short}} instances. For more information, see the [Resource Controller V2 API docs](/apidocs/resource-controller/resource-controller#list-resource-instances).
    {: note}

    Then, you can use the [List certificates API](/apidocs/certificate-manager#list) to retrieve the total number of certificates and list of certificate IDs that belong to a specified {{site.data.keyword.cloudcerts_short}} instance.

    ```sh
    curl -X GET "https://{region}.certificate-manager.cloud.ibm.com/api/v3/{url_encoded_instance_crn}/certificates" \
        -H 'Authorization: Bearer {IAM_token}'
    ```
    {: codeblock}

3. Determine an access hierarchy for your certificates within {{site.data.keyword.secrets-manager_short}}. 

    Create secret groups in {{site.data.keyword.secrets-manager_short}} ahead of time so that you can organize your incoming certificates by mapped IAM policies. 
    
    Be sure to create secret groups first because you can't change assignments to certificates after you migrate them. If you accidentally assign an incoming certificate to the wrong secret group, or if you don't want a certificate to belong to the default secret group, you must delete certificate and add it again.
    {: important}

4. Copy certificates from {{site.data.keyword.cloudcerts_short}} to {{site.data.keyword.secrets-manager_short}}. 

    After you determine which certificates need to be migrated, use the {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} APIs to copy your certificate data to {{site.data.keyword.secrets-manager_short}}. For example, you can create a script that uses the following environment variables to gather information about where your certificates are stored in {{site.data.keyword.cloudcerts_short}} and their target destination in {{site.data.keyword.secrets-manager_short}}:

    ```sh
    CM_INSTANCE_CRN="<instance_crn>"
    CERTIFICATE_ID="<certificate_id>"
    SM_INSTANCE_CRN="<instance_crn>"
    SECRET_GROUP_NAME="<group_name>"
    ```
    {: codeblock}

    <table>
        <tr>
            <th>Variable</th>
            <th>Description</th>
        </tr>
        <tr>
            <td><code>CM_INSTANCE_CRN</code></td>
            <td>The CRN of the {{site.data.keyword.cloudcerts_short}} service instance that contains your existing certificates.</td>
        </tr>
        <tr>
            <td><code>CERTIFICATE_ID</code></td>
            <td>The ID of the certificate that you want to migrate.</td>
        </tr>
        <tr>
            <td><code>SM_INSTANCE_CRN</td>
            <td>The CRN of the {{site.data.keyword.secrets-manager_short}} service instance where you want to copy the certificates.</td>
        </tr>
        <tr>
            <td><code>SECRET_GROUP_NAME</code></td>
            <td>The name of secret group in {{site.data.keyword.secrets-manager_short}} that contains the access policy that you want to assign to the incoming certificate.</td>
        </tr>
        <caption style="caption-side:bottom">Table 2. Environment variables to map certificate resources</caption>
    </table>

    Next, you can call the [Get a certificate API](/apidocs/certificate-manager#get) to retrieve the data that's associated with each certificate that you want to migrate. The output for this API contains the `contents`, `priv_key`, and `intermediate` values that you can copy into {{site.data.keyword.secrets-manager_short}}. Use this data to call the [Create a secret API](/apidocs/secrets-manager#create-secret) to import the contents of each certificate.

    The following table shows how certificate data fields map to each other in the {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.secrets-manager_short}} APIs.

    <table>
        <tr>
            <th>{{site.data.keyword.cloudcerts_short}} API</th>
            <th>{{site.data.keyword.secrets-manager_short}} API</th>
            <th>Description</th>
        </tr>
        <tr>
            <td><code>.data.content</code></td>
            <td><code>.resources[].secret_data.certificate</code></td>
            <td>(Always included) The certificate data.</td>
        </tr>
        <tr>
            <td><code>.data.priv_key</td>
            <td><code>.resources[].secret_data.private_key</code></td>
            <td>The private key that matches the certificate.</td>
        </tr>
        <tr>
            <td><code>.data.intermediate</code></td>
            <td><code>.resources[].secret_data.intermediate</code></td>
            <td>The intermediate certificate data.</td>
        </tr>
        <caption style="caption-side:bottom">Table 3. Fields that contain certificate data</caption>
    </table>


