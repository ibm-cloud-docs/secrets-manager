---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-29"

keywords: root certificate authority, root CA, internal signing, external signing

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

# Creating root certificate authorities
{: #root-certificate-authorities}

With {{site.data.keyword.secrets-manager_full}}, you can build and manage your own public key infrastructure (PKI) system by creating root and intermediate certificate authorities (CA).
{: shortdesc}

A certificate authority (CA) is the entity that signs and issues your SSL/TLS certificates. If you're looking for the ability to quickly generate a self-signed certificate, you can use {{site.data.keyword.secrets-manager_short}} to create an internally signed root CA. You can use this CA as the trust anchor for a certificates chain. After you create a root CA for your instance, you can use it to sign a lower-level or subordinate CA, for example other intermediate CAs that you create in the service.

You can create up to 10 root certificate authorities per instance. To view a list of the configurations that are available for your instance, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.
{: note}

## Before you begin
{: #before-root-ca}

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Creating a root certificate authority in the UI
{: #root-ca-ui}
{: ui}

You can create an internally signed root certificate authority for your service instance by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets engines** page, click the **Private certificates** tab.
4. In the Certificate authorities table, click **Create certificate authority** to start the creation wizard.
5. Specify the certificate authority type and options.
   1. Select **Root certificate authority** as the authority type.
   2. Enter a name to easily identify your certificate authority.
   3. Select a maximum time-to-live (TTL) for the certificate to be generated for this CA. The TTL determines how long the CA certificate remains valid.
   4. Select the maximum number of subordinate or lower-level intermediate certificates that can exist in the chain.
   5. To encode the issuing CA certificate URL into intermediate CA certificates, set the **Encode URL** option to **Enabled**.

6. Enter the subject name fields for your root CA certificate.
7. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
8.  Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

    A CRL is a list of certificates that are revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
    1. To build a CRL for your root CA with each certificate request, set the **CRL building** option to **Enabled**.
    2. To encode the URL of the revocation list in the root CA certificate, set the **CRL distribution points** option to **Enabled**.
    3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.
9.  Review your selections. To create the root CA, click **Create**.

    You can now select this root CA when you [create an intermediate CA with internal signing](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities#intermediate-ca-internal-signing-ui). To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.

## Creating a root certificate authority with the API
{: #root-ca-api}
{: api}

You can create an internally signed root certificate authority for your service instance by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a root certificate authority.
{: curl}



```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/config/public_cert/certificate_authorities' \
-H 'Authorization: Bearer {IAM_token}' \
-H 'Content-Type: application/json' \
-d' {
      "name": "test-root-CA",
      "type": "root_certificate_authority",
      "config": {
         "max_ttl": "43830h",
         "common_name": "example.com",
         "crl_disable": false,
         "crl_distribution_points_encoded": true,
         "issuing_certificates_urls_encoded": true
      }
}'
```
{: codeblock}
{: curl}



A successful response adds the configuration to your service instance. 

```json
{
    "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.config+json",
        "collection_total": 1
    },
    "resources": [
        {
            "config": {
                "common_name": "example.com",
                "country": [],
                "crl_disable": false,
                "crl_distribution_points_encoded": true,
                "crl_expiry": 259200,
                "data": {
                    "certificate": "-----BEGIN CERTIFICATE-----\nMIIDNTCCAh2gAwIBAgIUC68iMvjVRDZ4...(truncated)",
                    "expiration": 1651603608,
                    "issuing_ca": "-----BEGIN CERTIFICATE-----\nMIIDNTCCAh2gAwIBAgIUC68iMvjVRDZ4c...(truncated)",
                    "serial_number": "0b:af:22:32:f8:d5:44:36:78:72:2b:cc:b8:0b:bf:39:8d:e0:06:40"
                },
                "exclude_cn_from_sans": false,
                "expiration_date": "2022-05-03T00:00:00Z",
                "format": "pem",
                "issuing_certificates_urls_encoded": true,
                "key_bits": 2048,
                "key_type": "rsa",
                "locality": [],
                "max_path_length": -1,
                "max_ttl": 157788000,
                "organization": [],
                "other_sans": [],
                "ou": [],
                "permitted_dns_domains": [],
                "postal_code": [],
                "private_key_format": "der",
                "province": [],
                "status": "configured",
                "street_address": [],
                "ttl": 0
            },
            "name": "test-root-CA",
            "type": "root_certificate_authority"
        }
    ]
}
```
{: screen}

For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.

## Next steps
{: #root-ca-next-steps}

- [Create an intermediate certificate authority](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities)

