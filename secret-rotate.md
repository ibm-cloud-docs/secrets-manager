---

copyright:
  years: 2020, 2024
lastupdated: "2024-08-12"

keywords: rotate, manually rotate, renew, reimport, reorder, manual rotation

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

# Manually rotating secrets
{: #manual-rotation}

With {{site.data.keyword.secrets-manager_full}}, you can manually create new versions of a secret.
{: shortdesc}

When you rotate a secret in your service instance, you create a new version of its value. Rotating your credentials limits how long a protected resource can be accessed by a single secret, which can protect your business against the risks that are associated with compromised credentials. Rotate your secrets regularly, for example every 30 or 60 days, so that you're always meeting best practices around secrets management.


## Before you begin
{: #before-manual-rotate}

Before you get started, be sure that you have the required level of access. To rotate secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

### Supported secret types
{: #manual-rotate-by-type}

All the secrets that you store in {{site.data.keyword.secrets-manager_short}} can be rotated and replaced on-demand. How {{site.data.keyword.secrets-manager_short}} evaluates a request to rotate a secret depends on the secret type.

| Type | Rotation description |
| --- | --- |
| [Arbitrary secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets) | Arbitrary secrets are immediately replaced with the data that you provide on rotation. |
| [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials) |IAM credentials, which consist of a service ID and API key, are immediately regenerated according to their initial configuration. If the IAM credentials secret was created by using an existing service ID in the account, only the API key is regenerated as part of a manual rotation. In contrast, if the secret was created by selecting an access group, both the service ID and API key values are regenerated when they're manually rotated.<p class="important">The **Reuse IAM credentials until lease expires** (`reuse_api_key`) option for an IAM credentials secret impacts whether it can be rotated manually. If this field is `false` or set to **Off** in the UI, manual rotation isn't supported. The API key that is dynamically generated for the secret on each read is already a single-use, ephemeral value.</p>|
| [Imported certificates](/docs/secrets-manager?topic=secrets-manager-certificates#import-certificates) | Certificates that were initially imported to a service instance are immediately replaced with the data that you reimport on rotation. |
| [Key-value secrets](/docs/secrets-manager?topic=secrets-manager-key-value) | Key-value secrets are immediately replaced with the data that you provide on rotation. |
| [Private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates) | Private certificates are immediately replaced with a certificate that is signed by its parent or issuing certificate authority.|
| [Public certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates) | Public certificates move to the **Active, Rotation pending** status to indicate that a request to rotate a certificate is being processed. {{site.data.keyword.secrets-manager_short}} sends the request to the configured certificate authority (CA), for example Let's Encrypt, to validate the ownership of your domains. If the validation completes successfully, a new certificate is issued. |
| [User credentials](/docs/secrets-manager?topic=secrets-manager-user-credentials) | Passwords that are associated with user credentials secrets are immediately replaced with the data that you provide on rotation. |
| [Service credentials](/docs/secrets-manager?topic=secrets-manager-service-credentials) | The Service credential is replaced with a new one. The previous credential remains available for the remaining time in the defined TTL. |
{: caption="Table 1. Describes how {{site.data.keyword.secrets-manager_short}} evaluates manual rotation by secret type" caption-side="top"}


Note that in the case of service credentials created for Databases, if in addition to the credential you are also altering the database permissions for the created credential, these will not be synced once the service credential was rotated. When rotating a Databases service credential, this is considered an identity rotation.
{: important}


## Creating new secret versions in the UI
{: #manual-rotate-ui}
{: ui}

You can manually rotate your secrets and certificates by using the {{site.data.keyword.secrets-manager_short}} UI.

### Rotating arbitrary secrets
{: #manual-rotate-arbitrary-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate your arbitrary secrets.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Select a new file or enter a new secret value.
6. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
7. To rotate the secret immediately, click **Rotate**.
8. Optional: Check the version history to view the latest updates.


### Creating new versions of key-value secrets
{: #manual-rotate-key-value-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate your key-value secrets.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Select a file or enter a new secret value in JSON format.
6. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
7. To rotate the secret immediately, click **Rotate**.
8. Optional: Check the version history to view the latest updates.


### Creating new versions of user credentials
{: #manual-rotate-user-credentials-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate the password values that are associated with a user credentials secret.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.

5. Determine whether to enter your own password or generate a new one.

    If you choose to generate a new password, {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols.

6. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 

7. To rotate the secret immediately, click **Rotate**.  
8. Optional: Check the version history to view the latest updates.

### Creating new versions of iam credentials
{: #manual-rotate-iam-credentials-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate the password values that are associated with an IAM credentials secret.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
6. To rotate the secret immediately, click **Rotate**.  
7. Optional: Check the version history to view the latest updates.


### Creating new versions of service credentials
{: #manual-rotate-service-credentials-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate the password values that are associated with a Service credentials secret.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
6. To rotate the secret immediately, click **Rotate**.  
7. Optional: Check the version history to view the latest updates.


### Creating new versions of imported certificates
{: #manual-rotate-imported-cert-ui}
{: ui}


When it's time to renew a certificate that was initially imported to the service, you can use the {{site.data.keyword.secrets-manager_short}} UI to manually reimport it. After a certificate is rotated, the previous version is retained in case you need it.

If the certificate that you are rotating was previously imported with an intermediate certificate and a private key, include an intermediate certificate and private key on rotation to avoid service disruptions.
{: important}

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the certificate that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Select or enter the new certificate data.

    Keep in mind that manually rotating a certificate replaces the content of the certificate with the new data that you provide only. Private keys and intermediate certificates from previous versions are not retained.

6. To rotate the certificate immediately, click **Rotate**.
7. Optional: Check the version history to view the latest updates.
8. Redeploy the latest certificate version to your TLS termination point.

    To access the current version, you can [download the certificate](/docs/secrets-manager?topic=secrets-manager-access-secrets) or retrieve it programmatically by using the [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret) API.


### Creating new versions of public certificates
{: #manual-rotate-public-cert-ui}
{: ui}

If your {{site.data.keyword.secrets-manager_short}} service instance is enabled for [public certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates), you can manually renew a certificate that was previously ordered from a third-party certificate authority.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the certificate that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
6. Click **Rotate**.

    A success message is displayed to indicate that your order is being processed. If the validation completes successfully, a new certificate is issued and the status of the certificate changes from **Active, Rotation pending** back to **Active**. If the validation doesn't complete successfully, the status of the certificate changes to **Active, Rotation failed**.

7. Optional: Check the issuance details of a certificate.

    You can check the issuance details of a public certificate by clicking the **Actions** icon ![Actions icon](../icons/actions-icon-vertical.svg) **> Details**. If there was an issue with the request, the Status field provides information about why the rotation did not complete successfully.

8. Redeploy the latest certificate version to your TLS termination point.

    To access the current version, you can [download the certificate](/docs/secrets-manager?topic=secrets-manager-access-secrets) or retrieve it programmatically by using the [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret) API.




### Creating new versions of public certificates with your own DNS provider in the UI
{: #rotate-certificates-manual-ui}
{: ui}

To rotate a public certificate that was created by using a manual DNS provider in the UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the row for the certificate that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
4. Optional: Add a name and description to easily identify your certificate.
5. Optional: Click **Update**.
6. Click **Challenges** to access the TXT record name and value that are associated with each of your domains. You need them to complete the challenges.
7. To validate the ownership of your domains, manually add the TXT records that are provided for each of your domains to your DNS provider account. You must address only the challenges that are not validated, before the expiration date. 

    If you order a certificate for subdomains, for example, `sub1.sub2.domain.com`, you need to add the TXT records to your registered domain `domain.com`.
    {: note}

8. Verify that the TXT records that you added to your domains are propagated. Depending on your DNS provider, it can take some time to complete.
9. After you confirm that the records are propagated, click **Validate** to request Let's Encrypt to validate the challenges to your domains and create a public certificate. 

   If the order fails, for example, if the TXT records were not successfully propagated, you must start a new order to proceed. 
   {: note}

10. When your certificate is issued, clean up and remove the TXT records from the domains in your DNS provider account.


### Creating new versions of private certificates
{: #manual-rotate-private-cert-ui}
{: ui}

If your {{site.data.keyword.secrets-manager_short}} service instance is enabled for [private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates), you can manually renew a certificate that was previously issued by a certificate authority that is configured for your service instance.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the certificate that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.
5. Optional: Add metadata to your secret or to a specific version of your secret. 
   1. Upload a file or enter the metadata and the version metadata in JSON format. 
6. Click **Rotate**.
7. Redeploy the latest certificate version to your TLS termination point.

    To access the current version, you can [download the certificate](/docs/secrets-manager?topic=secrets-manager-access-secrets) or retrieve it programmatically by using the [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret) API.


## Manually creating new versions of secrets from the CLI
{: #manual-rotate-cli}
{: cli}

You can manually rotate your secrets and certificates by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in.

### Rotating arbitrary secrets
{: #manual-rotate-arbitrary-cli}
{: cli}

To rotate an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command. For example, the following command rotates a secret and assigns `new-secret-data` as its new version.


```sh
ibmcloud secrets-manager secret-version-create \    
   --secret-id=SECRET_ID \    
   --secret-version-prototype='{"payload": "updated secret credentials", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'

```
{: codeblock}


The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).

### Rotating user credentials
{: #manual-rotate-user-credentials-cli}
{: cli}

To rotate a user credential secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command. For example, the following command rotates a secret and assigns `new-password` as its new version.

```sh
ibmcloud secrets-manager secret-version-create \    
   --secret-id=SECRET_ID \    
   --secret-version-prototype='{"password": "new-password", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'

```
{: codeblock}


To have the service generate and assign a random password to your credential, omit the `password` field. For example, `{}`. {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols.
{: tip}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).


### Rotating IAM credentials
{: #manual-rotate-iam-credentials-cli}
{: cli}

To rotate an IAM credential secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command. For example, the following command will create a new API key

```sh
ibmcloud secrets-manager secret-version-create \    
   --secret-id=SECRET_ID
```
{: codeblock}

Manually rotating an IAM credentials is possible only if **Reuse IAM credentials** is set to **Off**.
{: note}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).





### Rotating imported certificates
{: #manual-rotate-imported-certificates-cli}
{: cli}

To reimport a certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command. For example, the following command rotates a secret and assigns `new-secret-data` as its new version.


```sh
ibmcloud secrets-manager secret-version-create \    
   --secret-id=SECRET_ID \    
   --secret-version-prototype='{"certificate": "new-secret-data", "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'
```
{: codeblock}

Replace new lines in the certificate, intermediate, and private key data with `\n`.
{: note}


The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).


### Rotating Public certificates
{: #manual-rotate-public-certificates-cli}
{: cli}

To order a new public certificate secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command.

```sh
ibmcloud secrets-manager secret-version-create \
   --secret-id SECRET_ID \
   --public-cert-rotation '{"rotate_keys": true|false}'
```
{: codeblock}

When rotating a certificate, choose if to also rotate its private key to assigning `true` or `false` for the `rotate_keys` field.
{: note}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).


### Rotating KV secrets
{: #manual-rotate-kv-cli}
{: cli}

To rotate a KV secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command) command. For example, the following command rotates a secret and assigns `new-secret-data` as its new version.


```sh
ibmcloud secrets-manager secret-version-create \    
   --secret-id=SECRET_ID \    
   --secret-version-prototype='{"data": {"key":"value"}, "custom_metadata": {"anyKey": "anyValue"}, "version_custom_metadata": {"anyKey": "anyValue"}}'

```
{: codeblock}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-version-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-version-create-command).


## Manually rotating secrets with the API
{: #manual-rotate-api}
{: api}

You can manually rotate your secrets and certificates by using the {{site.data.keyword.secrets-manager_short}} API.

### Rotating arbitrary secrets
{: #manual-rotate-arbitrary-api}
{: api}

You can rotate arbitrary secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer {iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{ 
         "custom_metadata": { 
            "metadata_custom_key": "metadata_custom_value" 
            }, 
         "payload": "Updated arbitrary data", 
         "version_custom_metadata": { 
            "custom_version_key": "custom_version_value" 
            } 
         }' \ 
      "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"
```
{: codeblock}
{: curl}


A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).


### Rotating key-value secrets
{: #manual-rotate-key-value-api}
{: api}

You can rotate key-value secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer ${iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{ 
      "custom_metadata": { 
         "metadata_custom_key": "metadata_custom_value" 
         }, 
      "data": {
            "key1": "val2"
         },
      "version_custom_metadata": { 
         "custom_version_key": "custom_version_value" 
         }
      }' \ 
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"
```
{: codeblock}
{: curl}


A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).


### Rotating user credentials
{: #manual-rotate-user-credentials-api}
{: api}

You can rotate secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer {iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{ 
      "custom_metadata": { 
         "metadata_custom_key": "metadata_custom_value" 
         }, 
      "password": "new-password",
      "version_custom_metadata": { 
         "custom_version_key": "custom_version_value" 
         } 
      }' \ 
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"

```
{: codeblock}
{: curl}

To have the service generate and assign a random password to your credential, omit the `password` field. For example, `{}`. {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols.
{: tip}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).

### Rotating IAM credentials
{: #manual-rotate-iam-credentials-api}
{: api}

You can rotate secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer {iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{ 
      "custom_metadata": { 
         "metadata_custom_key": "metadata_custom_value" 
         }, 
      "version_custom_metadata": { 
         "custom_version_key": "custom_version_value" 
         } 
      }' \ 
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"

```
{: codeblock}
{: curl}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).


### Rotating Service credentials
{: #manual-rotate-service-credentials-api}
{: api}

You can rotate secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer {iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{ 
      "custom_metadata": { 
         "metadata_custom_key": "metadata_custom_value" 
         }, 
      "version_custom_metadata": { 
         "custom_version_key": "custom_version_value" 
         } 
      }' \ 
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"

```
{: codeblock}
{: curl}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).


### Rotating imported certificates
{: #manual-rotate-imported-cert-api}
{: api}

You can rotate secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST \
   -H "Authorization: Bearer {iam_token}" \
   -H "Accept: application/json" \
   -H "Content-Type: application/json" \
   -d '{
         "certificate": "-----BEGIN CERTIFICATE-----\nMIIE3jCCBGSgAwIBAgIUZfTbf3adn87l5J2Q2Aw+6Vk/qhowCgYIKoZIzj0EAwIwx\n-----END CERTIFICATE-----", "custom_metadata": {
            "metadata_custom_key": "metadata_custom_value"
         },
         "intermediate": "-----BEGIN CERTIFICATE-----\nMIIE3DCCBGKgAwIBAgIUKncnp6BdSUKAFGBcP4YVp/gTb7gwCgYIKoZIzj0EAwIw\n-----END CERTIFICATE-----",
         "private_key": "-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQEAqcRbzV1wp0nVrPtEpMtnWMO6Js1q3rhREZluKZfu0Q8SY4H3\n-----END RSA PRIVATE KEY-----", "version_custom_metadata": {
            "custom_version_key": "custom_version_value"
         }
      }' \ 
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"

```
{: codeblock}
{: curl}

Replace new lines in the certificate, intermediate, and private key data with `\n`.
{: note}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API docs](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version).

## Manually rotating secrets with Terraform
{: #manual-rotate-terraform}
{: terraform}

You can manually rotate your arbitrary secrets, key-value secrets, or imported certificates by using Terraform for {{site.data.keyword.secrets-manager_short}}. To manually rotate other types of secrets, you must use the UI, the API or the CLI. 

### Rotating arbitrary secrets
{: #manual-rotate-arbitrary-terraform}
{: terraform}

You can rotate arbitrary secrets by using Terraform for {{site.data.keyword.secrets-manager_short}}.

To rotate an arbitrary secret, modify the value of the `payload` attribute in your `ibm_sm_arbitrary_secret` resource configuration, and run `terraform apply` to apply the change. If you're using an input variable for the payload, modify its value in the `variables.tf` file.

You can also modify other attributes of the arbitrary secret at the same time, including metadata attributes such as `description`, `custom_metadata`, or `version_custom_metadata`.

### Rotating key-value secrets
{: #manual-rotate-key-value-terraform}
{: terraform}

To rotate a key-value secret by using Terraform for {{site.data.keyword.secrets-manager_short}}, modify the value of the `data` attribute in your `ibm_sm_kv_secret` resource configuration, and run `terraform apply` to apply the change. If you're using an input variable for the payload, modify its value in the `variables.tf` file.

You can also modify other attributes of the key-value secret at the same time, including metadata attributes such as `description`, `custom_metadata` or `version_custom_metadata`.

### Rotating imported certificates
{: #manual-rotate-imported-certificate-terraform}
{: terraform}

You can rotate imported certificates by using Terraform for {{site.data.keyword.secrets-manager_short}}.

To rotate an imported certificate, modify the values of the `certificate`, `intermediate` (optional) and `private_key` (optional) attributes in your `ibm_sm_imported_certificate` resource configuration, and run `terraform apply` to apply the change. 

You can also modify other attributes of the imported certificate at the same time, including metadata attributes such as `description`, `custom_metadata` or `version_custom_metadata`.
