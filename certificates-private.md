---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-29"

keywords: import certificates, order certificates, request certificates, ssl certificates, tls certificates, private certificates

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

# Creating SSL/TLS private certificates
{: #private-certificates}

You can use {{site.data.keyword.secrets-manager_full}} to store, request, and generate private SSL/TLS certificates that you can use for your apps or services.
{: shortdesc}

An SSL/TLS certificate is a type of digital certificate that is used to establish communication privacy between a server and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information that is used to create trusted and secure connections between endpoints. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

In {{site.data.keyword.secrets-manager_short}}, certificates that you create by using a private certificate authority are private certificates (`private_cert`). Certificates that you import to the service are [imported certificates](/docs/secrets-manager?topic=secrets-manager-certificates) (`imported_cert`). Certificates that you order through {{site.data.keyword.secrets-manager_short}} from a third-party certificate authority are [public certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates) (`public_cert`). 
{: note}

## Before you begin
{: #before-private-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

Before you create a private certificate, be sure that you:  
- [Prepare your instance to create certificates](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates).  
- Review the intermediate certificate authorities that are available. To view the configurations that are defined for your instance, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.  
- Review the [certificate templates](/docs/secrets-manager?topic=secrets-manager-certificate-templates) that are available for your selected intermediate CA.  

A template selection is required when you create a private certificate. Depending on the template that you choose, some restrictions for your private certificate might apply. To view the templates that are defined for your intermediate CA, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI. Expand the row of the intermediate CA that you want to use as the issuing authority for private certificate, and click **Templates** to review the templates that are available. 

## Creating private certificates
{: #create-private-certificates}

After you [configure the private certificates engine](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates&interface=ui) for your instance, you can use {{site.data.keyword.secrets-manager_short}} to generate private certificates by using an internal certificate authority that was previously configured for your {{site.data.keyword.secrets-manager_short}} service instance. Before a certificate can be issued to you, {{site.data.keyword.secrets-manager_short}} checks to ensure that your certificate request matches the restrictions that are defined for the [certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates) that you select. After a certificate is issued, you can deploy it to your integrated apps, download it, revoke it, or rotate it manually.


### Creating private certificates in the UI
{: #create-private-certificates-ui}
{: ui}

You can create a private certificate by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Click the **Create private certificate** tile.
5. Click **Next**.
6. Add a name and description to easily identify your certificate.
7. Optional: Add labels to help you to search for similar secrets in your instance.
8. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
9. Click **Next**.
10. Select a certificate authority configuration.

    The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Private certificates**.
   
11. Select a [certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates).

    The template that you select determines the parameters to be applied to your generated certificate. To view the details of the certificate templates that are defined for your selected certificate authority, you can go to **Secrets engines > Private certificates**. From the list of certificate authorities, expand the row of the CA that you want to use as the issuing authority for your private certificate, and click **Templates**.
12. Optional: Enable automatic rotation for the certificate.

    To enable automatic rotation, switch the rotation toggle to **On**. Select an interval and unit that specifies the number of days between scheduled rotations.

    Depending on the certificate template that you choose in the following steps, some restrictions on the rotation interval for your private certificate might apply. For example, the rotation interval can't exceed the time-to-live (TTL) that is defined in the template. For more information, see [Certificate templates](/docs/secrets-manager?topic=secrets-manager-certificate-templates).
    {: note}

13. Required: Specify a common name for your certificate.

    Depending on the certificate template that you choose, some restrictions on the common name might apply. To view the details of your selected certificate template, you can go to **Secrets engines > Private certificates**. From the list of certificate authorities, expand the row of the CA that you want to use as the issuing authority for your private certificate, and click **Templates**.
14. Select the [secret group](#x9968962){: term} that you want to assign to the certificate.

    If your selected certificate template allows certificates to be added to specific secret groups, only those allowed groups are listed. If the template has no restrictions, you can create a secret group if you don't already have one. Your certificate is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
15. Optional: Specify alternative names for your certificate.

    The alternative names can be hostnames or email addresses.
16. Click **Next**.
17. Review the details of your certificate.
18. Click **Add**.

### Creating private certificates from the CLI
{: #generate-private-certificates-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To create a private certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command creates a private certificate secret from the certificate template that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
ibmcloud secrets-manager secret-create --secret-prototype=
    '[{
        "name": "example-private-certificate", 
        "description": "Extended description for this secret.", 
        "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
        "labels": [
            "dev","us-south"
        ], 
        "certificate_template": "example-certificate-template"
        "common_name": "cert_common_name",
        "rotation": {
            "enabled": false,
            "rotate_keys":false
            },
        "custom_metadata" : {
        "anyKey" : "anyValue"
    },
        "version_custom_metadata" : {
            "anyKey" : "anyValue"
        },
        "expiration_date" : "2030-01-01T00:00:00Z",
        }
    ]
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).


### Creating private certificates with the API
{: #create-private-certificates-api}
{: api}

You can generate private certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a private certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST 
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "name": "example-private-certificate",
        "description": "Description of my private certificate",
        "secret_type": "private_cert",
        "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
        "labels": [
            "dev",
            "us-south"
        ],
        "certificate_template": "test-certificate-template",
        "common_name": "localhost",
        "alt_names": [
            "alt-name-1",
            "alt-name-2"
        ],
        "ip_sans": "127.0.0.1",
        "uri_sans": "https://www.example.com/test",
        "ttl": "2190h",
        "rotation": {
            "auto_rotate": true,
            "interval": 1,
            "unit": "month"
        },
        "custom_metadata": {
            "metadata_custom_key": "metadata_custom_value"
        },
        "version_custom_metadata": {
            "custom_version_key": "custom_version_value"
        }
        }' \ "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets" 
```
{: codeblock}
{: curl}


Need to create a private certificate with advanced options? You can use optional request parameters to specify advanced attributes for your private certificate, such as Subject Alternative Names or a time-to-live (TTL). If you omit these optional parameters, the attributes that are defined for your selected certificate template are applied. For more information, see the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret).
{: tip}

A successful request returns the contents of your private certificate, along with other metadata that is determined by the certificate template and issuing certificate authority.

```json
{
  "alt_names": [
    "s1.example.com",
    "*.s2.example.com"
  ],
  "certificate_authority": "test-intermediate-CA",
  "certificate_template": "test-certificate-template",
  "common_name": "example.com",
  "created_at": "2022-10-02T14:08:07Z",
  "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
  "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
  "custom_metadata": {
    "metadata_custom_key": "metadata_custom_value"
  },
  "description": "Extended description for this secret.",
  "downloaded": true,
  "expiration_date": "2023-03-02T15:08:37Z",
  "id": "cb7a2502-8ede-47d6-b5b6-1b7af6b6f563",
  "issuer": "example.com",
  "key_algorithm": "RSA2048",
  "labels": [
    "dev",
    "us-south"
  ],
  "locks_total": 0,
  "name": "example-private-certificate",
  "next_rotation_date": "2022-03-02T14:08:07Z",
  "rotation": {
    "auto_rotate": false,
    "interval": 1,
    "unit": "month"
  },
  "secret_data": {
    "certificate": "-----BEGIN CERTIFICATE-----\nMIIE3jCCBGSgAwIBAgIUZfTbf3adn87l5J2Q2Aw+6Vk/qhowCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----",
    "issuing_ca": "-----BEGIN CERTIFICATE-----\nMIIE3jCCBGSgAwIBAgIUZfTbf3adn87l5J2Q2Aw+6Vk/qhowCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----",
    "ca_chain": [
      "-----BEGIN CERTIFICATE-----\nMIIE3jCCBGSgAwIBAgIUZfTbf3adn87l5J2Q2Aw+6Vk/qhowCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----"
    ],
    "private_key": "-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQEAqcRbzV1wp0nVrPtEpMtnWMO6Js1q3rhREZluKZfu0Q8SY4H3\n-----END RSA PRIVATE KEY-----"
  },
  "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
  "secret_type": "private_cert",
  "serial_number": "03:e2:c6:e4:0b:7d:30:e2:e2:78:1b:b9:13:fd:f0:fc:89:dd",
  "signing_algorithm": "SHA256-RSA",
  "state": 1,
  "state_description": "active",
  "updated_at": "2022-03-02T14:08:37Z",
  "validity": {
    "not_before": "2022-03-02T15:08:37Z",
    "not_after": "2023-03-01T00:00:00Z"
  },
  "versions_total": 1
}
```
{: screen}


### Creating private certificates with Terraform
{: #create-private-certificates-terraform}
{: terraform}

The following example shows a configuration that you can use to create a private certificate.

```terraform
    resource "ibm_sm_private_certificate" "test_private_certificate" {  
        instance_id = local.instance_id
        region = local.region
        name = "test-private-certificate"
        common_name = "my.example.com"
        certificate_template = ibm_sm_private_certificate_configuration_template.test_ca_template.name
        ttl = "90d"
    }
```
{: codeblock}


After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.
