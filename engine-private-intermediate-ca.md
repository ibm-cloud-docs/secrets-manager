---

copyright:
  years: 2020, 2024
lastupdated: "2024-07-31"

keywords: intermediate certificate authority, intermediate CA, internal signing, external signing

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

# Creating intermediate certificate authorities
{: #intermediate-certificate-authorities}

An intermediate certificate authority (CA) is a lower-level certificate authority that can sign and issue certificates to an end-entity, such as an app or website. In {{site.data.keyword.secrets-manager_full}}, you can use an intermediate CA to create [private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates).
{: shortdesc}

If you already created a parent CA in your {{site.data.keyword.secrets-manager_short}} instance, for example a root CA or an intermediate CA, you can use it to sign and issue an intermediate CA. If you created the parent CA elsewhere outside of {{site.data.keyword.secrets-manager_short}}, you can also use that CA to sign an intermediate CA.

You can create up to 10 intermediate certificate authorities per instance. To view a list of the configurations that are available for your instance, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.
{: note}

## Before you begin
{: #before-intermediate-ca}

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

### Supported signing methods
{: #intermediate-ca-signing}

The service offers two options:

| Signing options | When to use |
| --- | --- |
|Internally signed | You're building a chain of certificates that uses an existing parent CA, for example a root CA or another intermediate CA, as its trust anchor. The parent CA was previously created in the same {{site.data.keyword.secrets-manager_short}} instance.|
|Externally signed |If you created a parent CA offline or in another {{site.data.keyword.secrets-manager_short}} service instance, you can use the external CA to sign and issue an intermediate certificate authority.
{: caption="Table 1. Options for creating an intermediate CA" caption-side="top"}

### Unsupported configuration actions in Terraform
{: #configured-actions}
{: terraform}

The following actions are not supported.

- Rotate CRL
- Revoke CA

## Creating an intermediate CA with internal signing in the UI
{: #intermediate-ca-internal-signing-ui}
{: ui}

An intermediate CA with internal signing uses a parent CA that was previously created in your {{site.data.keyword.secrets-manager_short}} instance as its trust anchor. You can create an intermediate CA with internal signing by using the {{site.data.keyword.secrets-manager_short}} UI. 

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your {{site.data.keyword.secrets-manager_short}} instance.
3. In the **Secrets engines** page, click the **Private certificates** tab.
4. In the Certificate authorities table, click **Create certificate authority** to start the creation wizard.
5. Specify the certificate authority type and options.
   1. Select **Intermediate certificate authority** as the authority type.
   2. Select **Internal signing**. From the list of configured CAs, choose the CA that you want to use as the issuer of the intermediate CA certificate.
   3. Enter a name to easily identify your certificate authority.
   4. Select a maximum time-to-live (TTL) for the certificate to be generated for this CA. The TTL determines how long the CA certificate remains valid.
   5. Select the maximum number of end-entity certificates that can exist in the chain.
   6. To encode the issuing CA certificate URL into end-entity certificates, set the **Encode URL** option to **Enabled**.
6. Enter the subject name fields for your root CA certificate.
7. Select the Key management service. Choose the {{site.data.keyword.secrets-manager_short}} service for creating the root certificate authority keys internally by the service, or choose {{site.data.keyword.hscrypto}} (HPCS). In case HPCS is selected perform the following tasks:
   1. Select your HPCS instance from the instances dropdown list or enter your HPCS instance CRN manually 
   2. Select the IAM Credentials secret that was created earlier for authenticating with HPCS.
  
      Once the IAM credential has been set in the CA configuration it cannot be later replaced.
      {: note}
  
   3. Select the HPCS private keystore from the keystores dropdown list, or enter the keystore ID manually.
   4. Choose to use existing keys or generate new keys. In case selecting an existing HPCS private key or entering a private key ID manually, make sure that a public key exists and it has the same ID as the private key in the private keystore.
8. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
9. Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

   A CRL is a list of certificates that are revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
   1. To build a CRL for your intermediate CA with each certificate request, set the **CRL building** option to **Enabled**.
   2. To encode the URL of the revocation list in the intermediate CA certificate, set the **CRL distribution points** option to **Enabled**.
   3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.

10. Review your selections. To create the intermediate CA, click **Create**.

   You can now select this intermediate CA to [generate a private certificate](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates). To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.


## Creating an intermediate CA with internal signing with the API
{: #intermediate-ca-internal-signing-api}
{: api}

An intermediate CA with internal signing uses a parent CA that was previously created in your {{site.data.keyword.secrets-manager_short}} instance as its trust anchor. You can create an intermediate CA with internal signing by using the {{site.data.keyword.secrets-manager_short}} API. 

### Step 1: Create an intermediate CA with internal signing
The following example shows a query that you can use to create an intermediate CA with internal signing. In the request body, set the value of the `signing_method` attribute to `internal`. Use the `issuer` attribute to specify the parent certificate authority.

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "config_type": "private_cert_configuration_intermediate_ca",
  "name": "example-intermediate-CA",
  "common_name": "example.com",
  "crl_disable": false,
  "crl_distribution_points_encoded": true,
  "crl_expiry": "72h",
  "issuer": "example-root-CA",
  "issuing_certificates_urls_encoded": true,
  "max_ttl": "26300h",
  "signing_method": "internal"
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


### Step 2: Sign the intermediate CA
The following example shows a query that you can use to sign the intermediate CA that you created in step 1.

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d -d '{
  "action_type": "private_cert_configuration_action_sign_intermediate",
  "intermediate_certificate_authority": "example-intermediate-CA"
}' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/example-root-CA/actions"
```
{: codeblock}
{: curl}


## Creating an intermediate CA with external signing in the UI
{: #intermediate-ca-external-signing-ui}
{: ui}

An intermediate CA with external signing uses a parent CA from an external PKI system, or even another {{site.data.keyword.secrets-manager_short}} instance, as its trust anchor. The parent CA can be a root CA or an intermediate CA.

### Step 1: Create an intermediate CA and signing request
{: #intermediate-ca-external-create-ui}

You can create an intermediate CA certificate that uses external signing in the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your {{site.data.keyword.secrets-manager_short}} instance.
3. In the **Secrets engines** page, click the **Private certificates** tab.
4. In the Certificate authorities table, click **Create certificate authority** to start the creation wizard.
5. Specify the certificate authority type and options.
   1. Select **Intermediate certificate authority** as the authority type.
   2. Select **External signing**.
   3. Enter a name to easily identify your certificate authority.
   4. Select a maximum time-to-live (TTL) for the certificate to be generated for this CA. The TTL determines how long the CA certificate remains valid.
   5. To encode the issuing CA certificate URL into end-entity certificates, set the **Encode URL** option to **Enabled**.
6. Enter the subject name fields for your intermediate CA certificate.
7. Select the Key management service. Choose the {{site.data.keyword.secrets-manager_short}} service for creating the root certificate authority keys internally by the service, or choose {{site.data.keyword.hscrypto}} (HPCS). In case HPCS is selected perform the following tasks:
   1. Select your HPCS instance from the instances dropdown list or enter your HPCS instance CRN manually 
   2. Select the IAM Credentials secret that was created earlier for authenticating with HPCS. 
   3. Select the HPCS private keystore from the keystores dropdown list, or enter the keystore ID manually.
   4. Choose to use existing keys or generate new keys. In case selecting an existing HPCS private key or entering a private key ID manually, make sure that a public key exists and it has the same ID as the private key in the private keystore.
8. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
9. Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

   A CRL is a list of certificates that have been revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
   1. To build a CRL for your intermediate CA with each certificate request, set the **CRL building** option to **Enabled**.
   2. To encode the URL of the revocation list in the intermediate CA certificate, set the **CRL distribution points** option to **Enabled**.
   3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.

10. Review your selections. To create the intermediate CA, click **Create**.

   The intermediate CA is added to your list of configurations for your instance with a **Signing required** status. Before you can use this intermediate CA to issue private certificates, you must sign it by using the parent CA certificate that you created in your external PKI system.

### Step 2: Sign an intermediate CA with an external CA
{: #intermediate-ca-sign-ui}

When you create an intermediate CA in {{site.data.keyword.secrets-manager_short}} that you want to sign by using an external CA, a [certificate signing request (CSR)](#x3530521){: term} is generated. You can use the CSR to sign and issue your intermediate CA certificate.

1. In the {{site.data.keyword.secrets-manager_short}} UI, go to **Secrets engines > Private certificates**.
2. In the row of the intermediate CA that you want to sign, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Sign certificate**.
3. Copy or download the CSR.
4. Use the CSR to sign your intermediate CA certificate.

   You can choose from various tools, such as `openssl`, to sign a CSR file. For example, the following `openssl` command takes a CSR file that you downloaded from {{site.data.keyword.secrets-manager_short}} and uses a PEM-encoded CA file and its associated private key to issue a signed CA certificate.

   ```sh
   openssl x509 -req -in <intermediate-ca-csr-file> -CA <external-parent-ca-file> -CAkey <external-ca-key-file> -out <signed-intermediate-ca-file>
   ```
   {: pre}

   The command outputs a signed intermediate CA certificate file that you can then import to your {{site.data.keyword.secrets-manager_short}} instance to complete the signing process.   


### Step 3: Import a signed intermediate CA to your instance
{: #intermediate-ca-set-signed-ui}

After you sign an intermediate CA certificate by using an external parent CA, you can import it to your instance by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the {{site.data.keyword.secrets-manager_short}} UI, go to **Secrets engines > Private certificates**.
2. In the row of the intermediate CA that you signed, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Sign certificate**.
3. Select or enter the signed intermediate CA certificate file that you signed in the [previous step](#intermediate-ca-sign-ui).
4. Click **Sign** to complete the external signing process.
   
   The signed intermediate CA is added to your list of configurations for your instance with an **Active** status. You can now use this intermediate CA to [create private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates) for your applications. To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.


## Creating an intermediate CA with external signing with the API
{: #intermediate-ca-external-signing-api}
{: api}

An intermediate CA with external signing uses a parent CA from an external PKI system, or even another {{site.data.keyword.secrets-manager_short}} instance, as its trust anchor. The parent CA can be a root CA or an intermediate CA. You can create an intermediate CA with external signing by using the {{site.data.keyword.secrets-manager_short}} API. 

### Step 1: Create an intermediate CA and signing request
{: #intermediate-ca-external-create-api}

The following example shows a query that you can use to create an intermediate CA with external signing. In the request body, set the value of the `signing_method` attribute to `external`. 

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "config_type": "private_cert_configuration_intermediate_ca",
  "name": "example-intermediate-CA",
  "common_name": "example.com",
  "crl_disable": false,
  "crl_distribution_points_encoded": true,
  "crl_expiry": "72h",
  "issuer": "example-root-CA",
  "issuing_certificates_urls_encoded": true,
  "max_ttl": "26300h",
  "signing_method": "external"
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

Copy the CSR from the response JSON data. The CSR value is nested within the value of the `data` attribute of the response. You can also use the [**`get configuration`**](/apidocs/secrets-manager/secrets-manager-v2#get-configuration) API to get the CSR from the data of the newly created intermediate CA.

### Step 2: Sign an intermediate CA with an external CA
{: #intermediate-ca-sign-api}

Use the CSR that you copied in step 1 to sign your intermediate CA certificate. Put the CSR in a file to be used for signing.

You can choose from various tools, such as `openssl`, to sign a CSR file. For example, the following `openssl` command takes a CSR file and uses a PEM-encoded CA file and its associated private key to issue a signed CA certificate.

```sh
openssl x509 -req -in <intermediate-ca-csr-file> -CA <external-parent-ca-file> -CAkey <external-ca-key-file> -out <signed-intermediate-ca-file>
```
{: codeblock}

The command outputs a signed certificate file that you can import into your intermediate CA configuration to complete the signing process. 

If the parent CA is a root CA or an intermediate CA from another {{site.data.keyword.secrets-manager_short}} instance, you can sign the CSR by by using the `sign-csr` action. The following example shows a query that you can use to sign the CSR.  

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "action_type": "private_cert_configuration_action_sign_csr",
  "csr": "-----BEGIN CERTIFICATE REQUEST-----\nMIICiDCCAXACAQAwGDEWMBQGA1UEAxMNct5ANo8jybxCwNjHOA==\n-----END CERTIFICATE REQUEST-----"
}' \  
  "https://{other_instance_ID}.{other_instance_region}.secrets-manager.appdomain.cloud/api/v2/configurations/example-intermediate-CA/actions"
```
{: codeblock}
{: curl}


Copy the value of the `certificate` field from the response to use it in the next step.


### Step 3: Import a signed intermediate CA into your intermediate CA configuration
{: #intermediate-ca-set-signed-api}

After you sign an intermediate CA certificate by using an external parent CA, you can import it into your intermediate CA configuration by using the `set-signed` action. The following example shows a query that you can use to import the externally-signed certificate. 

```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "action_type": "private_cert_configuration_action_set_signed",
  "certificate": "-----BEGIN CERTIFICATE-----\nMIIGRjCCBS6gAwIBAgIUSKW6zI+E9JU4bva\n-----END CERTIFICATE-----"
}' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/example-intermediate-CA/actions"
```
{: codeblock}
{: curl}

## Creating an intermediate CA with internal signing from the CLI
{: #intermediate-ca-internal-signing-cli}
{: cli}

An intermediate CA with internal signing uses a parent CA that was previously created in your {{site.data.keyword.secrets-manager_short}} instance as its trust anchor. You can create an intermediate CA with internal signing by using the {{site.data.keyword.secrets-manager_short}} CLI.

### Step 1: Create an intermediate CA with internal signing

To create an intermediate CA with internal signing, run the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command. In the configuration prototype, set the value of the `signing_method` attribute to `internal`. Use the `issuer` attribute to specify the parent certificate authority. For example, the following command creates an intermediate CA with internal signing.

```sh
ibmcloud secrets-manager configuration-create 
   --configuration-prototype='{
      "config_type": "private_cert_configuration_intermediate_ca",
      "name": "example-intermediate-CA",
      "max_ttl": "26300h",
      "signing_method": "internal",
      "issuer": "example-root-CA",
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
      "format": "pem",
      "private_key_format": "der",
      "key_type": "rsa",
      "key_bits": 4096,
      "exclude_cn_from_sans": false
   }'
```
{: pre}

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
{: pre}

### Step 2: Sign the intermediate CA
{: #intermediate-ca-external-sign-cli}

To sign the intermediate CA from the {{site.data.keyword.secrets-manager_short}} CLI, apply the `sign-intermediate` action by running the [**`ibmcloud secrets-manager configuration-action-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-action-create-command) command. Pass the name of the signing certificate authority (the issuer) with the `--name` command option. 

```sh
ibmcloud secrets-manager configuration-action-create --name example-root-CA  
   --config-action-prototype='{
      "action_type": "private_cert_configuration_action_sign_intermediate",
      "intermediate_certificate_authority": "example-intermediate-CA"
   }'
```
{:pre}

## Creating an intermediate CA with external signing from the CLI
{: #intermediate-ca-external-signing-cli}
{: cli}

An intermediate CA with external signing uses a parent CA from an external PKI system, or even another {{site.data.keyword.secrets-manager_short}} instance, as its trust anchor. The parent CA can be a root CA or an intermediate CA. You can create an intermediate CA with external signing by using the {{site.data.keyword.secrets-manager_short}} CLI.

### Step 1: Create an intermediate CA and signing request
{: #intermediate-ca-external-create-cli}

To create an intermediate CA and signing request from the {{site.data.keyword.secrets-manager_short}} CLI, run the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command. In the configuration prototype, set the value of the `signing_method` attribute to `external`. Use the `--output json` option for getting a complete response that includes the value of the `csr` attribute. For example, the following command creates an intermediate CA with external signing.

```sh
ibmcloud secrets-manager configuration-create --output json 
   --configuration-prototype='{
      "config_type": "private_cert_configuration_intermediate_ca",
      "name": "example-intermediate-CA",
      "max_ttl": "26300h",
      "signing_method": "external",
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
      "format": "pem",
      "private_key_format": "der",
      "key_type": "rsa",
      "key_bits": 4096,
      "exclude_cn_from_sans": false
   }'
```
{:pre}

Copy the CSR from the response JSON data. The CSR value is nested within the value of the `data` attribute of the response. You can also get the data of the newly created intermediate CA by running run the [**`ibmcloud secrets-manager configuration`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-command) command with the `--output json` option. For example:

```sh
ibmcloud secrets-manager configuration --name example-intermediate-CA --output json 
```
{:pre}

### Step 2: Sign an intermediate CA with an external CA
{: #intermediate-ca-sign-cli}

Use the CSR that you copied in step 1 to sign your intermediate CA certificate. Put the CSR in a file to be used for signing.

You can choose from various tools, such as `openssl`, to sign a CSR file. For example, the following `openssl` command takes a CSR file and uses a PEM-encoded CA file and its associated private key to issue a signed CA certificate.

```sh
openssl x509 -req -in <intermediate-ca-csr-file> -CA <external-parent-ca-file> -CAkey <external-ca-key-file> -out <signed-intermediate-ca-file>
```
{: pre}

The command outputs a signed certificate file that you can import into your intermediate CA configuration to complete the signing process.

If the parent CA is a root CA or an intermediate CA from another {{site.data.keyword.secrets-manager_short}} instance, you can sign the CSR by applying the `sign-csr` action. To apply the action run the [**`ibmcloud secrets-manager configuration-action-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-action-create-command) command. Copy the value of the `certificate` field from the command output to use it in the next step. For example, if the parent CA is the root CA with the name `example-root-CA` in another {{site.data.keyword.secrets-manager_short}} instance, the command would be as follows.

```sh
ibmcloud secrets-manager configuration-action-create --name example-root-CA --output json  
   --config-action-prototype='{
      "action_type": "private_cert_configuration_action_sign_csr",
      "csr": "-----BEGIN CERTIFICATE REQUEST-----\nMIICiDCCAXACAQAwGDEWMBQGA1UEAxMNct5ANo8jybxCwNjHOA==\n-----END CERTIFICATE REQUEST-----"
    }'
```
{:pre}


For the `sign-csr` action you need to target the other {{site.data.keyword.secrets-manager_short}} instance where the parent CA resides. Before running the command, export the environment variable `SECRETS_MANAGER_URL` to target the other instance.
{: note}


### Step 3: Import a signed intermediate CA into your intermediate CA configuration
{: #intermediate-ca-set-signed-cli}

After you sign an intermediate CA certificate by using an external parent CA, you can import it into your intermediate CA configuration by applying the `set-signed` action to your intermediate CA configuration. To apply the action run the [**`ibmcloud secrets-manager configuration-action-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-action-create-command) command. For example:

```sh
ibmcloud secrets-manager configuration-action-create --name example-intermediate-CA 
   --config-action-prototype='{
      "action_type": "private_cert_configuration_action_set_signed",
      "certificate": "-----BEGIN CERTIFICATE-----\nMIIGRjCCBS6gAwIBAgIUSKW6zI+E9JU4bva\n-----END CERTIFICATE-----"
    }'
```
{:pre}

## Creating an intermediate CA with internal signing with Terraform
{: #intermediate-ca-internal-signing-terraform}
{: terraform}

An intermediate CA with internal signing uses a parent CA that was previously created in your {{site.data.keyword.secrets-manager_short}} instance as its trust anchor. You can create an intermediate CA with internal signing by using Terraform for {{site.data.keyword.secrets-manager_short}}.

The following example shows a configuration that you can use to create an intermediate CA with internal signing.

```terraform
    resource "ibm_sm_private_certificate_configuration_intermediate_ca" "my_intermediate_ca" {
        instance_id = local.instance_id
        signing_method = "internal"
        name = "example-intermediate-ca"
        common_name = "example.com"
        issuer = ibm_sm_private_certificate_configuration_root_ca.my_root_ca.name
        max_ttl = "180000"
    }
```
{: codeblock}

When using internal signing, the defined issuer is automatically signing the newly created intermediate CA certificate.
{: note}


## Creating an intermediate CA with external signing with the API
{: #intermediate-ca-external-signing-terraform}
{: terraform}

An intermediate CA with external signing uses a parent CA from an external PKI system, or even another {{site.data.keyword.secrets-manager_short}} instance, as its trust anchor. The parent CA can be a root CA or an intermediate CA. You can create an intermediate CA with external signing by using Terraform for {{site.data.keyword.secrets-manager_short}}.

The following example shows a configuration that you can use to create an intermediate CA with external signing. The configuration consists of three resources. 
The `ibm_sm_private_certificate_configuration_intermediate_ca` resource creates the intermediate CA configuration and the certificate signing request.
The `ibm_sm_private_certificate_configuration_action_sign_csr` resource signs the CSR with a root CA from another {{site.data.keyword.secrets-manager_short}} instance.
The `ibm_sm_private_certificate_configuration_action_set_signed` resource imports the signed certificate into the intermediate CA resource.

```terraform
    resource "ibm_sm_private_certificate_configuration_intermediate_ca" "my_intermediate_ca" {
        instance_id = local.instance_id
        name = "example-intermediate-ca"
        signing_method = "external"
        common_name = "example.com"
        max_ttl = "180000"
    }
    
    resource "ibm_sm_private_certificate_configuration_action_sign_csr" "my_sign_action" {
        instance_id = local.another_instance_id
        name = ibm_sm_private_certificate_configuration_root_ca.my_root_ca.name
        csr = ibm_sm_private_certificate_configuration_intermediate_ca.my_intermediate_ca.data[0].csr
    }

    resource "ibm_sm_private_certificate_configuration_action_set_signed" "my_set_signed_action" {
        instance_id = local.instance_id
        name = ibm_sm_private_certificate_configuration_intermediate_ca.my_intermediate_ca.name
        certificate = ibm_sm_private_certificate_configuration_action_sign_csr.my_sign_action.data[0].certificate
    }
```
{: codeblock}

In this example, we use external signing because the root CA is in another {{site.data.keyword.secrets-manager_short}} instance. In order to use a parent CA from an external PKI system, use another method to sign the CSR, instead of the `ibm_sm_private_certificate_configuration_action_sign_csr` resource. For example, you may use the `tls_locally_signed_cert` resource from the `tls` provider.
{: note}


## Retrieving an intermediate CA in the UI
{: #get-root-cert-engine-value-ui}
{: ui}

You can retrieve the intermediate CA value by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the **Public certificates** secret engine, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your engine configuration.
2. To view the configuration value, click **View configurationt**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}


## Retrieving an intermediate CA using CLI
{: #get-root-cert-engine-value-cli}
{: cli}

You can retrieve the intermediate CA value by using the {{site.data.keyword.secrets-manager_short}} CLI. In the following example command, replace the engine configuration name with your configuration's name.

```sh
ibmcloud secrets-manager configuration --name EXAMPLE_CONFIG --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud

```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)


## Retrieving an intermediate CA using API
{: #get-root-cert-engine-value-api}
{: api}

You can retrieve the intermediate CA value by using the {{site.data.keyword.secrets-manager_short}} API. In the following example request, replace the engine configuration name with your configuration's name.

```sh
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
--header "Accept: application/json" \
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

A successful response returns the value of the engine configuration, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-configuration){: external}.


## Next steps
{: #intermediate-ca-next-steps}

- [Add a certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates).

