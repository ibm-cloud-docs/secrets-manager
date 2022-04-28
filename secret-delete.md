---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-28"

keywords: delete secret, remove secret, destroy secret

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

# Deleting secrets
{: #delete-secrets}

You can use {{site.data.keyword.secrets-manager_full}} to delete a secret and its contents by using the console or service APIs.
{: shortdesc}

## Before you begin
{: #before-delete-secrets}

Before you begin, be sure that you have the required level of access. To delete secrets, you need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam).

## Deleting secrets in the UI
{: #delete-secret-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually delete your secrets.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. Use the **Secrets** table to browse the secrets in your instance.
4. In the row for the secret that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.
5. Enter the name of the secret to confirm its deletion.
6. Click **Delete**.

    After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.

## Deleting secrets from the CLI
{: #delete-secret-cli}
{: cli}

To delete a secret, run the [**`ibmcloud secrets-manager secret-delete`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-delete-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.

```sh
ibmcloud secrets-manager secret --secret-type SECRET_TYPE --id ID --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.

For more information about the command options, see [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-command).

## Deleting secrets with the API
{: #delete-secret-api}
{: api}

You can delete secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request deletes a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.
{: curl}



```bash
curl -X DELETE "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}" \
    -H "Authorization: Bearer $IAM_TOKEN"
```
{: codeblock}
{: curl}



After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.
