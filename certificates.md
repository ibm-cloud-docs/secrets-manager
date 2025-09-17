---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-17"

keywords: import certificates, order certificates, request certificates, ssl certificates, tls certificates, imported certificates

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


# Importing SSL/TLS certificates
{: #certificates}


You can use {{site.data.keyword.secrets-manager_full}} to import SSL/TLS certificates that you can use for your apps or services.
{: shortdesc}

An SSL/TLS certificate is a type of digital certificate that is used to establish communication privacy between a server and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information that is used to create trusted and secure connections between endpoints. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

In {{site.data.keyword.secrets-manager_short}}, certificates that you import to the service are imported certificates (`imported_cert`). Certificates that you order through {{site.data.keyword.secrets-manager_short}} from a third-party certificate authority are [public certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates) (`public_cert`). Certificates that you create by using a private certificate authority are [private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates) (`private_cert`).
{: note}

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

## Before you begin
{: #before-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

Before you import a certificate, be sure that you:  
- Create an X.509 compliant certificate with a matching private key (optional).
- Convert your files into Privacy-enhanced electronic mail (PEM) format. 
- Keep the private key unencrypted to ensure that it can be imported into {{site.data.keyword.secrets-manager_short}}.


## Importing your existing certificates
{: #import-certificates}

You can use {{site.data.keyword.secrets-manager_short}} to store certificate files that are signed and issued by external certificate authorities. After you import your certificate files, you can deploy the certificate to your apps and services, download the certificate, or [rotate it manually](/docs/secrets-manager?topic=secrets-manager-manual-rotation) when it's time to renew. 

### Importing certificates in the UI
{: #import-certificates-ui}
{: ui}

You can import an existing certificate by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Click **Next**.
5. Add a name and description to easily identify your secret.
6. Select the secret group that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Optional: Add labels to help you to search for similar secrets in your instance.
8. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
9. Click **Next**.
10. Select the **Import a certificate** tile. 
11. Select a certificate file or enter its value.

    You can store unexpired X.509 certificate files that are in PEM format. If you're working with certificates that are in a different format, you can use command-line utilities to convert your certificates to `.pem`. For more information, see [Why can't I import my certificate?](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem)

12. Optional: Select a private key file or enter its value.

    If you choose to store a private key, ensure that it matches to your certificate. The private key must be unencrypted before you can import it to the service.

13. Optional: Select an intermediate certificate file or enter its value.
14. Click **Next**.
15. Review the details of your certificate. 
16. Click **Add**.


### Importing certificates from the CLI
{: #import-certificates-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To import a certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command imports a certificate along with its private key and intermediate certificate.

You can import certificate files that are in the `.pem` format. Be sure to [convert your PEM files to single-line format](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem) so that they can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} CLI.
{: note}


```sh
certificate=$(cat cert.pem)
private_key=$(cat key.pem)

ibmcloud secrets-manager secret-create --secret-name example-imported-cert-secret --secret-type imported_cert --imported-cert-certificate ${certificate} --imported-cert-private-key ${private_key}
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).


### Importing certificates with the API
{: #import-certificates-api}
{: api}

You can import certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to import an existing certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

You can import certificate files that are in the `.pem` format. Be sure to [convert your PEM files to single-line format](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem) so that they can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} API.
{: note}


```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
            "name": "example-imported-certificate",
            "description": "description of my imported certificate.",
            "secret_type": "imported_cert",
            "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
            "labels": [
                "dev",
                "us-south"
            ],
            "certificate": "-----BEGIN CERTIFICATE-----\nMIIE3jCCBGSgAwIBAgIUZfTbf3adn87l5J2Q2Aw+6Vk/qhowCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----",
            "intermediate": "-----BEGIN CERTIFICATE-----\nMIIE3DCCBGKgAwIBAgIUKncnp6BdSUKAFGBcP4YVp/gTb7gwCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----",
            "private_key": "-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQEAqcRbzV1wp0nVrPtEpMtnWMO6Js1q3rhREZluKZfu0Q8SY4H3\n-----END RSA PRIVATE KEY-----",
            "custom_metadata": {
                "metadata_custom_key": "metadata_custom_value"
            },
            "version_custom_metadata": {
                "custom_version_key": "custom_version_value"
            }
        }' \ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.


### Importing certificates with Terraform
{: #import-certificates-terraform}
{: terraform}

You can import certificates programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.

The following example shows a query that you can use to import an existing certificate.

```terraform
    resource "ibm_sm_imported_certificate" "sm_imported_certificate" {
        instance_id = local.instance_id
        region = local.region
        name = "test-imported-certificate"
        secret_group_id = "default"
        certificate  = file("path_to_certificate_file")
        intermediate = file("path_to_intermediate_certificate_file")
        private_key  = file("path_to_private_key_file")
    }
```
{: codeblock}

## Creating a Certificate Signing Request 
{: #creating-csr}

You can use an Imported Certificate secret to generate a Certificate Signing Request (CSR) that you can then externally sign with your Certificate Authority and add it back to your Imported Certificate secret.  

The imported certificate secret is created in **Pre-activation** state. To activate it you should download the CSR file, sign it with your certificate authority and add the signed certificate back as a new secret version.  
{: important}

### Creating certificate signing requests in the UI 
{: #create-csr-ui}
{: ui}

You can create a certificate signing request by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Imported certificate** tile.
5. Click **Next**.
6. Add a name and description to easily identify your secret.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Add metadata to your secret or to a specific version of your secret.

    Upload a file or enter the metadata and the version metadata in JSON format.
10. Click **Next**.
11. Select the **Create a Certificate Signing Request (CSR)** tile. 
12. Add a **common name**. 
13. Optional: add subject fields.
14. Optional: Select a **key algorithm**. If not selected, RSA is used as default.
15. Optional: select a certificate role. 
16. Click **Next**.
17. Review the details of your certificate signing request. 
18. Click **Add**. 

### Updating and downloading the certificate signing requests in the UI 
{: updating-downlading-csr-ui}
{: ui}

1. In the row for the secret that you want to update, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) > **Details**.
2. In the **Details** page use the **Managed CSR** tab to edit the certificate signing request fields.  

     When updating the common name or subject alternative names, keep at least one of the names unchanged to maintain the certificate subject consistent.
     {: note}

3. Click Update.  
4. Open the secret the Managed CSR tab and click **Download CSR** to download the certificate signing request file.  

     With the certificate signing request file in hand, you should approach your signing certificate authority and sign your CSR file to obtain the signed certificate.
     {: important}
 

### Adding the signed certificate back in the UI 
{: adding-back-signed-certificate-ui}
{: ui}

Add the signed certificate file back to your secret in your {{site.data.keyword.secrets-manager_short}} instance using the process described in [Manually rotating imported certificates with managed CSR](/docs/secrets-manager?topic=secrets-manager-manual-rotation&interface=ui#manual-rotate-imported-cert-ui-csr).

### Creating certificate signing requests with the API
{: #create-csr-api}
{: api}

You can create a certificate signing request programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.  
When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
            "name": "example-csr",
            "description": "description of my csr.",
            "secret_type": "imported_cert",
            "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
            "labels": [
                "dev",
                "us-south"
            ],
            "managed_csr": {
              "require_cn": true,
              "common_name": "example.com",
              "ip_sans": "127.0.0.1",
              "uri_sans": "https://www.example.com/test",
              "other_sans": "2.5.4.5;UTF8:*.example.com",
              "exclude_cn_from_sans": false
            },
            "custom_metadata": {
                "metadata_custom_key": "metadata_custom_value"
            },
            "version_custom_metadata": {
                "custom_version_key": "custom_version_value"
            }
        }' \ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.


### Updating and downloading the certificate signing requests with the API
{: updating-downlading-csr-api}
{: api}

You can update a certificate signing request programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.  
When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
            "name": "example-csr",
            "description": "description of my csr.",
            "secret_type": "imported_cert",
            "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
            "labels": [
                "dev",
                "us-south"
            ],
            "managed_csr": {
              "alt_names": "alt1"
            }
        }' \ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.


### Adding the signed certificate back with the API
{: #adding-back-signed-certificate-api}
{: api}

Add the signed certificate file back to your secret in your {{site.data.keyword.secrets-manager_short}} instance using the process described in [Manually rotating imported certificates](/docs/secrets-manager?topic=secrets-manager-manual-rotation&interface=api#manual-rotate-imported-cert-api-csr).


### Creating certificate signing requests from the CLI
{: #create-csr-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To create a certificate signing request by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command creates a certificate signing request with the `--imported-cert-managed-csr` option to add the field `common_name` with value **example.com**. Review the [{{site.data.keyword.secrets-manager_short}} API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret) for a full list of supported optional subject fields.

```
ibmcloud secrets-manager secret-create --secret-name example-imported-cert-csr-secret --secret-type imported_cert --imported-cert-managed-csr '{"common_name":"example.com"}'
```

### Updating and downloading the certificate signing requests from the CLI
{: #updating-downlading-csr-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To update a certificate signing request by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-metadata-update`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command) command. For example, the following command updates a certificate signing request with the `--imported-cert-managed-csr` option to add the field `alt_names` with value **alt1**. Review the [{{site.data.keyword.secrets-manager_short}} API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret) for a full list of supported optional subject fields.

```
ibmcloud secrets-manager secret-metadata-update --id SECRET_ID --imported-cert-managed-csr '{"alt_names":"alt1"}'
```

To get the certificate signing request by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-metadata`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-metadata-command) command.

```
ibmcloud secrets-manager secret-metadata --id SECRET_ID
```

### Adding the signed certificate back from the CLI
{: #adding-back-signed-certificate-cli}
{: cli}

Add the signed certificate file back to your secret in your {{site.data.keyword.secrets-manager_short}} instance using the process described in [Manually rotating imported certificates](/docs/secrets-manager?topic=secrets-manager-manual-rotation&interface=cli#manual-rotate-imported-certificates-cli-csr).
