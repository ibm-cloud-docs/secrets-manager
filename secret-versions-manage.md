---

copyright:
  years: 2022
lastupdated: "2022-09-12"

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

# Managing secrets versions
{: #version-history}

When you rotate a secret in {{site.data.keyword.secrets-manager_full}}, you create a new version of its value. You can use {{site.data.keyword.secrets-manager_full}} to view the version history and update version metadata of secrets. If you accidentally replace or overwrite a secret, you can also restore it to a previous version.
{: shortdesc}

## Before you begin
{: #before-manage-secret-version}

Before you get started, be sure that you have the required level of access. To view the version history of a secret, update the metadata of a secret version, and restore a secret to its previous version, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Viewing the version history of secrets
{: #version-history-view}

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

## Listing secret versions from the CLI
{: #versions-cli}
{: cli}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in to view the general characteristics of each rotation.

To list all the versions that are associated with a secret, run the [**`ibmcloud secrets-manager secret-versions`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-versions-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.

```sh
ibmcloud secrets-manager secret-versions --secret-type SECRET-TYPE --id ID --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

The command outputs information about the current and previous versions. For example, the date that each version was created. Up to 50 versions can be listed for a secret. For more information about the command options, see [**`ibmcloud secrets-manager secret-versions`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-versions-command).

## Listing secret versions with the API
{: #versions-api}
{: api}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} API to view the general characteristics of each rotation.

The following example request lists metadata properties for each version. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.
{: curl}

```sh
curl -X GET "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}/versions" \
  --H "Authorization: Bearer {IAM_token}" \
  --H "Accept: application/json"
```
{: codeblock}
{: curl}

A successful response returns metadata details about each secret version.

```json
{
    "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret.version+json",
        "collection_total": 4
    },
    "resources": [
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:27:32Z",
            "downloaded": false,
            "id": "88fe8ddd-caf9-45c7-59c4-35c9954045db",
            "payload_available": false,
            "version_custom_metadata": {
              "version_special_id" : "test6789"
            }
        },
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:28:27Z",
            "downloaded": true,
            "id": "c73845b2-4a3f-7a59-a691-1e678680a970",
            "payload_available": false,
            "version_custom_metadata": {
              "version_special_id" : "test6789"
            }
        },
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:29:02Z",
            "downloaded": true,
            "id": "78c3dba8-cb34-7868-fbf5-8c608dc10d5a",
            "payload_available": false,
            "version_custom_metadata": {
              "version_special_id" : "test6789"
            }
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:29:16Z",
            "downloaded": true,
            "id": "b46092a0-af37-eb51-0ce5-d3ddfb369b9f",
            "payload_available": false,
            "version_custom_metadata": {
              "version_special_id" : "test6789"
            }
        }
    ]
}
```
{: screen}

The `downloaded` property indicates whether the data for each secret version was already read or accessed. If the `payload_available` field has a value of `true`, it means that you're able to access or [restore the secret data of that version](#restore-secret-api). For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager).

You can store metadata that is relevant to the needs of your organization with the `version_custom_metadata` request parameter. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager).



## Updating secret versions metadata from the CLI
{: #versions-metadata-cli}
{: cli}

You can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in to update the metadata of a specific version of a secret.

To update the metadata of a secret, run the [**`ibmcloud secrets-manager secret-metadata-update`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`. You can format the `resources RESOURCES` option by including the `SecretMetadata[]` object as shown in the following example. 

```sh
[ {
  "labels" : [ "dev", "us-south" ],
  "name" : "updated-secret-name",
  "description" : "Updated description for this secret.",
  "expiration_date" : "2030-04-01T09:30:00Z"
} ]
```
The following example shows the format of the `ibmcloud secrets-manager secret-metadata-update` command.

```sh
ibmcloud secrets-manager secret-metadata-update     --secret-type=arbitrary     --id=exampleString     --resources='[{"labels": ["dev","us-south"], "name": "updated-secret-name", "description": "Updated description for this secret.", "expiration_date": "2030-04-01T09:30:00Z"}]'
```
{: pre}


## Updating secret versions metadata with the API
{: #versions-metadata-api}
{: api}

If you're updating the metadata of a secret version, you can use the {{site.data.keyword.secrets-manager_short}} API.

The following example request updates metadata properties for each version. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.
{: curl}

```sh
curl -X PUT "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{secret_id}/versions/{version_id}/metadata" \
  --H "Authorization: Bearer {IAM_token}" \
  --H "Accept: application/json"
  --d '{
            "metadata": {
            "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
            "collection_total": 1
            },
            "resources": [
            {
            "version_custom_metadata": {
                "version_special_id" : "test6789"
            }
        }
    ]
}'
```
{: codeblock}
{: curl}

A successful response returns metadata details about each secret version.

```json
{
  "metadata": {
    "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
    "collection_total": 1
  },
  "resources": [
    {
      "id": "24ec2c34-38ee-4038-9f1d-9a629423158d",
      "crn": "crn:v1:bluemix:public:secrets-manager:us-south:a/a5ebf2570dcaedf18d7ed78e216c263a:f1bc94a6-64aa-4c55-b00f-f6cd70e4b2ce:secret:24ec2c34-38ee-4038-9f1d-9a629423158d",
      "version_id": "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
      "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
      "creation_date": "2020-10-05T21:33:11Z",
      "downloaded": true,
      "payload_available": true,
      "locks_total": 1,
      "version_custom_metadata": {
        "version_special_id" : "test6789"
      } 
    }
  ]
}
```

The `downloaded` property indicates whether the data for each secret version was already read or accessed. If the `payload_available` field has a value of `true`, it means that you're able to access or [restore the secret data of that version](#restore-secret-api). 

You can store metadata that is relevant to the needs of your organization with the `version_custom_metadata` request parameter. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager)

## Restoring secrets to a previous version
{: #restore-secrets}

Accidentally replace or overwrite an existing secret? You can use {{site.data.keyword.secrets-manager_full}} to immediately roll back to the previous version.
{: shortdesc}

When you restore a secret to its previous version, a new version of the secret is created. For example, if the current version of your secret is 3, and you roll back to version 2, the data that was restored from version 2 becomes version 4.

You can restore one version back on [supported secret types](/docs/secrets-manager?topic=secrets-manager-automatic-rotation&interface=ui#before-auto-rotate-supported-secret-types). For auditing purposes, the service retains the metadata of up to 50 versions for each secret, which you can review as part of a secret's [version history](/docs/secrets-manager?topic=secrets-manager-version-history).
{: note}

### Supported secret types
{: #supported-secret-types}

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

   Currently, you can restore only one version back for IAM credentials and public certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration was not reached. If you don't see an option available, restoring a version isn't supported.
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
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -d '{
      "version_id": <version_id|previous>
    }'
```
{: codeblock}
{: curl}

Currently, you can restore only one version back for IAM credentials and public certificate secrets. A secret version can be restored only if the defined time-to-live (TTL) or lease duration was not reached.
{: note}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager).
