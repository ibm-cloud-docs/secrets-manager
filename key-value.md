---

copyright:
  years: 2023
lastupdated: "2023-03-01"

keywords: key:value, key/value, key-value, storing key:value secrets

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

# Storing key-value secrets
{: #key-value}

You can use {{site.data.keyword.secrets-manager_full}} to store and manage key-value secrets, including complex JSON documents, that are used to access protected systems that are inside or outside of IBM Cloud. {: shortdesc}

A key-value secret is a type of application secret that can be used to hold sensitive data that is structured as a JSON object. After you create the secret, you can use it to connect your application to a protected resource, such as a database or a third-party app. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle. You can store multiple versions per key and access the history and metadata of your key-value secret with {{site.data.keyword.secrets-manager_short}}.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-key-value}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Creating key-value secrets in the UI
{: #key-value-ui}
{: ui}

To add a key-value secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps. 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**. 
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}. 
3. In the **Secrets** table, click **Add**. 
4. From the list of secret types, click the **Key-value** tile. 
5. Add a name and description to easily identify your secret. 
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret. 
   
   Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Optional: Add labels to help you to search for similar secrets in your instance. 
8. Select a file or enter the secret value that you want to associate with the secret. 

   You must enter the key-value data as a JSON object. The maximum file size is 512 KB.
   {: note}

9. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format.       
10. Click **Add**

## Creating key-value secrets from the CLI
{: #key-value-cli}
{: cli}

To create a key-value secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type kv` option. For example, the following command creates a key-value secret and stores `{"key1":"value1"}` as its value.

```sh
ibmcloud secrets-manager secret-create --secret-type kv --resources '[{"name": "example-kv-secret","description": "Extended description for my secret.","payload": {"key1":"value1"}}]' --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}



The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

## Creating key-value secrets with the API
{: #key-value-api}
{: api}

You can create key-value secrets programmatically by using the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create and store a key-value secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}




```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/kv" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
        "collection_total": 1
        },
        "resources": [
        {
          "name": "example-kv-secret",
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "payload": {
                  "key1": "val1"
                },
          "labels": [
            "dev",
            "us-south"
          ],
          "expiration_date": "2030-01-01T00:00:00Z",
          "custom_metadata": {
            "collection_nickname" : "test_collection"
            "collection_special_id" : "test12345"
          },
          "version_custom_metadata": {
            "version_special_id" : "test6789"
          }
        }
        ]
    }'
```
{: codeblock}
{: curl}





A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.


