---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-23"

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

## Before you begin
{: #before-manage-secret-version}

Before you get started, be sure that you have the required level of access. To view the version history of a secret, update the metadata of a secret version, and restore a secret to its previous version, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Viewing the version history of secrets
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

## Listing secret versions from the CLI
{: #versions-cli}
{: cli}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in to view the general characteristics of each rotation.

To list all of the versions that are associated with a secret, run the [**`ibmcloud secrets-manager secret-versions`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-versions-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.

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
            "payload_available": false
        },
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:28:27Z",
            "downloaded": true,
            "id": "c73845b2-4a3f-7a59-a691-1e678680a970",
            "payload_available": false
        },
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:29:02Z",
            "downloaded": true,
            "id": "78c3dba8-cb34-7868-fbf5-8c608dc10d5a",
            "payload_available": false
        {
            "created_by": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0",
            "creation_date": "2021-12-14T17:29:16Z",
            "downloaded": true,
            "id": "b46092a0-af37-eb51-0ce5-d3ddfb369b9f",
            "payload_available": false
        }
    ]
}
```
{: screen} For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager).

