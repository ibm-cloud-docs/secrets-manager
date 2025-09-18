---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-18"

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

# Accessing secrets
{: #access-secrets}

After you store secrets in your {{site.data.keyword.secrets-manager_full}} service instance, you can retrieve their values.
{: shortdesc}

## Before you begin
{: #before-access-secrets}

Before you begin, be sure that you have the required level of access. To view a list of your available secrets, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam). To retrieve the value of a secret, you need the [**SecretsReader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Retrieving a secret in the UI
{: #get-secret-value-ui}
{: ui}

You can retrieve a secret by using the {{site.data.keyword.secrets-manager_short}} UI. Follow these steps to get your secret.

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
2. To view the secret value, click **View secret**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}

After your secret has been rotated, you can view the previous secret value from the **Version history** option.
{: tip}

You can also retrieve a secret's details such as expiration date, rotation interval, or state.

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
2. To view the secret value, click **Details**.

You can further filter retrieved secrets from the filter option in the Secrets table, and select a secret group, secret type, or both.
{: note}


### Downloading certificates
{: #download-certificate-ui}
{: ui}

To download a certificate by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, open the overflow menu for the certificate that you want to download.
4. Click **Download**. The certificate file is downloaded to your local system.


## Retrieving a secret from the CLI
{: #get-secret-value-cli}
{: cli}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in.

To get the value of a secret as well as review its details such as expiration date and rotation interval or state, run the [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-command) command. 

```sh
ibmcloud secrets-manager secret --id SECRET_ID
```
{: pre}

The command outputs the value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-command).

You can also get a secret by using its Name:

```sh
ibmcloud secrets-manager secret-by-name --secret-type SECRET_TYPE --name SECRET_NAME --secret-group-name SECRET_GROUP_NAME
```
{: pre}

You can further filter retrieved secrets by using the `--secret-types` and `--match-all-labels` optional flags.
{: note}


### Downloading certificates
{: #download-certificate-cli}
{: cli}

When you're working with certificates, you might need the ability to download the payload of a certificate into a `pem` file by using the CLI. To do so, you can use the {{site.data.keyword.secrets-manager_short}} CLI plug-in and `jq`.

To store the certificate into a `pem` file, run the [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-command) command.

```sh
ibmcloud secrets-manager secret --id=SECRET_ID | jq -r '.certificate' | sed 's/\\n/\n/g' > my-cert-file.pem 
```
{: pre}

The command outputs the value of the certificate and stores it to `my-cert-file.pem`. For more information about the command options, see [**`ibmcloud secrets-manager secret`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-command).


## Retrieving a secret with the API using secret ID
{: #get-secret-value-api}
{: api}

After you store a secret in your instance, you might need to retrieve its value so that you can connect to an external app or get access to a protected service. You can retrieve the value of a secret by using the {{site.data.keyword.secrets-manager_short}} API.

The following example request retrieves a secret and its details, such as expiration date and rotation interval or state. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}

```sh
curl -X GET 
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \ 
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{secret_ID}"
```
{: codeblock}
{: curl}

A successful response returns the value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret){: external}.

You can further filter retrieved secrets by using the `?secret_types` and `?match_all_labels` optional parameters.
{: note}


## Retrieving a secret with the API using secret Name
{: #get-secret-value-api-secret-name}
{: api}

You can also retrieve the secret's value by reference its Name instead of ID:

```sh
curl -X GET 
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \ 
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secret_groups/{secret_group_name}/secret_types/{secret_type}/secrets/{secert_name}"
```
{: codeblock}
{: curl}

Note that you need to specify the secret's `name`, `secret group name` and `secret_type`.
{: note}

You can further filter retrieved secrets by using the `?secret_types` and `?match_all_labels` optional parameters.
{: note}

### Retrieving arbitrary secrets that contain binary data
{: #get-arbitrary-secret-file-api}
{: api}

If you created an arbitrary secret by using a binary file, such as an image, the service uses base64 encoding to store the data as a base64 encoded string. To access the secret in its original form, you need to complete a few additional steps to base64 decode your retrieved secret.

First, retrieve the secret by calling the {{site.data.keyword.secrets-manager_short}} API. The following example uses cURL and  `jq` to collect the `payload` value of a secret.


```bash
export ARBITRARY_SECRET=`curl -X GET  
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" 
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/arbitrary/{id}" | jq --raw-output '.payload | sub(".*,"; "")'`
```
{: pre}


If you inspect the contents of `$ARBITRARY_SECRET`, you see base64 encoded data. The following snippet shows an example output.

```bash
echo $ARBITRARY_SECRET
eUdB68klDSrzSKgWcQS5...(truncated)
```
{: pre}
{: screen}

To view the secret in its original form (binary file), you can use base64 decoding. The following example uses the `base64` macOS utility to base64 decode the `$ARBITRARY_SECRET` contents.

```bash
echo $ARBITRARY_SECRET | base64 --decode > my-secret.png
```
{: pre}

The data is converted back to a binary file that you can open from your local computer.


### Downloading the previous version of a certificate
{: #get-previous-secret}

After you rotate a certificate, you can programmatically access its previous version by using the {{site.data.keyword.secrets-manager_short}} API.

The following example request retrieves a secret and its contents. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


```bash
curl -X GET  
   --header "Authorization: Bearer {iam_token}" \
   --header "Accept: application/json" \
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/versions/previous"
```
{: codeblock}
{: curl}

### Linking directly to a secret
{: #get-secert-link}
{: ui}

You can copy a link that points directly to a secret's details panel.

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
2. To copy a link to the secret, click **Copy secret URL**.
