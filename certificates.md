<imported-cert>---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-03"

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

# SSL/TLS certificates
{: #certificates}

You can use {{site.data.keyword.secrets-manager_full}} to import or request SSL/TLS certificates that you can use for your apps or services.
{: shortdesc}

**This content is currently being developed and reviewed.**

An SSL or TLS certificate is a type of digital certificate that is used to establish communication privacy between a server (for example, your app or service) and a client. Certificates are issued by [certificate authorities (CA)](#x2016383){: term} and contain information about XYZ. After you add a certificate to your {{site.data.keyword.secrets-manager_short}} instance, you can use it to secure network communications for your cloud or on-premises deployments. Your certificate is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-certificates}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Importing certificates
{: #import-certificates}

You can use {{site.data.keyword.secrets-manager_short}} to store certificate files that are signed and issued by external certificate authorities.

Before you import a certificate, be sure that you:

- Create an X.509 compliant certificate with a matching private key (optional).
- Convert your files into Privacy-enhanced Electronic Mail (PEM) format.
- Keep the private key unencrypted to ensure that it can be imported into {{site.data.keyword.secrets-manager_short}}.



### Importing certificates in the UI
{: #import-certificates-ui}
{: ui}

To add a certificate by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Certificates** tile.
5. Add a name and description to easily identify your certificate.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a certificate file or enter its value.
8.  Optional: Select a private key file or enter its value.
9.  Optional Select an intermediate certificate file or enter its value.
10. Optional: Add labels to help you to search for similar secrets in your instance.
11. Click **Import**.



### Importing certificates with the API
{: #import-certificates-api}
{: api}


You can import certificates programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to import an existing certificate. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `createSecret` method to import a certificate. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `createSecret(params)` method to import a certificate. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `create_secret(params)` method to import a certificate. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `CreateSecret` method to import a certificate. The following code shows an example call.
{: go}

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
          "certificate": "'"$ROOT_CERTIFICATE"'",
          "private_key": "'"$PRIVATE_KEY"'",
          "intermediate": "'"$INTERMEDIATE_CERTIFICATE"'",
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

```java
TBU
```
{: codeblock}
{: java}

```javascript
TBU
```
{: codeblock}
{: javascript}

```python
TBU
```
{: codeblock}
{: python}

```go
TBU
```
{: codeblock}
{: go}

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.


</imported-cert>