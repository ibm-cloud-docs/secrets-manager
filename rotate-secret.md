---

copyright:
  years: 2021
lastupdated: "2021-01-20"

keywords: rotate secrets, manually rotate, new secret, automatically rotate, automatic rotation, set rotation policy

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

# Rotating secrets
{: #rotate-secrets}

You can rotate your secrets manually by using {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

When you rotate a secret in {{site.data.keyword.secrets-manager_short}}, you create a new version of its value. By rotating your secret at regular intervals, you limit its lifespan and protect against inadvertent exposure of your sensitive data. 



Rotation is available only for static secrets, such as user credentials and arbitrary secrets. IAM credentials are created dynamically on your behalf so that you don't have to manually rotate them.
{: note}

## Before you begin
{: #before-rotate-secrets}

Before you get started, be sure that you have the required level of access. To rotate secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Rotating your secrets manually
{: #manual-rotate-secret}

You can manually rotate user credentials and arbitrary secrets by using the console or APIs.

### Rotating secrets manually in the UI
{: #manual-rotate-secret-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate your secrets.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. Use the **Secrets** table to browse the secrets in your instance.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.

   If you initially provided a value for your secret, you can select a new file or enter a new value, depending on the type of secret that you are rotating. 

   For user credentials, you can choose to have the service generate a new password on your behalf. {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols.
   {: note}
5. Click **Rotate**.

  The previous version of your secret is now replaced by its latest value. If you need to audit your version history, you can use the {{site.data.keyword.secrets-manager_short}} API to retrieve the secret. To learn more, check out the [API docs](/apidocs/secrets-manager#get-secret){: external}. 

### Rotating secrets manually by using the API
{: #manual-rotate-secret-api}
{: api}

The following example request creates a new version of your secret.

```bash
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}?action=rotate" \
  -H "Authorization: Bearer {IAM_token}"  
  -H "Accept: application/json" 
  -H "Content-Type: application/json" 
  -d '{ 
    "payload": "new-secret-data" 
  }'
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Invoke an action on a secret](/apidocs/secrets-manager#update-secret){: external}.

## Rotating your secrets automatically
{: #auto-rotate-secret}

You can enable automatic rotation for specific secret types so that you can replace their values automatically at an interval that you specify.

After you set a rotation policy for a secret, the clock starts immediately based on the initial creation date of the secret. For example, if you set a monthly rotation policy for a secret that you created on `2020/10/01`, {{site.data.keyword.secrets-manager_short}} automatically rotates the secret on `2020/11/01`.

Currently, you can enable automatic rotation only for the user credentials (`username_password`) secret type.
{: note}

### Setting a rotation policy in the UI
{: #auto-rotate-secret-ui}
{: ui}

You can enable automatic rotation for your secret at its creation, or by editing the details of an existing secret. Choose between a 30, 60, or 90-day rotation interval.

If you need more control over the rotation frequency of a secret, you can use the {{site.data.keyword.secrets-manager_short}} API to set a custom interval by using `day` or `month` units of time. For more information, see [Set secret policies](/apidocs/secrets-manager#put-policy){: external}.
{: tip}

1. If you're [adding a secret](/docs/secrets-manager?topic=secrets-manager-store-secrets#user-credentials-ui), enable the rotation option by selecting a 30, 60, or 90-day rotation interval.
2. If you're editing an existing secret, set its rotation policy by updating its details.
   1. In the **Secrets** table, browse a list of your existing secrets.
   2. In the row for the secret that you want to edit, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Edit details**.
   3. Use the **Automatic rotation** option to add or remove a rotation policy for the secret.

### Setting a rotation policy by using the API
{: #auto-rotate-secret-api}
{: api}

The following example request sets a monthly rotation policy for a `username_password` secret type.

```bash
curl -X POST "https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/username_password/{id}/policies?policy=rotation" \
  -H "Authorization: Bearer {IAM_token}"  
  -H "Accept: application/json" 
  -H "Content-Type: application/json" 
  -d '{
  "metadata": {
    "collection_type": "application/vnd.ibm.secrets-manager.secret.policy+json",
    "collection_total": 1
  },
  "resources": [
    {
      "type": "application/vnd.ibm.secrets-manager.secret.policy+json",
      "rotation": {
        "interval": 1,
        "unit": "month"
      }
    }
  ]
}'
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Invoke an action on a secret](/apidocs/secrets-manager#update-secret){: external}.

