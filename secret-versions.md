---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-15"

keywords: secret version history, view versions, secret versions

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

# Viewing the version history of secrets
{: #version-history}

When you rotate a secret in {{site.data.keyword.secrets-manager_full}}, you create a new version of its value. You can quickly examine the version history of your secrets by using the UI or API.
{: shortdesc}

## Before you begin
{: #before-versions}

Before you get started, be sure that you have the required level of access. To view the version history of a secret, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Viewing version history in the UI
{: #versions-ui}
{: ui}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} UI to view the general characteristics of each rotation.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to your **Secrets** list.
4. In the row for the secret that you want to inspect, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Version history**.

    If the secret was rotated previously, the page displays information about the current and previous versions, for example the date that each version was created. Up to 50 versions can be listed for a secret.

    If you're inspecting the version history of a public or imported certificate, you can also [download the certificate contents](/docs/secrets-manager?topic=secrets-manager-access-secrets#download-certificate-ui).
    {: tip}



## Listing secret versions with the API
{: #versions-api}
{: api}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} API to view the general characteristics of each rotation.

When you use the API to list the secrets that are stored in your service instance, each secret contains a `versions_total` field that indicates the number of versions that are associated with it. You can use the List versions API to retrieve information about each version. 

The following example request lists metadata properties for each version. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `username_password`, `imported_cert`, `public_cert`. 
{: curl}

```sh
curl -X GET "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}/versions" \
  --H "Authorization: Bearer $IAM_TOKEN" \
  --H "Accept: application/json"
```
{: codeblock}
{: curl}

A successful response returns metadata details about each secret version. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager).