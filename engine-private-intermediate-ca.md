---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-25"

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

An intermediate certificate authority (CA) is a lower-level certificate authority that can sign and issue certificates to an end-entity, such as an app or website. In {{site.data.keyword.secrets-manager_full}}, you can use an intermediate CA to create [private certificates](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates).
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
|[Internally signed](#intermediate-ca-internal-signing-ui) | You're building a chain of certificates that uses an existing parent CA, for example a root CA or another intermediate CA, as its trust anchor. The parent CA was previously created in the same {{site.data.keyword.secrets-manager_short}} instance.|
|[Externally signed](#intermediate-ca-external-signing-ui) |If you created a parent CA offline or in another {{site.data.keyword.secrets-manager_short}} service instance, you can use the external CA to sign and issue an intermediate certificate authority.
{: caption="Table 1. Options for creating an intermediate CA" caption-side="top"}

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
   2. Select **Internal signing**.
   3. Enter a name to easily identify your certificate authority.
   4. Select a maximum time-to-live (TTL) for the certificate to be generated for this CA. The TTL determines how long the CA certificate remains valid.
   5. Select the maximum number of end-entity certificates that can exist in the chain.
   6. To encode the issuing CA certificate URL into end-entity certificates, set the **Encode URL** option to **Enabled**.

6. Enter the subject name fields for your root CA certificate.
7. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
8. Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

   A CRL is a list of certificates that have been revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
   1. To build a CRL for your intermediate CA with each certificate request, set thte **CRL building** option to **Enabled**.
   2. To encode the URL of the revocation list in the intermediate CA certificate, set the **CRL distribution points** option to **Enabled**.
   3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.

9. Review your selections. To create the intermediate CA, click **Create**.

   You can now select this intermediate CA to [generate a private certificate](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates). To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.

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
   2. Select **Internal signing**.
   3. Enter a name to easily identify your certificate authority.
   4. Select a maximum time-to-live (TTL) for the certificate to be generated for this CA. The TTL determines how long the CA certificate remains valid.
   5. To encode the issuing CA certificate URL into end-entity certificates, set the **Encode URL** option to **Enabled**.

6. Enter the subject name fields for your intermediate CA certificate.
7. [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the public and private key for your CA certificate.
8. Determine whether to enable certificate revocation list (CRL) building and distribution points for your CA certificate.

   A CRL is a list of certificates that have been revoked by the issuing certificate authority before their scheduled expiration date. A certificate that is listed as part of a CRL can no longer be trusted by applications. 
    
   1. To build a CRL for your intermediate CA with each certificate request, set thte **CRL building** option to **Enabled**.
   2. To encode the URL of the revocation list in the intermediate CA certificate, set the **CRL distribution points** option to **Enabled**.
   3. Select a time-to-live (TTL) of the generated CRL. The TTL determines how long the CRL remains valid.

9. Review your selections. To create the intermediate CA, click **Create**.

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
   
   The signed intermediate CA is added to your list of configurations for your instance with an **Active** status. You can now use this intermediate CA to [create private certificates](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates) for your applications. To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the certificate authority that you want to update.

## Next steps
{: #intermediate-ca-next-steps}

- [Add a certificate template](/docs/secrets-manager?topic=secrets-manager-certificate-templates)

