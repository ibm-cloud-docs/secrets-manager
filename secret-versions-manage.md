---

copyright:
  years: 2024
lastupdated: "2024-02-23"

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

# Managing secrets versions
{: #version-history}

When you rotate a secret in {{site.data.keyword.secrets-manager_full}}, you create a new version of its value. You can use {{site.data.keyword.secrets-manager_full}} to view the version history and update version metadata of secrets. If you accidentally replace or overwrite a secret, you can also restore it to a previous version.
{: shortdesc}

## Before you begin
{: #before-manage-secret-version}

Before you get started, be sure that you have the required level of access. To update the metadata of a secret version or restore a secret to its previous version, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam). To view the version history of a secret, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Viewing the version history of secrets
{: #version-history-view}

When you rotate a secret in {{site.data.keyword.secrets-manager_full}}, you create a new version of its value. You can quickly examine the version history of your secrets by using the UI or API.


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

5. Optional: Update the metadata of the specific version of the secret that you are viewing.
    1. To update the metadata of your secret version, upload a file or enter the metadata and the version metadata in JSON format. 
    2. Click **Update**.

## Listing secret versions from the CLI
{: #versions-cli}
{: cli}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in to view the general characteristics of each rotation.

To list all the versions that are associated with a secret, run the [**`ibmcloud secrets-manager secret-versions`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-versions-command) command. The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, `service_credentials`, and `username_password`.

```sh
ibmcloud secrets-manager secret-versions --secret-id SECRET-ID
```
{: pre}


The command outputs information about the current and previous versions. For example, the date that each version was created. Up to 50 versions can be listed for a secret. For more information about the command options, see [**`ibmcloud secrets-manager secret-versions`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-versions-command).

## Listing secret versions with the API
{: #versions-api}
{: api}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} API to view the general characteristics of each rotation.

The following example request lists metadata properties for each version. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, `service_credentials`, and `username_password`.
{: curl}


```sh
curl -X GET 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions"
```
{: codeblock}
{: curl}


A successful response returns metadata details about each secret version.


```json
{
  "versions": [
    {
      "created_at": "2022-06-27T11:58:15Z",
      "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
      "expiration_date": "2023-10-05T11:49:42Z",
      "id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
      "payload_available": true,
      "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
      "secret_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
      "secret_name": "example-imported-certificate",
      "secret_type": "imported_cert",
      "serial_number": "38:eb:01:a3:22:e9:de:55:24:56:9b:14:cb:e2:f3:e3:e2:fb:f5:18",
      "validity": {
        "not_after": "2023-10-05T11:49:42Z",
        "not_before": "2022-06-27T11:58:15Z"
      },
      "version_custom_metadata": {
        "custom_version_key": "custom_version_value"
      }
    }
  ],
  "total_count": 1
}
```
{: screen}


The `downloaded` property indicates whether the data for each secret version was already read or accessed. If the `payload_available` field has a value of `true`, it means that you're able to access or [restore the secret data of that version](/docs/secrets-manager?topic=secrets-manager-version-history&interface=api#restore-secret-api). For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2).

You can store metadata that is relevant to the needs of your organization with the `version_custom_metadata` request parameter. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2).

## Updating secret versions metadata in the UI
{: #versions-metadata-ui}
{: ui}

You can update the metadata of a specific version of a secret by using the {{site.data.keyword.secrets-manager_short}} UI.

To update the metadata of a secret version, complete the following steps. 
1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to your **Secrets** list.
4. In the row for the secret that you want to inspect, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Version history**.

    If the secret was rotated previously, the page displays information about the current and previous versions, for example the date that each version was created. Up to 50 versions can be listed for a secret.
    {: tip}

5. Upload a file or enter the metadata and the version metadata in JSON format. 
6. Click **Update**.


## Updating secret versions metadata from the CLI
{: #versions-metadata-cli}
{: cli}

You can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in to update the metadata of a specific version of a secret.

To update the metadata of a secret, run the [**`ibmcloud secrets-manager secret-metadata-update`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command) command. 
  
The following example shows the format of the `ibmcloud secrets-manager secret-metadata-update` command.
  
```sh
ibmcloud secrets-manager secret-version-metadata-update --secret-id SECRET-ID --id VERSION-ID --version-custom-metadata='{"anyKey": "anyValue"}'
```
{: pre}


## Updating secret versions metadata with the API
{: #versions-metadata-api}
{: api}

If you're updating the metadata of a secret version, you can use the {{site.data.keyword.secrets-manager_short}} API.

The following example request updates metadata properties for each version. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, `service_credentials`, and `username_password`.
{: curl}


```sh
curl -X PATCH  
  -H "Authorization: Bearer {iam_token}" \
  -H "Accept: application/json" \
  --H "Content-Type: application/merge-patch+json" \
  -d '{ "version_custom_metadata": { "version_special_id" : "someString" } }' \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions/{version_id}/metadata"
```
{: codeblock}
{: curl}


A successful response returns metadata details about each secret version.


```json
{
  "alias": "current",
  "created_at": "2022-06-27T11:58:15Z",
  "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
  "expiration_date": "2023-10-05T11:49:42Z",
  "id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
  "payload_available": true,
  "secret_group_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
  "secret_id": "67d025e1-0248-418f-83ba-deb0ebfb9b4a",
  "secret_name": "example-arbitrary-secret",
  "secret_type": "arbitrary",
  "version_custom_metadata": {
    "custom_version_key": "custom_version_value"
  }
}
```
{: screen}


The `downloaded` property indicates whether the data for each secret version was already read or accessed. If the `payload_available` field has a value of `true`, it means that you're able to access or [restore the secret data of that version](#restore-secret-api). 

You can store metadata that is relevant to the needs of your organization with the `version_custom_metadata` request parameter. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2)

## Restoring secrets to a previous version
{: #restore-secrets}

Accidentally replace or overwrite an existing secret? You can use {{site.data.keyword.secrets-manager_full}} to immediately roll back to the previous version.
{: shortdesc}

When you restore a secret to its previous version, a new version of the secret is created. For example, if the current version of your secret is 3, and you roll back to version 2, the data that was restored from version 2 becomes version 4.

You can restore one version back on [supported secret types](/docs/secrets-manager?topic=secrets-manager-automatic-rotation&interface=ui#before-auto-rotate-supported-secret-types). For auditing purposes, the service retains the metadata of up to 50 versions for each secret, which you can review as part of a secret's [version history](/docs/secrets-manager?topic=secrets-manager-version-history).
{: note}

### Supported secret types
{: #supported-secret-types}

Restoring to a previous version is supported for [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials).

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

   Currently, you can restore only one version back for IAM credentials secret type. A secret version can be restored only if the defined time-to-live (TTL) or lease duration was not reached. If you don't see an option available, restoring a version isn't supported.
   {: note}
   
## Restoring a previous version from the CLI
{: #restore-secret-cli}
{: cli}

You can use the {{site.data.keyword.secrets-manager_short}} CLI to restore a secret to its previous version.

The following example command restores the previous version of a secret. When you call the command, replace the SECRET_ID variable with the value that is specific to your {{site.data.keyword.secrets-manager_short}} instance.

```bash
ibmcloud sm secret-version-create --secret-d SECRET_ID --secret-version-restore-from-version "previous"
```
{:pre}

Currently, you can restore only one version back for IAM credentials and imported certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration was not reached.
{: note}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager).


## Restoring a previous version with the API
{: #restore-secret-api}
{: api}

You can use the {{site.data.keyword.secrets-manager_short}} API to restore a secret to its previous version.

The following example request restores the previous version of a secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. Allowable values for `{secret_type}` are: `iam_credentials`.

To list the versions of a secret and obtain the ID of each version, use the [List versions API](/docs/secrets-manager?topic=secrets-manager-version-history&interface=api#versions-api).
{: tip}


```bash
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
         "restore_from_version": "previous",
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


Currently, you can restore only one version back for IAM credentials and imported certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration was not reached.
{: note}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager).

