---

copyright:
  years: 2020, 2024
lastupdated: "2024-08-12"

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
7. Select the Key management service. Choose the {{site.data.keyword.secrets-manager_short}} service for creating the root certificate authority keys internally by the service, or choose {{site.data.keyword.hscrypto}} (HPCS). In case HPCS is selected perform the following tasks:
   1. Select your HPCS instance from the instances dropdown list or enter your HPCS instance CRN manually 
   2. Select the IAM Credentials secret that was created earlier for authenticating with HPCS.
  
       Once the IAM credential has been set in the CA configuration it cannot be later replaced.
       {: note}
  
   3. Select the HPCS private keystore from the keystores dropdown list, or enter the keystore ID manually.
   4. Choose to use existing keys or generate new keys. In case selecting an existing HPCS private key or entering a private key ID manually, make sure that a public key exists and it has the same ID as the private key in the private keystore.

       In case you choose to generate new keys, those keys will not be deleted by {{site.data.keyword.secrets-manager_short}} in case the configuration will be deleted.
       {: note} 

8. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
9.  Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

     A CRL is a list of certificates that are revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
     1. To build a CRL for your root CA with each certificate request, set the **CRL building** option to **Enabled**.
     2. To encode the URL of the revocation list in the root CA certificate, set the **CRL distribution points** option to **Enabled**.
     3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.
10.  Review your selections. To create the root CA, click **Create**.

