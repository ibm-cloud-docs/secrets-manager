---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-06"

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

# Accessing secrets
{: #access-secrets}

After you store secrets in your {{site.data.keyword.secrets-manager_full}} service instance, you can retrieve their values programmatically by using the APIs.
{: shortdesc}

## Before you begin
{: #before-access-secrets}

Before you begin, be sure that you have the required level of access. To view a list of your available secrets, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam). To retrieve the value of a secret, you need the [**SecretsReader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).



## Retrieving a secret in the UI
{: #get-secret-value-ui}
{: ui}

This action can be done only through the CLI, API, or SDKs. To see the steps, switch to the **CLI** or **API** instructions.

## Retrieving a secret from the CLI
{: #get-secret-value-cli}
{: cli}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in.

To get the value of a secret, run the [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. For example, The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`, and `username_password`.

```sh
ibmcloud secrets-manager secret --secret-type SECRET_TYPE --id ID
```
{: pre}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-command).


## Retrieving a secret with the API
{: #get-secret-value-api}
{: api}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} API.


The following example request retrieves a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`, and `username_password`.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `getSecret` method to retrieve a secret and its contents. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `getSecret(params)` method to retrieve a secret and its contents. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `get_secret(params)` method to retrieve a secret and its contents. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `GetSecret` method to retrieve a secret and its contents. The following code shows an example call.
{: go}

```bash
curl -X GET "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}" \
  -H "Authorization: Bearer $IAM_TOKEN" \
  -H "Accept: application/json"
```
{: codeblock}
{: curl}

```java
GetSecretOptions getSecretOptions = new GetSecretOptions.Builder()
  .secretType("<secret_type>")
  .id(secretIdLink)
  .build();

Response<GetSecret> response = sm.getSecret(getSecretOptions).execute();
GetSecret getSecret = response.getResult();

System.out.println(getSecret);
```
{: codeblock}
{: java}

```javascript
const params = {
  secretType: '<secret_type>',
  id: secretId,
};

secretsManagerApi.getSecret(params)
  .then(res => {
    console.log('Get secret:\n', JSON.stringify(result.resources, null, 2));
    })
  .catch(err => {
    console.warn(err)
  });
```
{: codeblock}
{: javascript}

```python
response = secretsManager.get_secret(
    secret_type='<secret_type>',
    id=secret_id_link
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
getSecretOptions := secretsManagerApi.NewGetSecretOptions(
    "<secret_type>",
    secretIdLink,
)

result, response, err := secretsManagerApi.GetSecret(getSecretOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager#get-secret){: external}.

