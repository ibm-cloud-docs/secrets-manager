---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-01"

keywords: import certificates

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


# TLS certificates
{: #certificates}




You can use {{site.data.keyword.secrets-manager_full}} to store  SSL or TLS certificates that you can use for your apps or services.
{: shortdesc}

An SSL or TLS certificate is a type of digital certificate that is used to establish communication privacy between a server and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information that is used to create trusted and secure connections between endpoints. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


| Prerequisites |
| :------------ |
| <p>Before you import a certificate, be sure that you:</p><ul><li>Create an X.509 compliant certificate with a matching private key (optional).</li><li>Convert your files into Privacy-enhanced electronic mail (PEM) format.</li><li>Keep the private key unencrypted to ensure that it can be imported into {{site.data.keyword.secrets-manager_short}}</li></ul> |
{: caption="Table 1. Prerequisites - Importing certificates" caption-side="top"}
{: #import-certificates-prereqs}
{: tab-title="Importing certificates"}
{: tab-group="cert-prereqs"}
{: class="simple-tab-table"}

| Prerequisites |
| :------------ |
| <p>Before you order a certificate, be sure that you:</p><ul><li>Prepare your instance for certificate ordering.<li>Review your certificate authority and DNS provider configurations.</li><li> |
{: caption="Table 1. Prerequisites - Ordering certificates" caption-side="top"}
{: #order-certificates-prereqs}
{: tab-title="Ordering certificates"}
{: tab-group="cert-prereqs"}
{: class="simple-tab-table"}

## Importing certificates in the UI
{: #import-certificates-ui}
{: ui}

You can use {{site.data.keyword.secrets-manager_short}} to store certificate files that are signed and issued by external certificate authorities. After you import your certificate files, you can deploy the certificate to your apps and services, download the certificate, or [rotate it manually](/docs/secrets-manager?topic=secrets-manager-rotate-secrets#manual-rotate-secret) when it's time to renew. 

To add a certificate by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **TLS Certificates** tile.
5. Add a name and description to easily identify your certificate.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a certificate file or enter its value.

    You can store unexpired X.509 certificate files that are in PEM format. If you're working with certificates that are in a different format, you can use command line utilities to convert your certificates to `.pem`. For more information, see [Why can't I import my certificate?](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem).

8. Optional: Select a private key file or enter its value.

    If you choose to store a private key, ensure that it matches to your certificate. The private key must be unencrypted before you can import it to the service.

9. Optional: Select an intermediate certificate file or enter its value.
10. Optional: Add labels to help you to search for similar secrets in your instance.
11. Click **Import**.



## Importing certificates from the CLI
{: #import-certificates-cli}
{: cli}

To import a certificate by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type imported_cert` option. For example, the following command imports a certificate along with its private key and intermediate certificate.

You can import certificate files that are in the `.pem` format. Be sure to [convert your PEM files to single-line format](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem) so that they can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} CLI.
{: note}

```sh
ibmcloud secrets-manager secret-create --secret-type imported_cert --resources '[{"name": "example-imported-certificate","description": "Extended description for my secret.","certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)","private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)","intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...(redacted)"}]'
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).



## Importing certificates with the API
{: #import-certificates-api}
{: api}


You can import certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to import an existing certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}



You can import certificate files that are in the `.pem` format. Be sure to [convert your PEM files to single-line format](/docs/secrets-manager?topic=secrets-manager-troubleshoot-pem) so that they can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} API.
{: note}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/imported_cert" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
        "collection_total": 1
        },
        "resources": [
        {
          "name": "example-certificate",
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "certificate": "-----BEGIN CERTIFICATE-----\nMIICWzCCAcQCC...(redacted)",
          "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)",
          "intermediate": "-----BEGIN CERTIFICATE-----\nMIICUzHHraOa...(redacted)",
          "labels": [
            "dev",
            "us-south"
          ]
        }
        ]
    }'
```
{: codeblock}
{: curl}



A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.




