---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-15"

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

# Creating IAM credentials
{: #iam-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to dynamically generate IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication.
{: shortdesc}

IAM credentials are [dynamic secrets](#x9968958){: term} that you can use to access an {{site.data.keyword.cloud_notm}} resource. A set of IAM credentials consists of a service ID and an API key that is generated each time that the protected resource is read or accessed. You can define a time-to-live (TTL) or a lease duration for your IAM credential at its creation so that you shorten the amount of time that the secret exists.

To learn more about the types of secrets that you can manage in {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret)

## Before you begin
{: #before-iam-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

IAM credentials require a configuration step before you can start to create or manage them in the service. For more information, see [Configuring the IAM credentials engine](/docs/secrets-manager?topic=secrets-manager-configure-iam-engine).
{: note}

When changing an IAM credential secret's TTL, it will be applied only on the next secret version rotation.
{: note}

The IAM credential secret that is created by {{site.data.keyword.secrets-manager_short}} will also be the name of the IAM API key. For example, a secret called `my-secret-name` will have a matching API key named `Secrets-Manager-IAM-Secret-my-secret-name`. If you will later rename the secret in {{site.data.keyword.secrets-manager_short}}, this change will not reflect in IAM but functionality will not break.
{:: note}

An account administrator (or any entity with the required level of access) can externally alter IAM Credentials that are created and managed by {{site.data.keyword.secrets-manager_short}}. If such a service ID or API key is deleted outside of {{site.data.keyword.secrets-manager_short}}, the service might behave unexpectedly. For example, you might be unable to create, or rotate credentials.
{: important}

## Creating IAM credentials in the UI
{: #iam-credentials-ui}
{: ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **IAM credentials** tile.
5. Click **Next**.
6. Add a name and description to easily identify your secret.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format.  
10. Click **Next**.
11. Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.

    Minimum duration is 1 minute. Maximum is 90 days.
    {: note}

12. Optional: [Determine whether IAM credentials can be reused](/docs/secrets-manager?topic=secrets-manager-iam-credentials&interface=ui#iam-credentials-reuse-ui) for your secret.
13. Optional: Enable automatic rotation of your secret. Secrets can be automatically rotated only if the reuse IAM credentials option is selected.
14. Click **Next**.
15. [Determine the source account](/docs/secrets-manager?topic=secrets-manager-iam-credentials#iam-credentials-source-account-ui).
16. [Determine the scope of access](/docs/secrets-manager?topic=secrets-manager-iam-credentials#iam-credentials-scope-of-access-ui) to assign.
17. Click **Next**.
18. Review the details of your secret. 
19. Click **Add**.

### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-ui}
{: ui}

IAM credentials consist of a service ID and an API key. By default, the service ID and API key are single-use, ephemeral values that are generated and deleted each time that an IAM credentials secret is read or accessed. 

If you'd like to continue to use those credentials through the end of the lease of your secret, you can set **Reuse IAM credentials until lease expires** to **On**. When you enable this option, your secret retains its current service ID, and API key values and reuses them on each read while the secret remains valid. After the secret reaches the end of its lease, the credentials are revoked automatically.

If the reuse IAM credentials option is set to **Off**, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}

### Determine source account
{: #iam-credentials-source-account-ui}
{: ui}

{{site.data.keyword.secrets-manager_short}} can create and manage IAM credential secrets from either the current {{site.data.keyword.cloud_notm}} account, or from a specific {{site.data.keyword.cloud_notm}} account. When selecting to create a from a specific account, provide the account's ID.

### Determine the scope of access to assign
{: #iam-credentials-scope-of-access-ui}
{: ui}

You might already have a service ID in your account that you want to generate an API key for by selecting the service ID. Alternatively, you can generate both a service ID and an API key by assigning access to an access group.

In the **Assign access** step of the Create IAM credentials wizard, choose a scope of access for your secret.

1. To use an existing service ID, select an ID from the list. If the source account is a specific other account, provide the ID of the service ID, in the following format: `ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0`.

   Choose this option when you need {{site.data.keyword.secrets-manager_short}} to generate and manage only an API key for your IAM credentials secret, and not the service ID itself. The API key inherits the access policy of the service ID that you select from your account. Only the service IDs that you have access to are displayed. 

2. To generate both a new service ID and API key for the secret, select an access group. If the source account is a specific account, provide the IDs of the wanted access groups.

   By selecting an access group, you determine the scope of permissions that are assigned to the service ID and the API key using the access group. The service ID and API key are generated and associated with your new IAM credential. You can assign up to 10 access groups.

   Access policies should be assigned to the selected access group(s) and not directly to service IDs. Both the service ID and API key are deleted and new ones are created once the IAM credential's TTL is reached.
   {: important}

If you used an existing service ID, the API key that was generated by {{site.data.keyword.secrets-manager_short}} is automatically locked. If you selected an access group, both the new service ID and API key that {{site.data.keyword.secrets-manager_short}} creates for the secret are automatically locked. Each time you that [retrieve an IAM credentials secret](/docs/secrets-manager?topic=secrets-manager-access-secrets&interface=api#get-secret-value-api) by using the API, the API key and the service ID that {{site.data.keyword.secrets-manager_short}} generates are locked, even if you manually unlock them before retrieving the secret.
{: note}


## Creating IAM credentials from the CLI
{: #iam-credentials-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To create a service ID and API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. To create it in a specific other account add the `--iam-credentials-account-id` option.

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --secret-name "example-iam-credentials-secret" --secret-description "Description of my IAM credentials secret" --iam-credentials-access-groups ["<access_group_id>, ..."] --secret-ttl 30m --iam-credentials-reuse-apikey true
```
{: pre}

To use an existing service ID and create an API key by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. To create it in a specific other account add the `--iam-credentials-account-id` option.

```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --secret-name "example-iam-credentials-secret" --secret-description "Description of my IAM credentials secret" --iam-credentials-service-id "ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0" --secret-ttl 90d --iam-credentials-reuse-apikey true
```
{: pre}

You can find the **ID** value of a service ID in the IAM section of the console. Go to **Manage > Access (IAM) > Service IDs > _name_**. Click **Details** to view the ID.
{: note}

The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).

### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-cli}
{: cli}

If you'd like to continue to use the IAM credentials through the end of the lease of your secret, you can use the `--iam-credentials-reuse-apikey` option. If set to `true`, your secret retains its current service ID and API key values and reuses them on each read while the secret remains valid, otherwise set it to `false`. For example, the following example command creates IAM credentials that can be reused until they expire.


```sh
ibmcloud secrets-manager secret-create --secret-type iam_credentials --secret-name "example-iam-credentials-secret" --secret-description "Description of my IAM credentials secret" --iam-credentials-service-id "<iam_id_of_service_id>" --secret-ttl 30m --iam-credentials-reuse-apikey true
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. After the secret reaches the end of its lease, the credentials are revoked automatically. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).

If `--iam-credentials-reuse-apikey` is set to `false` for IAM credentials, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}


## Creating IAM credentials with the API
{: #iam-credentials-api}
{: api}

You can create IAM credentials programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a service ID and API key. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. To create it in a specific other account add the `account_id` field.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}


```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
      {
        "name": "example-iam-credentials-secret",
        "description": "Description of my IAM Credentials secret",
        "secret_type": "iam_credentials",
        "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
        "labels": [
          "dev",
          "us-south"
        ],
        "ttl": "30m",
        "access_groups": [
          "AccessGroupId-45884031-54be-4dd7-86ff-112511e92699",
          "AccessGroupId-8c0ed733-dfee-4a94-992b-e2247b86e2a2"
        ],
        "reuse_api_key": false,
        "custom_metadata": {
          "metadata_custom_key": "metadata_custom_value"
        },
        "version_custom_metadata": {
          "custom_version_key": "custom_version_value"
        }
      }' \ "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}


A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret).

### Reusing the same API key until the lease expires
{: #iam-credentials-reuse-api}
{: api}

If you'd like to use the IAM credentials through the end of the lease of your secret, you can use the `reuse_api_key` field. If set to `true`, your secret retains its current service ID and API key values and reuses them on each read while the secret remains valid. For example, the following example command creates IAM credentials that can be reused until they expire. 

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
      {
        "name": "example-iam-credentials-secret",
        "description": "Description of my IAM Credentials secret",
        "secret_type": "iam_credentials",
        "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
        "labels": [
          "dev",
          "us-south"
        ],
        "ttl": "30m",
        "access_groups": [
          "AccessGroupId-45884031-54be-4dd7-86ff-112511e92699",
          "AccessGroupId-8c0ed733-dfee-4a94-992b-e2247b86e2a2"
        ],
        "reuse_api_key": true,
        "custom_metadata": {
          "metadata_custom_key": "metadata_custom_value"
        },
        "version_custom_metadata": {
          "custom_version_key": "custom_version_value"
        }
      }' \ 
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}


A successful request returns the ID value of the secret, along with other metadata. After the secret reaches the end of its lease, the credentials are revoked automatically. For more information, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2).

If `reuse_api_key` is `false` for IAM credentials, manual rotation for the secret isn't supported. For more information, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation).
{: important}

### Using an existing service ID in your account
{: #iam-credentials-service-id-api}
{: api}

You might already have a service ID in your account that you want to use to dynamically generate an API key. In this scenario, you can choose to create an IAM credentials secret by bringing your own service ID. For example, the following command creates an IAM credential by using the `service_id` field. To create it in a specific other account add the `account_id` field.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.

You can find the **ID** value of a service ID in the IAM section of the console. Go to **Manage > Access (IAM) > Service IDs > _name_**. Click **Details** to view the ID.
{: note}


```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
          "name": "example-iam-credentials-secret",
          "description": "Description of my IAM Credentials secret",
          "secret_type": "iam_credentials",
          "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
          "labels": [
            "dev",
            "us-south"
          ],
          "ttl": "30m",
          "service_id": "ServiceId-c0c7cfa4-b24e-4917-ad74-278f2fee5ba0,
          "reuse_api_key": false,
          "custom_metadata": {
            "metadata_custom_key": "metadata_custom_value"
          },
          "version_custom_metadata": {
            "custom_version_key": "custom_version_value"
          }
        }' \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}

A successful request returns the ID value of the secret, along with other metadata. For more information, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2).

## Creating IAM credentials with Terraform
{: #iam-credentials-terraform}
{: terraform}

You can create IAM credentials programmatically by using Terraform for {{site.data.keyword.secrets-manager_short}}.

You must add a `depends_on` Terraform meta-argument and refer it to your IAM configuration resource. The `depends_on` meta-argument instructs Terraform to complete all actions on the IAM configuration before you perform actions on the IAM credentials secrets. When creating a cross-account IAM credentials secret, include the `account_id` property, pointing to the IBM Cloud account where the Service ID was created at.

The following example shows a configuration that you can use to create IAM credentials.

```terraform
    resource "ibm_sm_iam_credentials_secret" "test_iam_credentials_secret" {
        instance_id = local.instance_id
        region = local.region
        service_id = "ServiceId-f4b2deac-fbb5-4bf7-85de-88426701db97"
        ttl = "1800"
        name = "test-iam-credentials-secret"
        reuse_api_key = true
        secret_group_id = ibm_sm_secret_group.sm_secret_group_test.secret_group_id
        depends_on = [
            ibm_sm_iam_credentials_configuration.iam_credentials_configuration
        ]
    }
```
{: codeblock}


## Deleting IAM credentials
{: #iam-credentials-delete}

If you have a service ID or API key that was generated by the IAM credentials secret engine and delete your instance of {{site.data.keyword.secrets-manager_short}}, you must also delete the secret from IAM. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).
