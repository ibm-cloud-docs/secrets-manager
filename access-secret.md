---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-14"

keywords: access secret, retrieve secret, read secret, get secret value, get secrets, view secrets, search secrets, read secrets, get secret value

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

Most secret types in {{site.data.keyword.secrets-manager_short}} can't be retrieved directly from the {{site.data.keyword.secrets-manager_short}} service dashboard. This security mechanism is in place by default to help to prevent inadvertent exposure of your sensitive data. Secret types that you can access in the UI include: [SSL/TLS certificates](#download-certificate-ui)

You can retrieve all secret types programmatically by using the CLI, API, or SDKs. To see the steps for accessing `arbitrary`, `iam_credentials`,  and `username_password` secrets, switch to the **CLI** or **API**  instructions.
{: tip}


### Downloading certificates in the UI
{: #download-certificate-ui}
{: ui}

To download your certificate by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, open the overflow menu for the certificate that you want to download.
4. Click **Download**. The certificate file is downloaded to your local system.

After your secret has been rotated, you can click **Download previous** to obtain the previous version of your certificate. 
{: tip}


## Retrieving a secret from the CLI
{: #get-secret-value-cli}
{: cli}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in.

To get the value of a secret, run the [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-command) command. You can specify the type of secret by using the `--secret-type SECRET-TYPE` option. For example, The options for `SECRET_TYPE` are: `arbitrary`, `iam_credentials`,  and `username_password`.

```sh
ibmcloud secrets-manager secret --secret-type SECRET_TYPE --id ID
```
{: pre}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-command).


## Retrieving a secret with the API
{: #get-secret-value-api}
{: api}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} API.


The following example request retrieves a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. The options for `{secret_type}` are: `arbitrary`, `iam_credentials`,  and `username_password`.
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


### Retrieving arbitrary secrets that contain binary data
{: #get-arbitrary-secret-file-api}
{: api}

If you created an arbitrary secret by using a binary file, such as an image, the service uses base64 encoding to store the data as a base64 encoded string. To access the secret in its original form, you need to complete a few extra steps to base64 decode your retrieved secret.

First, retrieve the secret by calling the {{site.data.keyword.secrets-manager_short}} API. The following example uses cURL and  `jq` to collect the `payload` value of a secret.

```bash
export ARBITRARY_SECRET=`curl -X GET "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/arbitrary/{id}" \
  -H "Authorization: Bearer $IAM_TOKEN" \
  -H "Accept: application/json" | jq --raw-output '.resources[].secret_data.payload | sub(".*,"; "")'`
```
{: pre}

If you inspect the contents of `$ARBITRARY_SECRET`, you see base64 encoded data. The following snippet shows an example output.

```bash
echo $ARBITRARY_SECRET
eUdB68klDSrzSKgWcQS5...(truncated)
```
{:pre}
{: screen}

To view the secret in its original form (binary file), you can use base64 decoding. The following example uses the `base64` macOS utility to base64 decode the `$ARBITRARY_SECRET` contents.

```bash
echo $ARBITRARY_SECRET | base64 --decode > my-secret.png
```
{: pre}

The data is converted back to a binary file that you can open from your local computer.

<import_cert>

### Downloading the previous version of a certificate
{: #get-previous-secret}

After you rotate a certificate, you can programmatically access its previous version by using the {{site.data.keyword.secrets-manager_short}} API.


The following example request retrieves a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
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
curl -X GET "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/import_cert/{id}/versions/previous" \
  -H "Authorization: Bearer $IAM_TOKEN" \
  -H "Accept: application/json"
```
{: codeblock}
{: curl}

```java
Not currently supported.
```
{: codeblock}
{: java}

```javascript
Not currently supported.
```
{: codeblock}
{: javascript}

```python
Not currently supported.
```
{: codeblock}
{: python}

```go
Not currently supported.
```
{: codeblock}
{: go}

</import_cert>
