---

copyright:
  years: 2020
lastupdated: "2020-09-24"

keywords: access stored secrets, retrieve secrets, get secret value, get secrets, view secrets, search secrets, get secret value

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
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}

# Accessing secrets
{: #access-secrets}

After you store secrets in your {{site.data.keyword.secrets-manager_full}} service instance, you can browse for secrets and retrieve their values programmatically by using the APIs.
{: shortdesc}

## Before you begin
{: #before-access-secrets}

Before you begin, be sure that you have the required level of access. To view a list of your available secrets, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam). To retrieve the value of a secret, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Searching for secrets in your instance
{: #search-secrets-ui}

For a high-level view of your secrets, you can use the {{site.data.keyword.secrets-manager_short}} UI to view the general characteristics of your secrets and quickly audit their configuration.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, use the search bar to search for secrets by name, secret type, or label.

    You can also use the {{site.data.keyword.secrets-manager_short}} APIs to search for secrets programmatically by type. To find out more, check out the [API docs](/apidocs/secrets-manager){: external}.

## Retrieving the value of a secret
{: #view-metadata-api}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} API.

The following example request retrieves a secret and its contents.

```bash
curl -X GET "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}"
  -H "Authorization: Bearer {IAM_token}" 
  -H "Accept: application/json" 
```
{: pre}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Retrieve a secret](/apidocs/secrets-manager#get-secret){: external}.

