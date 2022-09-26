---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-26"

keywords: IAM credentials, dynamic, IAM API key, IAM secret engine, IAM secrets engine

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

# Creating IAM credentials
{: #iam-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to dynamically generate IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication.
{: shortdesc}

IAM credentials are [dynamic secrets](#x9968958){: term} that you can use to access an {{site.data.keyword.cloud_notm}} resource. A set of IAM credentials consists of a service ID and an API key that is generated each time that the protected resource is read or accessed. You can define a time-to-live (TTL) or a lease duration for your IAM credential at its creation so that you shorten the amount of time that the secret exists.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-iam-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

IAM credentials require an extra configuration step before you can start to create or manage them in the service. For more information, see [Configuring the IAM credentials engine](/docs/secrets-manager?topic=secrets-manager-configure-iam-engine).
{: note}


## Creating IAM credentials in the UI
{: #iam-credentials-ui}
{: ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **IAM credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Optional: Add labels to help you to search for similar secrets in your instance.
8. Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.

    Minimum duration is 1 minute. Maximum is 90 days.
    {: note}

9. Optional: [Determine whether IAM credentials can be reused](#iam-credentials-reuse-ui) for your secret. Then, click **Next**.
10. Optional: Add metadata to your secret or to a specific version of your secret.
    1. To include metadata with your secret, switch the metadata toggle to **Yes**.
    2. Upload a file or enter the metadata and the version metadata in JSON format. The maximum file size is 10 KB. 
11. Click **Next**.
12. [Determine the scope of access](#iam-credentials-service-id-ui) to assign for your IAM credential.
13. To confirm your selections, click **Create**.


### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-ui}
{: ui}

IAM credentials consist of a service ID and an API key. By default, the service ID and API key are single-use, ephemeral values that are generated and deleted each time that an IAM credentials secret is read or accessed. 

If you'd like to continue to use those credentials through the end of the lease of your secret, you can set **Reuse IAM credentials until lease expires** to **On**. When you enable this option, your secret retains its current service ID, and API key values and reuses them on each read while the secret remains valid. After the secret reaches the end of its lease, the credentials are revoked automatically.

If **Reuse IAM credentials until lease expires** for IAM credentials is set to **Off**, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}

### Using an existing service ID in your account
{: #iam-credentials-service-id-ui}
{: ui}

You might already have a service ID in your account that you want to use to dynamically generate an API key. In this scenario, you can choose to create an IAM credentials secret by bringing your own service ID. Or, if you prefer to generate both a service ID and an API key, you can assign access by choosing an access group.

In the **Assign access** step of the Create IAM credentials wizard, choose a scope of access for your credentials.

1. To use an existing service ID, select an ID from the list.

   Choose this option when you need {{site.data.keyword.secrets-manager_short}} to generate and manage only an API key for your IAM credentials secret, and not the service ID itself. The API key inherits the access policy of the service ID that you select from your account. Only the service IDs that you have access to are displayed. 

2. To generate both a new service ID and API key for the secret, select an access group.

   By selecting an access group from your {{site.data.keyword.cloud_notm}} account, you determine the scope of access to assign to the service ID API key. The API key is dynamically generated and associated with your new IAM credential. This step ensures that your IAM credentials are scoped with the preferred level of permissions in your {{site.data.keyword.cloud_notm}} account. You can assign up to 10 access groups.

## Creating IAM credentials from the CLI
{: #iam-credentials-cli}
{: cli}

To create a dynamic service ID and API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type iam_credentials` option. For example, the following command creates an IAM secret with a lease duration of 12 hours.

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --resources '[{"name":"example-IAM-credentials","description":"Extended description for my secret.","access_groups":["<access_group_id>"],"secret_group_id":"<secret_group_id>","ttl":"12h","labels":["dev","us-south"]}]' --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-cli}
{: cli}

IAM credentials consist of a service ID and an API key. By default, the service ID and API key are single-use, ephemeral values that are generated and deleted each time that an IAM credentials secret is read or accessed. 

If you'd like to continue to use those credentials through the end of the lease of your secret, you can use the `reuse_api_key` field. If set to `true`, your secret retains its current service ID and API key values and reuses them on each read while the secret remains valid. For example, the following example command create IAM credentials that can be reused until they expire.

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --resources '[{"name":"example-reuse-credentials","description":"Uses the same service ID API key on each read until the lease expires.","reuse_api_key": true,"secret_group_id":"<secret_group_id>","ttl":"30m","labels":["reusable"]}]' --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. After the secret reaches the end of its lease, the credentials are revoked automatically. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).

If `reuse_api_key` is `false` for IAM credentials, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}

### Using an existing service ID in your account
{: #iam-credentials-service-id-cli}
{: cli}

You might already have a service ID in your account that you want to use to dynamically generate an API key. In this scenario, you can choose to create an IAM credentials secret by bringing your own service ID. For example, the following command creates an IAM credential by using the `service_id` field.

You can find the ID value of a service ID in the IAM section of the console. Go to **Manage > Access (IAM) > Service IDs > _name_**. Click **Details** to view the ID.
{: note}

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --resources '[{"name":"example-api-key-only","description":"Generates only an API key on each read.","service_id":"<service_id>","secret_group_id":"<secret_group_id>","ttl":"12h","labels":["api-key-only"]}]'  --service-url https://<instance_id>.<region>.secrets-manager.appdomain.cloud
```
{: pre}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command).


## Creating IAM credentials with the API
{: #iam-credentials-api}
{: api}

You can create IAM credentials programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a dynamic service ID and API key. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/iam_credentials" \
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
          "name": "example-IAM-credentials",
          "description": "Extended description for my secret.",
          "access_groups": [
            "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
          ],
          "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
          "reuse_api_key": <true|false>,
          "ttl": "12h",
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

### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-api}
{: api}

IAM credentials consist of a service ID and an API key. By default, the service ID and API key are single-use, ephemeral values that are generated and deleted each time that an IAM credentials secret is read or accessed.

If you'd like to use those credentials through the end of the lease of your secret, you can use the `reuse_api_key` field. If set to `true`, your secret retains its current service ID and API key values and reuses them on each read while the secret remains valid. For example, the following example command create IAM credentials that can be reused until they expire.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/iam_credentials" \
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
          "name": "example-IAM-credentials",
          "description": "Extended description for my secret.",
          "access_groups": [
            "AccessGroupId-0529f490-129c-4877-a2a0-b57f50d3e53b"
          ],
          "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
          "reuse_api_key": true,
          "ttl": "12h",
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

A successful request returns the ID value of the secret, along with other metadata. After the secret reaches the end of its lease, the credentials are revoked automatically. For more information, check out the [API reference](/apidocs/secrets-manager).

If `reuse_api_key` is `false` for IAM credentials, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}

### Using an existing service ID in your account
{: #iam-credentials-service-id-api}
{: api}

You might already have a service ID in your account that you want to use to dynamically generate an API key. In this scenario, you can choose to create an IAM credentials secret by bringing your own service ID. For example, the following command creates an IAM credential by using the `service_id` field.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

You can find the ID value of a service ID in the IAM section of the console. Go to **Manage > Access (IAM) > Service IDs > _name_**. Click **Details** to view the ID.
{: note}

```sh
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/iam_credentials" \
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
          "name": "example-IAM-credentials",
          "description": "Extended description for my secret.",
          "service_id": "iam-ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0,
          "secret_group_id": "339c026a-ac0f-1ea1-3d43-99adf871b49a",
          "ttl": "12h",
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

A successful request returns the ID value of the secret, along with other metadata. For more information, check out the [API reference](/apidocs/secrets-manager).


## Deleting IAM credentials
{: #iam-credentials-delete}

If you have a service ID or API key that was generated by the IAM credentials secret engine and delete your instance of {{site.data.keyword.secrets-manager_short}}, you must also delete the secret from IAM. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).