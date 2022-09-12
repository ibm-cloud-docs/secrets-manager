---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-12"

keywords: arbitrary secrets, arbitrary text, custom secrets

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

# Storing arbitrary secrets
{: #arbitrary-secrets}

You can use {{site.data.keyword.secrets-manager_full}} to store arbitrary secrets that are used to access protected systems that are inside or outside of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

An arbitrary secret is a type of application secret that can be used to hold structured or unstructured data, such as a key, configuration file, or any other piece of sensitive information. After you create the secret, you can use it to connect your application to a protected resource, such as a third-party app or database. Your secret is stored securely in your dedicated {{site.data.keyword.secrets-manager_short}} service instance, where you can centrally manage its lifecycle.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)


## Before you begin
{: #before-arbitrary}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Creating arbitrary secrets in the UI
{: #arbitrary-ui}
{: ui}

To add an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Other secret type** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a file or enter the secret value that you want to associate with the secret.

    {{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you select a file to assign to an arbitrary secret, the service uses base64 encoding to store the data in your instance. To access this secret later, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can keep track of secrets that require base64 decoding.
    {: note}

8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
    1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
10. Click **Add**.


## Creating arbitrary secrets from the CLI
{: #arbitrary-cli}
{: cli}

To create an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type arbitrary` option. For example, the following command creates an arbitrary secret and stores `secret-data` as its value.

{{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you need to upload a binary file, you must base64 encode the data first so that you can pass it to the {{site.data.keyword.secrets-manager_short}} CLI in a single-line string. To access this secret later in its original form, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can keep track of secrets that require base64 decoding.
{: note}

```sh
ibmcloud secrets-manager secret-create --secret-type arbitrary --resources '[{"name": "example-arbitrary-secret","description": "Extended description for my secret.","payload": "secret-data"}]' --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


## Creating arbitrary secrets with the API
{: #arbitrary-api}
{: api}

You can create arbitrary secrets programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create and store an arbitrary secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

{{site.data.keyword.secrets-manager_short}} supports text-based payloads only for arbitrary secrets. If you need to upload a binary file, you must base64 encode the data first so that you can pass it to the {{site.data.keyword.secrets-manager_short}} API in a single-line string. To access this secret later in its original form, you need to base64 decode it. Consider assigning a label on your secret with encoded data, such as `encode:base64`, so that you can keep track of secrets that require base64 decoding.
{: note}


```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/arbitrary" \
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
        "collection_total": 1
        },
        "resources": [
        {
          "name": "example-arbitrary-secret",
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "payload": "secret-data",
          "expiration_date": "2030-12-31T00:00:00Z",
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

A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager#create-secret).
