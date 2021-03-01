---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-01"

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

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. Use the **Secrets** table to browse the secrets in your instance.
4. In the row for the secret that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.
5. Enter the name of the secret to confirm its deletion.
6. Click **Delete**.

    After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.

## Deleting secrets with the API
{: #delete-secret-api}
{: api}


You can delete secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request deletes a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, and `username_password`.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `deleteSecret` method to delete a secret. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `deleteSecret(params)` method to delete a secret. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `delete_secret(params)` method to delete a secret. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `DeleteSecret` method to delete a secret. The following code shows an example call.
{: go}

```bash
curl -X DELETE "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}" \
  -H "Authorization: Bearer $IAM_TOKEN"
```
{: codeblock}
{: curl}

```java
DeleteSecretOptions deleteSecretOptions = new DeleteSecretOptions.Builder()
  .secretType("<secret_type>")
  .id(secretIdLink)
  .build();

service.deleteSecret(deleteSecretOptions).execute();
```
{: codeblock}
{: java}

```javascript
const params = {
  secretType: '<secret_type>',
  id: secretId,
};

secretsManagerApi.deleteSecret(params)
  .then(res => {
    console.log('Secret deleted.');
    })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
response = secretsManager.delete_secret(
    secret_type='<secret_type>',
    id=secret_id_link
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
deleteSecretOptions := secretsManagerApi.NewDeleteSecretOptions(
    "<secret_type>", secretIdLink,
)

response, err := secretsManagerApi.DeleteSecret(deleteSecretOptions)
if err != nil {
    panic(err)
}
```
{: codeblock}
{: go}

After you delete a secret, the secret transitions to the _Destroyed_ state. Secrets in this state are no longer recoverable. Metadata that is associated with the secret, such as the secret's deletion date, is kept in the {{site.data.keyword.secrets-manager_short}} database.