You can now select this root CA when you [create an intermediate CA with internal signing](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities#intermediate-ca-internal-signing-ui). To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.

## Creating a root certificate authority with the API
{: #root-ca-api}
{: api}

You can create an internally signed root certificate authority for your service instance by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a root certificate authority.
{: curl}


```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "config_type": "private_cert_configuration_root_ca",
  "name": "test-root-CA",
  "common_name": "example.com",
  "crl_disable": false,
  "crl_distribution_points_encoded": true,
  "issuing_certificates_urls_encoded": true,
  "max_ttl": "43830h"
}' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations"
```
{: codeblock}
{: curl}

If you are bringing your own HSM, include the following in the request:

```sh
"crypto_key": {
    "label": "my_key",
    "allow_generate_key": true,
    "provider": {
      "type": "hyper_protect_crypto_services",
      "instance_crn": "replace_with_hpcs_crn::",
      "pin_iam_credentials_secret_id": "replace_with_iam_credentials_secret_guid",
      "private_keystore_id": "replace_with_keystore_id"
    }
  }
```
{: codeblock}
{: curl}

A successful response adds the configuration to your service instance. 

```json
{
  "common_name": "example.com",
  "config_type": "private_cert_configuration_root_ca",
  "created_at": "2022-06-27T11:58:15Z",
  "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
  "crl_disable": false,
  "crl_distribution_points_encoded": true,
  "data": {
    "certificate": "-----BEGIN CERTIFICATE-----\nMIIGRjCCBS6gAwIBAgIUSKW6zI+E9JU4bva\n-----END CERTIFICATE-----",
    "expiration": 1825612535,
    "issuing_ca": "-----BEGIN CERTIFICATE-----\nMIIGRjCCBS6gAwIBAgIUSKW6zI+E9JU4bvad\n-----END CERTIFICATE-----",
    "serial_number": "48:a5:ba:cc:8f:84:f4:95:38:6e:f6:9d:9e:d7:8f:d8:43:d3:55:bd"
  },
  "exclude_cn_from_sans": false,
  "format": "pem",
  "issuing_certificates_urls_encoded": true,
  "max_path_length": -1,
  "max_ttl_seconds": 31536000,
  "name": "test-root-CA",
  "private_key_format": "der",
  "secret_type": "private_cert",
  "updated_at": "2022-10-05T21:33:11Z"
}
```
{: screen}

For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-configuration){: external}.


## Creating a root certificate authority from the CLI
{: #root-ca-cli}
{: cli}

You can create an internally signed root certificate authority for your service instance by using the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command.

The following example shows a command that you can use to create a root certificate authority.

```sh
ibmcloud secrets-manager configuration-create 
  --configuration-prototype='{
    "config_type": "private_cert_configuration_root_ca",
    "name": "example-root-CA", 
    "max_ttl": "43830h", 
    "crl_expiry": "72h", 
    "crl_disable": false, 
    "crl_distribution_points_encoded": true, 
    "issuing_certificates_urls_encoded": true, 
    "common_name": "example.com", 
    "alt_names": [
      "alt-name-1","alt-name-2"
      ], 
    "ip_sans": "127.0.0.1", 
    "uri_sans": "https://www.example.com/test", 
    "other_sans": ["1.2.3.5.4.3.201.10.4.3;utf8:test@example.com"], 
    "ttl": "2190h", 
    "format": "pem", 
    "private_key_format": "der", 
    "key_type": "rsa", 
    "key_bits": 4096, 
    "max_path_length": -1, 
    "exclude_cn_from_sans": false, 
    "permitted_dns_domains": ["exampleString"], 
    "ou": ["exampleString"], 
    "organization": ["exampleString"], 
    "country": ["exampleString"], 
    "locality": ["exampleString"], 
    "province": ["exampleString"], 
    "street_address": ["exampleString"], 
    "postal_code": ["exampleString"], 
    "serial_number": "d9:be:fe:35:ba:09:42:b5:35:ba:09:42:b5"
  }'
```
{: pre}

If you are bringing your own HSM, include the following in the command:

```sh
"crypto_key": {
    "label": "my_key",
    "allow_generate_key": true,
    "provider": {
      "type": "hyper_protect_crypto_services",
      "instance_crn": "replace_with_hpcs_crn::",
      "pin_iam_credentials_secret_id": "replace_with_iam_credentials_secret_guid",
      "private_keystore_id": "replace_with_keystore_id"
    }
  }
```
{: pre}


## Creating a root certificate authority with Terraform
{: #root-ca-terraform}
{: terraform}

You can create an internally signed root certificate authority for your service instance by using Terraform for {{site.data.keyword.secrets-manager_short}}.

The following example shows a configuration that you can use to create a root certificate authority.

```terraform
    resource "ibm_sm_private_certificate_configuration_root_ca" "test_root_ca" {
        instance_id = local.instance_id
        region = local.region
        name = "test-root-ca"
        common_name = "root.example.com"
        max_ttl = "3650d"
        issuing_certificates_urls_encoded = true
    }
```
{: codeblock}

If you are bringing your own HSM, include the following in the configuration:

```terraform
crypto_key {
    label = "my_key"
    allow_generate_key = true
    provider {
      type = "hyper_protect_crypto_services"
      instance_crn = "replace_with_hpcs_crn::"
      pin_iam_credentials_secret_id = "replace_with_iam_credentials_secret_guid"
      private_keystore_id = "replace_with_keystore_id"
    }
  }
  ```
{: codeblock}


## Retrieving a root certificate authority in the UI
{: #get-root-cert-engine-value-ui}
{: ui}

You can retrieve the root certificate authority value by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the **Public certificates** secret engine, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your engine configuration.
2. To view the configuration value, click **View configurationt**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}


## Retrieving a root certificate authority using CLI
{: #get-root-cert-engine-value-cli}
{: cli}

You can retrieve the root certificate authority value by using the {{site.data.keyword.secrets-manager_short}} CLI. In the following example command, replace the engine configuration name with your configuration's name.

```sh
ibmcloud secrets-manager configuration --name EXAMPLE_CONFIG --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud

```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)


## Retrieving a root certificate authority using API
{: #get-root-cert-engine-value-api}
{: api}

You can retrieve the root certificate authority value by using the {{site.data.keyword.secrets-manager_short}} API. In the following example request, replace the engine configuration name with your configuration's name.

```sh
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
--header "Accept: application/json" \
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

A successful response returns the value of the engine configuration, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-configuration){: external}.

## Next steps
{: #root-ca-next-steps}

- [Create an intermediate certificate authority](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities)

