---

copyright:
  years: 2020
lastupdated: "2020-12-11"

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# Deleting secrets
{: #delete-secrets}

You can use {{site.data.keyword.secrets-manager_full}} to delete a secret and its contents by using the console or service APIs.
{: shortdesc}

## Before you begin
{: #before-delete-secrets}

Before you begin, be sure that you have the required level of access. To delete secrets, you need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam).

## Deleting secrets with the console
{: #delete-secret-ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually delete your secrets.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. Use the **Secrets** table to browse the secrets in your instance.
4. In the row for the secret that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.
5. Enter the name of the secret to confirm its deletion.
6. Click **Delete**.

    After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.

## Deleting secrets by using the API
{: #delete-secret-api}

The following example request deletes a secret and its contents.

```bash
curl -X DELETE "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}" \
  -H "Authorization: Bearer {IAM_token}" 
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Invoke an action on a secret](/apidocs/secrets-manager#update-secret){: external}.
