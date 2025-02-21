---

copyright:
  years: 2021, 2025
lastupdated: "2025-02-21"

keywords: import certificates, order certificates, request certificates, ssl certificates, tls certificates, public certificates

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

# Ordering SSL/TLS public certificates
{: #public-certificates}

You can use {{site.data.keyword.secrets-manager_full}} to store, request, and generate public SSL/TLS certificates that you can use for your apps or services.
{: shortdesc}

An SSL/TLS certificate is a type of digital certificate that is used to establish communication privacy between a server and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information that is used to create trusted and secure connections between endpoints. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

In {{site.data.keyword.secrets-manager_short}}, Certificates that you order through {{site.data.keyword.secrets-manager_short}} from a third-party certificate authority are public certificates. Certificates that you import to the service are [imported certificates](/docs/secrets-manager?topic=secrets-manager-certificates) (`imported_cert`). Certificates that you create by using a private certificate authority are [private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates) (`private_cert`).
{: note}


## Before you begin
{: #before-public-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

Before you order a certificate, be sure that you: 
- [Prepare your instance to order certificates](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates). 
- Review the certificate authority and DNS provider configurations that are available. To view the configurations that are defined for your instance, go to the **Secrets engines > Public certificates** page in the {{site.data.keyword.secrets-manager_short}} UI. 

To work with a DNS provider that is not currently integrated with the service, you do not need to add a configuration to order your public certificate.  

## Ordering public certificates
{: #order-public-certificates}

After you [configure the public certificates engine](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates) for your instance, you can use {{site.data.keyword.secrets-manager_short}} to request public SSL/TLS certificates from Let's Encrypt. Before a certificate can be issued to you, {{site.data.keyword.secrets-manager_short}} uses domain validation to verify the ownership of your domains. When you order a certificate:

- {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. The status of the certificate changes to **Pre-activation** to indicate that your request is being processed.
- If the validation completes successfully, your certificate is issued and its status changes to **Active**.

- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.
{: ui}

- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.
{: cli}

- If the validation doesn't complete successfully, the status of your certificate changes to **Deactivated**. You can use the [Get secret metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-metadata) API to check the `resources.issuance_info` field for issuance details on your certificate.
{: api}

- After the certificate is issued, you can deploy it to your integrated apps, download it, or modify its rotation options. 


### Ordering public certificates with integrated DNS providers in the UI
{: #order-public-certificates-ui}
{: ui} 

You can order a certificate by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Click the **Order public certificate** tile.
5. Add a name and description to easily identify your certificate.
6. Select the secret group that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).

7. Optional: Add labels to help you to search for similar secrets in your instance.
8. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
9. Click **Next**.
9. Select a certificate authority configuration.

    The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.

10. Select the key algorithm to be used to generate the public key for your certificate.

    The key algorithm that you select determines the encryption algorithm (`RSA` or `ECDSA`) and key size to use to generate keys and sign certificates. For longer living certificates, it is recommended to use longer key lengths to provide more encryption protection. Options include `RSA2048`, `RSA4096`, `ECDSA256`, and `ECDSA384`.

11. Optional: Enable advanced options for the certificate.

    1. To bundle your issued certificate with intermediate certificates, switch the bundle toggle to **On**. After your certificates are bundled, they can no longer be unbundled. If you choose not to bundle the certificates, this cannot be changed afterwards, only by creating a new secret.
    2. To enable automatic rotation for the certificate, switch the rotation toggle to **On**. Your certificate is rotated 31 days before it expires.
    3. To request a new private key with the certificate on each rotation, switch the rekey toggle to **On**.
   
12. Select a DNS provider configuration.

    The configuration that you select determines the DNS provider to validate the ownership of your domains. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.  

13. Add the domains to include in your request.

    1. Click **Select domains**.
    2. From your list of domains, select the Common Name of the certificate.
   
       The Common Name is an optional input. If the Common Name is not explicitly specified, Let's encrypt automatically assigns the first alt name that is not longer than 64 chars as the Common Name. If no such alt name is found the certificate will be issued without a Common Name.
       {: note}

       You can optionally also manually add valid domains using the **Add domains manually** field.
       {: note}
    
13. Click **Next**.
14. Review the details of your certificate.
15. Click **Add**.

    When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete. After you submit your certificate details, {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. Your private key for SSL/TLS is generated directly in {{site.data.keyword.secrets-manager_short}} and stored securely.
    
    Need to check your order status? From your Secrets table, you can check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**.


### Ordering public certificates with integrated DNS providers from the CLI
{: #order-public-certificates-integrated-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To order a public certificate with an integrated DNS provider by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command.For example, the following command requests a public certificate secret from the certificate authority that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
ibmcloud secrets-manager secret-create --secret-prototype=
'{
    "name": "example-public-certificate", 
    "description": "Extended description for this secret.",
    "secret_type": "public_cert",
    "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
    "labels": [
        "dev","us-south"
    ], 
    "dns": "dns_provider",
    "common_name": "cert_common_name"
    "alt_names": [
        "alt_name1", "alt_name2"
    ],
    "ca": "lets-encrypt-config",
    "key_algorithm": "RSA2048",
    "rotation": {
        "auto_rotate": true,
        "rotate_keys":false
        },
    "custom_metadata" : {
        "anyKey" : "anyValue"
    },
    "version_custom_metadata" : {
        "anyKey" : "anyValue"
    }
}
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).


### Ordering public certificates with integrated DNS providers by using the API
{: #order-public-certificates-integrated-api}
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


When you submit your certificate details, {{site.data.keyword.secrets-manager_short}} sends your request to the selected certificate authority. After a certificate is issued, you can deploy it to your integrated apps, download it, or rotate it manually. Your private key for SSL/TLS is generated directly in {{site.data.keyword.secrets-manager_short}} and stored securely. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret){: external}.

Need to check your order status? Use the [Get secret metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-metadata) API to check the `resources.issuance_info` field for issuance details on your certificate.
{: tip} 


### Ordering public certificates with integrated DNS providers by using Terraform
{: #order-public-certificates-integrated-terraform}
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
{: #order-public-cert-manual-ui}
{: ui}

To create a public certificate by using a manual DNS provider in the UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Click the **Order public certificate** tile.
5. Add a name and description to easily identify your certificate.
6. Select the secret group that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).

7. Optional: Add labels to help you to search for similar secrets in your instance.
8. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format. 
9. Click **Next**.
10. Select a certificate authority configuration.

    The configuration that you select determines the certificate authority to use for signing and issuing the certificate. To view the configurations that are defined for your instance, you can go to **Secrets engines > Public certificates**.

11. Select the key algorithm to be used to generate the public key for your certificate.

    The key algorithm that you select determines the encryption algorithm (`RSA` or `ECDSA`) and key size to use to generate keys and sign certificates. For longer living certificates, it is recommended to use longer key lengths to provide more encryption protection. Options include `RSA2048`, `RSA4096`, `ECDSA256`, and `ECDSA384`.

12. Optional: Enable advanced options for the certificate.
    1. To bundle your issued certificate with intermediate certificates, switch the bundle toggle to **On**. After your certificates are bundled, they can no longer be unbundled. If you choose not to bundle the certificates, this cannot be changed afterwards, only by creating a new secret.
    2. To request a new private key with the certificate on each rotation, switch the rekey toggle to **On**.
13. Select **Manual** as your DNS provider.
14. Add the domains to include in your request.

    You can include up to 100 domains, subdomains, or wildcards. The Common Name, or fully qualified domain name of the certificate, can't exceed 64 characters in length. A wildcard can be selected as the Common Name.

    1. In the **Common name** section, from your list of domains, select the Common Name of the certificate.
15. Click **Next**.
16. Review the details of your certificate.
17. Click **Add**.
18. Check the issuance details of your certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> View details**. 
19. Click **Challenges** to access the TXT record name and value that are associated with each of your domains. You need them to complete the challenges.
20. To validate the ownership of your domains, manually add the TXT records that are provided for each of your domains to your DNS provider account. You must address only the challenges that are not validated before the expiration date. 

     If you order a certificate for a subdomain, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
     {: note}

21. Verify that the TXT records that you added to your domains are propagated. Depending on your DNS provider, it can take some time to complete.
22. After you confirm that the records are propagated, click **Validate** to request Let's Encrypt to validate the challenges to your domains and create a public certificate. 

     If the order fails because the TXT records were not successfully propagated, you must start a new order to proceed. 
     {: note}

20. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.

### Ordering public certificates with your own DNS provider by using the API
{: #order-public-cert-manual-api}
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


5. After the records are propagated, call the {{site.data.keyword.secrets-manager_short}} [Create a secret action](/apidocs/secrets-manager/secrets-manager-v2#create-secret-action) API to request Let's Encrypt to validate the challenges to your domain and create a public certificate. 

   ```sh
    curl -X POST 
    --header "Authorization: Bearer {iam_token}" 
    --header "Accept: application/json" 
    --header "Content-Type: application/json" 
    --data '{ 
        "action_type": "public_cert_action_validate_dns_challenge"
    }'\ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/actions"
   ```
   {: codeblock}
   {: curl}


   If you need to update your certificate later, you can use the [Create a secret action](/apidocs/secrets-manager/secrets-manager-v2#create-secret-action) API but with the action `rotate`. However, you can't automatically rotate manual DNS provider certificates in {{site.data.keyword.secrets-manager_short}}.
   {: note}


6. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.


Want to automate the creation of your public certificates? If your domains are configured through a DNS provider, you can create a script to complete the challenges. Some DNS providers offer an API that checks whether the new records are fully transmitted. If your DNS provider doesn't offer this option, you can configure your client to wait for a specified amount of time, sometimes up to an hour. In {{site.data.keyword.secrets-manager_short}}, after you call `validate-dns-challenges`, you can check the status of the certificate issuance by obtaining your certificate metadata. When the `IssuanceInfo.State` field that is returned changes to `active`, the certificate is issued. 
{: tip}


### Ordering public certificates with your own DNS provider by using the CLI
{: #order-public-cert-manual-cli}
{: cli}

To order a public certificate with your own DNS provider by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. For example, the following command requests a public certificate secret from the certificate authority that you specify.

When you order a certificate, domain validation takes place to verify the ownership of your selected domains. This process can take a few minutes to complete.
{: note}


```sh
ibmcloud secrets-manager secret-create --secret-prototype \
'{
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
    "ca": "lets-encrypt-config",
    "key_algorithm": "RSA2048",
    "rotation": {
        "enabled": false,
        "rotate_keys":false
        },
    "custom_metadata" : {
        "anyKey" : "anyValue"
    },
    "version_custom_metadata" : {
        "anyKey" : "anyValue"
    }
}
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Ordering public certificates with Akamai DNS provider by using Terraform
{: #order-public-certificates-akamai-terraform}
{: terraform}

To create a public certificate by using Akamai as your DNS provider, complete the following steps.

1. Create a certificate authority (CA) configuration by following the steps that are defined in [Adding a CA configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority).
2. Create a new public certificate by specifying `akamai` as your DNS configuration. 
3. Use one of the following Akamai's authentication methods. You can use an `edgerc` file or directly provide your Akamai authentication credentials.  [Learn more about Akamai's authentication credentials](https://techdocs.akamai.com/developer/docs/set-up-authentication-credentials).

	1. Provide the path to your `.edgerc` file and the relevant `config_section`.

		```terraform
			resource "ibm_sm_public_certificate" "sm_public_certificate" {
					instance_id = local.instance_id
					region = local.region
					name = "test-public-certificate"
					secret_group_id = "default"
					ca = ibm_sm_public_certificate_configuration_ca_lets_encrypt.my_lets_encrypt_config.name
					dns = “akamai”
					akamai {
						edgerc {
							path_to_edgerc = “/path/to/your/edgerc/file”
							config_section = “default”
						}
					}
					rotation {
						auto_rotate = true
						rotate_keys = false
					}
			}

      ```
      {: pre}
  
	2. Provide your Akamai's authentication credentials.

		```terraform
			resource "ibm_sm_public_certificate" "sm_public_certificate" {
					instance_id = local.instance_id
					region = local.region
					name = "test-public-certificate"
					secret_group_id = "default"
					ca = ibm_sm_public_certificate_configuration_ca_lets_encrypt.my_lets_encrypt_config.name
					dns = “akamai”
					akamai {
						config {
							client_secret = “your_client_secret”
							host = “your_host”
							access_token = "your_access_token"
							client_token = "your_client_token"
						}
					}
					rotation {
						auto_rotate = true
						rotate_keys = false
					}
			}
      ```
      {: pre}


The newly created TXT records that are in the relevant domains in Akamai are not automatically deleted. 
{: note}


### Ordering public certificates with your own DNS provider by using Terraform
{: #order-public-certificates-manual-terraform}
{: terraform}

1. Create a certificate authority (CA) configuration by following the steps that are defined in [Adding a CA configuration](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority&interface=ui).

2. Create a new public certificate by specifying `manual` as your DNS configuration.

```terraform
    resource "ibm_sm_public_certificate" "sm_public_certificate" {
        instance_id = local.instance_id
        region = local.region
        name = "test-public-certificate"
        secret_group_id = "default"
        ca = ibm_sm_public_certificate_configuration_ca_lets_encrypt.my_lets_encrypt_config.name
        dns = “manual”
        rotation {
            auto_rotate = true
            rotate_keys = false
        }
    }

```
{: codeblock}

   Example response:
   
   ```json
   {
      "alt_names": [
        "domain2",
        "domain3"
      ],
      "bundle_certs": false,
      "ca": "ca_config_name",
      "common_name": "domain1",
      "created_by": "User",
      "creation_date": "2022-09-13T06:21:33Z",
      "crn": "secret crn",
      "description": "Description for ordered certificate.",
      "downloaded": false,
      "id": "38747ae6-8c69-d745-5276-cdf3157b9021",
      "issuance_info": {
          "auto_rotated": false,
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
   ```
   {: screen}

3. Complete the challenges that are marked as `pending` before they expire by adding the TXT records that are specified in the challenge to your domain in your DNS provider account to verify your ownership of the domain.

    If you order a certificate for subdomains, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
    {: note}

4. Validate that the TXT records that you added are propagated. Depending on your DNS provider, it can take some time to complete.

5. After the records are propagated, request Let's Encrypt to validate the challenges to your domain and create a public certificate. 
You can do this by using the Terraform’s [ibm_sm_public_certificate_action_validate_manual_dns](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/sm_public_certificate_action_validate_manual_dns) resource, as shown in the following example of a configuration:

```terraform
    resource "ibm_sm_public_certificate_action_validate_manual_dns" "sm_public_certificate_action_validate_manual_dns_instance" {
        instance_id = local.instance_id
        region = local.region
        secret_id = ibm_sm_public_certificate.sm_public_certificate.secret_id
    }
```
{: codeblock}

You can use [Terraform’s `depends_on` meta-argument](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on) to insure Terraform’s configuration is being created in the correct logical order as shown in these instructions.   
{: tip}

Alternatively, you can call the {{site.data.keyword.secrets-manager_short}} [Create a secret action](/apidocs/secrets-manager/secrets-manager-v2#create-secret-action) API to request Let's Encrypt to validate the challenges to your domain and create a public certificate.

   ```sh
    curl -X POST 
    --header "Authorization: Bearer {iam_token}" 
    --header "Accept: application/json" 
    --header "Content-Type: application/json" 
    --data '{ 
        "action_type": "public_cert_action_validate_dns_challenge"
    }'\ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/actions"
   ```
   {: codeblock}
   {: curl}

6. After your certificate is issued (its state is `active`), you must run the Terraform command `terraform apply` again to update your public certificate’s Terraform resource and to use your newly issued certificate.

7. Clean up and remove the TXT records from the domains in your DNS provider account.
