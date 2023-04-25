---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-25"

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



# Adding SSL/TLS certificates
{: #certificates}



You can use {{site.data.keyword.secrets-manager_full}} to store, request, and generate SSL/TLS certificates that you can use for your apps or services.
{: shortdesc}

An SSL/TLS certificate is a type of digital certificate that is used to establish communication privacy between a server and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information that is used to create trusted and secure connections between endpoints. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

In {{site.data.keyword.secrets-manager_short}}, certificates that you import to the service are imported certificates (`imported_cert`). Certificates that you order through {{site.data.keyword.secrets-manager_short}} from a third-party certificate authority are public certificates (`public_cert`). Certificates that you create by using a private certificate authority are private certificates (`private_cert`).
{: note}

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)


## Before you begin
{: #before-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

| Prerequisites |
| :------------ |
| Before you import a certificate, be sure that you:  \n  \n - Create an X.509 compliant certificate with a matching private key (optional). \n - Convert your files into Privacy-enhanced electronic mail (PEM) format. \n - Keep the private key unencrypted to ensure that it can be imported into {{site.data.keyword.secrets-manager_short}}. |
{: caption="Table 1. Prerequisites - Importing certificates" caption-side="top"}
{: #import-certificates-prereqs}
{: tab-title="Importing certificates"}
{: tab-group="cert-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| Before you order a certificate, be sure that you:  \n  \n - [Prepare your instance to order certificates](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates).  \n - Review the certificate authority and DNS provider configurations that are available. To view the configurations that are defined for your instance, go to the **Secrets engines > Public certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.  \n  \n To work with a DNS provider that is not currently integrated with the service, you do not need to add a configuration to order your public certificate.  |
{: caption="Table 1. Prerequisites - Ordering public certificates" caption-side="top"}
{: #order-certificates-prereqs}
{: tab-title="Ordering public certificates"}
{: tab-group="cert-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| Before you create a certificate, be sure that you:  \n  \n - [Prepare your instance to create certificates](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates).  \n - Review the intermediate certificate authorities that are available. To view the configurations that are defined for your instance, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.  \n - Review the [certificate templates](/docs/secrets-manager?topic=secrets-manager-certificate-templates) that are available for your selected intermediate CA.  \n A template selection is required when you create a private certificate. Depending on the template that you choose, some restrictions for your private certificate might apply. To view the templates that are defined for your intermediate CA, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI. Expand the row of the intermediate CA that you want to use as the issuing authority for private certificate, and click **Templates** to review the templates that are available. |
{: caption="Table 1. Prerequisites - Creating private certificates" caption-side="top"}
{: #create-certificates-prereqs}
{: tab-title="Creating private certificates"}
{: tab-group="cert-prereqs"}
{: class="simple-tab-table"}


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
4. From the list of secret types, click the **TLS certificates** tile.
5. Add a name and description to easily identify your certificate.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a certificate file or enter its value.

    You can store unexpired X.509 certificate files that are in PEM format. If you're working with certificates that are in a different format, you can use command-line utilities to convert your certificates to `.pem`. For more information, see [Why can't I import my certificate?](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem)

8. Optional: Select a private key file or enter its value.

    If you choose to store a private key, ensure that it matches to your certificate. The private key must be unencrypted before you can import it to the service.

9. Optional: Select an intermediate certificate file or enter its value.
10. Optional: Add labels to help you to search for similar secrets in your instance.
11. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format. 
12. Click **Import**.

### Importing certificates from the CLI
{: #import-certificates-cli}
{: cli}

To import a certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command imports a certificate along with its private key and intermediate certificate.

You can import certificate files that are in the `.pem` format. Be sure to [convert your PEM files to single-line format](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem) so that they can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} CLI.
{: note}


```sh
ibmcloud secrets-manager secret-create \    
    --resources='[
        {
            "name": "example-imported-certificate", 
            "description": "Extended description for my secret.", 
            "certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)",
            "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)","intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...", 
            "custom_metadata": {
                "anyKey": "anyValue"
                }, 
            "version_custom_metadata": {
                "anyKey": "anyValue"
                }
            }
        ]'
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


### Importing certificates with the API
{: #import-certificates-api}
{: api}

You can import certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to import an existing certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}

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

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.


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


## Ordering public certificates
{: #order-certificates}
{: ui}

After you [configure the public certificates engine](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates) for your instance, you can use {{site.data.keyword.secrets-manager_short}} to request public SSL/TLS certificates from Let's Encrypt. Before a certificate can be issued to you, {{site.data.keyword.secrets-manager_short}} uses domain validation to verify the ownership of your domains. When you order a certificate:

- {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. The status of the certificate changes to **Pre-activation** to indicate that your request is being processed.
- If the validation completes successfully, your certificate is issued and its status changes to **Active**.
- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.
    {: ui}

- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.
    {: cli}

- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. You can use the [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata) API to check the `resources.issuance_info` field for issuance details on your certificate.
    {: api}

- After the certificate is issued, you can deploy it to your integrated apps, download it, or modify its rotation options. 


### Ordering public certificates with integrated DNS providers in the UI
{: #order-certificates-ui}
{: ui}

You can order a certificate by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **TLS certificates** tile.
5. Click the **Order a public certificate** tile.
6. Add a name and description to easily identify your certificate.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

   Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Select a certificate authority configuration.

   The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.
9. Select the key algorithm to be used to generate the public key for your certificate.

   The key algorithm that you select determines the encryption algorithm (`RSA` or `ECDSA`) and key size to use to generate keys and sign certificates. For longer living certificates, it is recommended to use longer key lengths to provide more encryption protection. Options include `RSA2048`, `RSA4096`, `ECDSA256`, and `ECDSA384`.
10. Optional: Add labels to help you to search for similar secrets in your instance.
11. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format. 
12. Optional: Enable advanced options for the certificate.
    1. To bundle your issued certificate with intermediate certificates, switch the bundle toggle to **On**. After your certificates are bundled, they can no longer be unbundled.
    2. To enable automatic rotation for the certificate, switch the rotation toggle to **On**. Your certificate is rotated 31 days before it expires.
    3. To request a new private key with the certificate on each rotation, switch the rekey toggle to **On**.
13. Select a DNS provider configuration.

    The configuration that you select determines the DNS provider to validate the ownership of your domains. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.  
14. Add the domains to include in your request.

    You can include up to 100 domains, subdomains, or wildcards. The Common Name, or fully qualified domain name of the certificate, can't exceed 64 characters in length. A wildcard can be selected as the Common Name.

    1. Click **Select domains**.
    2. From your list of domains, select the Common Name of the certificate.
15. Click **Order**.

    When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete. After you submit your certificate details, {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. Your private key for SSL/TLS is generated directly in {{site.data.keyword.secrets-manager_short}} and stored securely.
    
    Need to check your order status? From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.


### Ordering public certificates with integrated DNS providers from the CLI
{: #order-certificates-cli}
{: cli}

To order a public certificate with an integrated DNS provider by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command.For example, the following command requests a public certificate secret from the certificate authority that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
ibmcloud secrets-manager secret-create --resources=
'[{
    "name": "example-public-certificate", 
    "description": "Extended description for this secret.", 
    "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
    "labels": [
        "dev","us-south"
    ], 
    "dns": "dns_provider",
    "common_name": "cert_common_name"
    "alt_names": [
        "alt_name1", "alt_name2"
    ],
    "algorithm": "sha256WithRSAEncryption",
    "key_algorithm": "rsaEncryption 2048 bit",
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



The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


### Ordering public certificates with integrated DNS providers by using the API
{: #order-certificates-api}
{: api}

You can order certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to order a certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
            "name": "example-public-certificate",
            "description": "Description of my public certificate",
            "secret_type": "public_cert",
            "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
            "labels": [
                "dev",
                "us-south"
            ],
            "common_name": "example.com",
            "alt_names": [
                "s1.example.com",
                "*.s2.example.com"
            ],
            "ca": "lets-encrypt-config",
            "dns": "cloud-internet-services-config",
            "rotation": {
                "auto_rotate": true,
                "rotate_keys": true
            },
            "bundle_certs": true,
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



When you submit your certificate details, {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. Your private key for SSL/TLS is generated directly in {{site.data.keyword.secrets-manager_short}} and stored securely. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

Need to check your order status? Use the [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata) API to check the `resources.issuance_info` field for issuance details on your certificate.
{: tip} 

### Ordering public certificates with integrated DNS providers by using Terraform
{: #order-certificates-terraform}
{: terraform}

The following example shows a configuration that you can use to order a public certificate.

```terraform
    resource "ibm_sm_public_certificate" "sm_public_certificate" {
        instance_id = local.instance_id
        region = local.region
        name = "test-public-certificate"
        secret_group_id = "default"
        ca = ibm_sm_public_certificate_configuration_ca_lets_encrypt.my_lets_encrypt_config.name
        dns = ibm_sm_public_certificate_configuration_dns_cis.my_cis_dns_config.name
        rotation {
            auto_rotate = true
            rotate_keys = false
        }
    }
```

{: codeblock}

### Ordering public certificates with your own DNS provider in the UI
{: #order-certificates-manual-ui}
{: ui}

To create a public certificate by using a manual DNS provider in the UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **TLS certificates** tile.
5. Click the **Order a public certificate** tile.
6. Add a name and description to easily identify your certificate.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

   Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Select a certificate authority.

   The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.
9. Select the key algorithm to be used to generate the public key for your certificate.

   The key algorithm that you select determines the encryption algorithm (`RSA` or `ECDSA`) and key size to use to generate keys and sign certificates. For longer living certificates, it is recommended to use longer key lengths to provide more encryption protection. Options include `RSA2048`, `RSA4096`, `ECDSA256`, and `ECDSA384`.
10. Optional: Add labels to help you to search for similar secrets in your instance.
11. Optional: Enable advanced options for the certificate.
    1. To bundle your issued certificate with intermediate certificates, switch the bundle toggle to **On**. After your certificates are bundled, they can no longer be unbundled.
12. Select **Manual** as your DNS provider.
13. Add the domains to include in your request.

    You can include up to 100 domains, subdomains, or wildcards. The Common Name, or fully qualified domain name of the certificate, can't exceed 64 characters in length. A wildcard can be selected as the Common Name.

    1. In the **Common name** section, from your list of domains, select the Common Name of the certificate.
14. Click **Order**.
15. Check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**. 
16. Click **Challenges** to access the TXT record name and value that are associated with each of your domains. You need them to complete the challenges.
17. To validate the ownership of your domains, manually add the TXT records that are provided for each of your domains to your DNS provider account. You must address only the challenges that are not validated before the expiration date. 

    If you order a certificate for subdomains, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
    {: note}

18. Verify that the TXT records that you added to your domains are propagated. Depending on your DNS provider, it can take some time to complete.
19. After you confirm that the records are propagated, click **Validate** to request Let's Encrypt to validate the challenges to your domains and create a public certificate. 

    If the order fails because the TXT records were not successfully propagated, you must start a new order to proceed. 
    {: note}

20. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.


### Ordering public certificates with your own DNS provider by using the API
{: #order-certificates-manual-api}
{: api}

To create a public certificate by using a manual DNS provider, complete the following steps.

1. Create a certificate authority (CA) configuration by following the steps that are defined in [Adding a CA configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority&interface=ui).
2. Create a new public certificate by specifying `manual` as your DNS configuration.


    ```sh
    curl -X POST 
        -H "Authorization: Bearer {iam_token}" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d '{ 
                "name": "example-public-certificate",
                "description": "description of my public certificate",
                "secret_type": "public_cert",
                "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
                "labels": [
                    "dev",
                    "us-south"
                ],
                "common_name": "example.com",
                "alt_names": [
                    "s1.example.com",
                    "*.s2.example.com"
                ],
                "ca": "lets-encrypt-config",
                "dns": "manual",
                "rotation": {
                    "auto_rotate": true,
                    "rotate_keys": true
                },
                "bundle_certs": true,
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


   Example response:

   ```json
   "metadata": {
      "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
      "collection_total": 1
    },
   "resources": [
    {
      "alt_names": [
        "domain2",
        "domain3"
      ],
      "common_name": "domain1",
      "created_by": "User",
      "creation_date": "2022-09-13T06:21:33Z",
      "crn": "secret crn",
      "description": "Description for ordered certificate.",
      "downloaded": false,
      "id": "38747ae6-8c69-d745-5276-cdf3157b9021",
      "issuance_info": {
      "auto_rotated": false,
      "bundle_certs": false,
      "ca": "ca_config_name",
      "challenges": [
        {
            "domain": "domain1",
            "expiration": "2022-09-20T06:21:36Z",
            "status": "pending",
            "txt_record_name": "_acme-challenge.domain1.",
            "txt_record_value": "TA6J7fFYrwP3Jg-S_IAQSj2Ydqfw4Ycm4sMwlzuCcxk"
        },
            {
               "domain": "domain2",
               "expiration": "2022-09-20T06:21:36Z",
               "status": "pending",
               "txt_record_name": "_acme-challenge.domain2.",
               "txt_record_value": "qSDrCkFAViX4xANKuEPcMNairWm1PUtROm6kp9bmSS0"
            },
            {
               "domain": "domain3",
               "expiration": "2022-09-20T06:21:36Z",
               "status": "pending",
               "txt_record_name": "_acme-challenge.domain3.",
               "txt_record_value": "8dcgan91fW6aK3aIhPAVZRkHpbYEoMcCNPpVh1n4tSA"
            }
         ],
      "dns": "manual",
      "ordered_on": "2022-09-13T06:21:33Z",
      "state": 0,
      "state_description": "Pre-activation"
      },
      "key_algorithm": "RSA2048",
      "labels": [],
      "last_update_date": "2022-09-13T06:21:33Z",
      "locks_total": 0,
      "name": "my-public-certificate",
      "rotation": {
      "auto_rotate": false,
      "rotate_keys": false
      },
      "secret_type": "public_cert",
      "state": 0,
      "state_description": "Pre-activation",
      "versions": [],
      "versions_total": 1
      }
   ] 
   ```
   {: screen}


3. Complete the challenges that are marked as `pending` before they expire by adding the TXT records that are specified in the challenge to your domain in your DNS provider account to verify your ownership of the domain.

   If you order a certificate for subdomains, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
   {: note}


4. Validate that the TXT records that you added are propagated. Depending on your DNS provider, it can take some time to complete.


5. After the records are propagated, call the {{site.data.keyword.secrets-manager_short}} [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) API to request Let's Encrypt to validate the challenges to your domain and create a public certificate. 

   ```sh
    curl -X POST 
    --header "Authorization: Bearer {iam_token}" 
    --header "Accept: application/json" 
    --header "Content-Type: application/json" 
    --data '{ 
        "action_type": "public_cert_action_validate_dns_challenge"
    }'\ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/v2/secrets/{id}/actions"
   ```
   {: codeblock}
   {: curl}


   If you need to update your certificate later, you can use the [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) API but with the action `rotate`. However, you can't automatically rotate manual DNS provider certificates in {{site.data.keyword.secrets-manager_short}}.
   {: note}


6. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.


Want to automate the creation of your public certificates? If your domains are configured through a DNS provider, you can create a script to complete the challenges. Some DNS providers offer an API that checks whether the new records are fully transmitted. If your DNS provider doesn't offer this option, you can configure your client to wait for a specified amount of time, sometimes up to an hour. In {{site.data.keyword.secrets-manager_short}}, after you call `validate-dns-challenges`, you can check the status of the certificate issuance by obtaining your certificate metadata. When the `IssuanceInfo.State` field that is returned changes to `active`, the certificate is issued. 
{: tip}


### Ordering public certificates with your own DNS provider by using the CLI
{: #order-certificates-manual-cli}
{: cli}

To order a public certificate with your own DNS provider by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command requests a public certificate secret from the certificate authority that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}



```sh
ibmcloud secrets-manager secret-create --resources=
'[{
    "name": "example-public-certificate", 
    "description": "Extended description for this secret.", 
    "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
    "labels": [
        "dev","us-south"
    ], 
    "dns": "manual",
    "common_name": "cert_common_name"
    "alt_names": [
        "alt_name1", "alt_name2"
    ],
    "algorithm": "sha256WithRSAEncryption",
    "key_algorithm": "rsaEncryption 2048 bit",
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



The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


## Creating private certificates
{: #create-certificates}

After you [configure the private certificates engine](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates) for your instance, you can use {{site.data.keyword.secrets-manager_short}} to generate private certificates by using an internal certificate authority that was previously configured for your {{site.data.keyword.secrets-manager_short}} service instance. Before a certificate can be issued to you, {{site.data.keyword.secrets-manager_short}} checks to ensure that your certificate request matches the restrictions that are defined for the [certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates) that you select. After a certificate is issued, you can deploy it to your integrated apps, download it, revoke it, or rotate it manually.


### Creating private certificates in the UI
{: #create-certificates-ui}
{: ui}

You can create a private certificate by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **TLS certificates** tile.
5. Click the **Create private certificate** tile.
6. Add a name and description to easily identify your certificate.
7. Select a certificate authority configuration.

    The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Private certificates**.
   
8. Select a [certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates).

    The template that you select determines the parameters to be applied to your generated certificate. To view the details of the certificate templates that are defined for your selected certificate authority, you can go to **Secrets engines > Private certificates**. From the list of certificate authorities, expand the row of the CA that you want to use as the issuing authority for your private certificate, and click **Templates**.

9. Optional: Add labels to help you to search for similar secrets in your instance.

10. Optional: Enable automatic rotation for the certificate.

    To enable automatic rotation, switch the rotation toggle to **On**. Select an interval and unit that specifies the number of days between scheduled rotations.

    Depending on the certificate template that you choose in the following steps, some restrictions on the rotation interval for your private certificate might apply. For example, the rotation interval can't exceed the time-to-live (TTL) that is defined in the template. For more information, see [Certificate templates](/docs/secrets-manager?topic=secrets-manager-certificate-templates).
    {: note}

11. Select the [secret group](#x9968962){: term} that you want to assign to the certificate.

    If your selected certificate template allows certificates to be added to specific secret groups, only those allowed groups are listed. If the template has no restrictions, you can create a secret group if you don't already have one. Your certificate is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).

12. Required: Specify a common name for your certificate.

    Depending on the certificate template that you choose, some restrictions on the common name might apply. To view the details of your selected certificate template, you can go to **Secrets engines > Private certificates**. From the list of certificate authorities, expand the row of the CA that you want to use as the issuing authority for your private certificate, and click **Templates**.
13. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format.    
14. Click **Create**.

    After a certificate is issued, you can deploy it to your integrated apps, download it, revoke it, or rotate it manually. Your private key for SSL/TLS is generated directly in {{site.data.keyword.secrets-manager_short}} and stored securely.


### Creating private certificates from the CLI
{: #generate-certificates-cli}
{: cli}

To create a private certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command creates a private certificate secret from the certificate template that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
ibmcloud secrets-manager secret-create --resources=
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


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).



### Creating private certificates with the API
{: #create-certificates-api}
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


Need to create a private certificate with advanced options? You can use optional request parameters to specify advanced attributes for your private certificate, such as Subject Alternative Names or a time-to-live (TTL). If you omit these optional parameters, the attributes that are defined for your selected certificate template are applied. For more information, see the [API reference](/apidocs/secrets-manager#create-secret).
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
{: #create-certificates-terraform}
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


After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager#create-secret){: external}.


