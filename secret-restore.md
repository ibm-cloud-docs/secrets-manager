---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-05"

keywords: restore previous version, revert, roll back

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

# Restoring secrets to a previous version
{: #restore-secrets}

Accidentally replace or overwrite an existing secret? You can use {{site.data.keyword.secrets-manager_full}} to immediately roll back to the previous version.
{: shortdesc}

When you restore a secret to its previous version, a new version of the secret is created. For example, if the current version of your secret is 3, and you roll back to version 2, the data that was restored from version 2 becomes version 4.

You can restore 1 version back on [supported secret types](#before-restore-secret-supported-secret-types). For auditing purposes, the service retains the metadata of up to 50 versions for each secret, which you can review as part of a secret's [version history](/docs/secrets-manager?topic=secrets-manager-version-history).
{: note}

## Before you begin
{: #before-restore-secret}

Before you get started, be sure that you have the required level of access. To view version history and restore a secret to its previous version, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

### Supported secret types
{: #before-restore-secret-supported-secret-types}

Restoring to a previous version is supported for [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials) and [public certificates](/docs/secrets-manager?topic=secrets-manager-certificates).

## Restoring a previous version in the UI
{: #restore-secret-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to restore a secret to its previous version.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to your **Secrets** list.
4. In the row for the secret that you want to inspect, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Version history**.

    If the secret was rotated previously, the page displays information about the current and previous versions.

5. Click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Restore** next to the version of the secret that you want to restore.

   Currently, you can restore only 1 version back for IAM credentials and public certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration hasn't been reached. If you don't see an option available, restoring a version isn't supported.
   {: note}



## Restoring a previous version with the API
{: #restore-secret-api}
{: api}

You can use the {{site.data.keyword.secrets-manager_short}} API to restore a secret to its previous version.

The following example request restores the previous version of a secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. Allowable values for `{secret_type}` are: `iam_credentials`, `public_cert`

To list the versions of a secret and obtain the ID of each version, use the [List versions API](/docs/secrets-manager?topic=secrets-manager-version-history&interface=api#versions-api).
{: tip}

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" \
    -d '{
      "version_id": <version_id|previous>
    }'
```
{: codeblock}
{: curl}

Currently, you can restore only 1 version back for IAM credentials and public certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration hasn't been reached.
{: note}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager).
