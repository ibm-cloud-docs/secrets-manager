---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-10"

keywords: change log for Secrets Manager APIs, API changelog, updates to Secrets Manager APIs

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

# {{site.data.keyword.secrets-manager_short}} API change log
{: #api-change-log}

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} API](/apidocs/secrets-manager/secrets-manager-v2). The change log lists changes that have been made, ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

To learn about general updates and improvements to the {{site.data.keyword.secrets-manager_short}} service, see [Release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).

## 17 February 2025
{: #2024-02-17-api}

- A new field `managed_csr` was added to support the Imported certificated with Managed CSR feature.

## 7 October 2024
{: #2024-10-07-api}

- A new configuration `action_type`, `private_cert_configuration_action_rotate_intermediate`, is now available to enable rotation of an intermediate CA's certificate. Learn more about [rotating an intermediate CA](/docs/secrets-manager?topic=secrets-manager-rotating-ca-certificates).

## 23 September 2024
{: #2024-09-23-api}

-	The [Create Secret API](/apidocs/secrets-manager/secrets-manager-v2#create-secret) now supports creating an IAM secret for managing credentials on a different {{site.data.keyword.cloud_notm}} account by passing the account ID in the new `account_id` field.
-	A new property `disabled` has been added to the IAM credentials configuration. Use this property to disable the API key configuration when you switch to using IAM service authorization configuration.
-	The [Update configuration](/apidocs/secrets-manager/secrets-manager-v2#update-configuration) method can now be used to update either the `api_key` field or the `disabled` field.
- A new field `account_id` is returned when the Service ID being managed belongs to a different {{site.data.keyword.cloud_notm}} account.

## 9 September 2024
{: #2024-09-09-api}

- The `expiration_date` field is now returned also for secret versions.

## 6 August 2024
{: #2024-08-06-api}

- Use the `secret_types` option to list configuration for a specific engine. Supported values are: `iam_credentials`, `public_cert`, and `private_cert`. 
- New property `crypto_key` to provide your own HSM.

## 10 June 2024
{: #2024-06-10-api}

- The [Update metadata API](/apidocs/secrets-manager/secrets-manager-v2#update-secret-metadata) now supports `null` for the `expiration_date` field, making it possible to disable expiration for the `arbitrary` and `username_passowrd` secret types.

## 11 March 2024
{: #2024-03-11-api}

- The [service endpoints API](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls) now also returns the key management service selected for the service instance - provider-managed, or user-provided ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}).
- The [List secrets API](/apidocs/secrets-manager/secrets-manager-v2#list-secrets) now accepts additional parameters, `?secret_types=...` to filter by secret type, and `?match_all_labels=...` to filter by a label or a combination of labels.

## 12 February 2024
{: #2024-02-12-api}

- The User credentials secret type now supports generating a random password on secret creation if the `password` field is kept empty. In addition you can control the password's length, and whether to include numbers, symbols and upper-case letters by including the `password_generation_policy` field. To learn more, see [Storing user credentials](/docs/secrets-manager?topic=secrets-manager-user-credentials).
- For an existing DNS Provider configuration, you can switch between API key and service-to-service authorization by passing an empty string in the `apikey` field. Note: it is assumed that a service-to-service authorization to the same Cloud Internet Services instance with an identical or matching access policy was configured prior to the switch.

## 20 September 2023
{: #2023-09-20-api}

Get a secret by name instead of ID by using a new API endpoint `/api/v2/secret_groups/{secret_group_name}/secret_types/{secret_type}/secrets/{name}`. Learn more in [Accessing secrets](/docs/secrets-manager?topic=secrets-manager-access-secrets&interface=api).

## 17 April 2023
{: #2023-04-17-api}

Version 2.0.0 was released on 17 April 2023. This release includes the following updates:

* You no longer need to include `secret_type` in the API URL to identify a secret. 
* The secret group name must be unique per {{site.data.keyword.secrets-manager_short}} instance. 
* Resources updates are defined as HTTP patch operations.
* The configurations API follows the pattern of the {{site.data.keyword.secrets-manager_short}} API. `config_type` acts as the API discriminator, similarly to `secret_type`.
* Configurations are modeled as openAPI composites with metadata and data parts, similarly to the {{site.data.keyword.secrets-manager_short}} model. Mappings between IAM roles and configurations API follow the same pattern for the {{site.data.keyword.secrets-manager_short}} API. For example, an IAM viewer can list configurations to view their metadata.
* List operations return metadata only for secret, secret version, and config resources.
* The action to rotate a secret is now the create a new secret version API: `POST/v2/secrets/{id}/versions`.
* The action to restore secret version is now the create a new secret version API with the `restored_from_version` body parameter.
* The action to delete IAM credentials is now the delete a secret version data API: `DELETE /v2/secrets/{id}/versions/{version_id}/secret_data`.
* Policies API is now embedded into the metadata API in version 2.0.
* The actions to list Secrets and get secret metadata return the `versions_total` field. The version's content is not included.
* Current and previous secret versions can be referenced by using the `current` and `previous` aliases in version APIs.
* As of April 17, 2023, the {{site.data.keyword.secrets-manager_full}} API v1 has been deprecated in favor of v2. If you're still actively working with the {{site.data.keyword.secrets-manager_short}} API v1, please be sure to start your upgrade as soon as possible. On 31 October 2023, support for the {{site.data.keyword.secrets-manager_short}} API v1 will be removed.


## 12 September 2022
{: #2022-09-12-api}

This release includes the following updates:

- Added the [Update the metadata of a secret version](/apidocs/secrets-manager/secrets-manager-v2#update-secret-metadata) method that can be used to store version custom metadata that is relevant to the needs of your organization.
- Updated the [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret), Invoke an action on a secret, [Get secret metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-metadata), [Get secret version metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version-metadata), and [Update secret metadata](/apidocs/secrets-manager/secrets-manager-v2#update-secret-metadata) methods to include `custom_metadata` and `version_custom_metadata` fields.

## 10 July 2022
{: #2022-07-10-api}

This release includes the following updates: 

- Added the [Lock a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret-locks-bulk) and [Lock a secret version](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version-locks-bulk) methods that can be used to create locks on a secret in your instance. For more information, see [Locking secrets](/docs/secrets-manager?topic=secrets-manager-secret-locks).
- Added the [Unlock a secret](/apidocs/secrets-manager/secrets-manager-v2#delete-secret-locks-bulk) and [Unlock a secret version](/apidocs/secrets-manager/secrets-manager-v2#delete-secret-version-locks-bulk) methods that can be used to remove locks on a secret or specific secret version.
- Added the [List secret locks](/apidocs/secrets-manager/secrets-manager-v2#list-secret-locks), [List secret version locks](/apidocs/secrets-manager/secrets-manager-v2#list-secret-version-locks), and [List all secrets and locks](/apidocs/secrets-manager/secrets-manager-v2#list-secrets-locks).
- Updated all secrets operations to return a `locks_total` field as part of the metadata of a secret.

## 25 April 2022
{: #2022-04-25-api}

This release includes the following updates: 

- Added the `private_cert` secret type that can be used to generate TLS certificates with the service. For more information, see [Creating private certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates).
- Added the [Invoke an action on a version of a secret](/apidocs/secrets-manager/secrets-manager-v2#update-secret-metadata) method that can be used to revoke a version of a private certificate. Currently, this API supports `private_cert` secrets only.
- Updated the Invoke an action on a secret method to include `revoke` as a supported action. Currently, the `revoke` action is supported for `private_cert` secrets only.
- Updated the [Get a version of a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `private_cert` secrets in addition to `imported_cert` and `public_cert`.
- Updated the [Add a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-configuration), [List configurations](/apidocs/secrets-manager/secrets-manager-v2#list-configurations), [Update a configuration](/apidocs/secrets-manager/secrets-manager-v2#update-configuration), [Get a configuration](/apidocs/secrets-manager/secrets-manager-v2#get-configuration), and [Remove a configuration](/apidocs/secrets-manager/secrets-manager-v2#delete-configuration) methods. These APIs now support `private_cert` secrets in addition to `public_cert`.
- Added the [Invoke an action on a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-configuration-action) method that be used to run operations on specific configuration elements, for example root or intermediate certificate authorities. Currently, this API supports `private_cert` secrets only.


## 3 February 2022
{: #2022-02-03-api}

This release includes the following update: 

- Added the [Register with {{site.data.keyword.en_short}}](/apidocs/secrets-manager/secrets-manager-v2#create-notifications-registration), [Get {{site.data.keyword.en_short}} registration details](/apidocs/secrets-manager/secrets-manager-v2#get-notifications-registration), [Unregister from {{site.data.keyword.en_short}}](/apidocs/secrets-manager/secrets-manager-v2#delete-notifications-registration), and [Send test event](/apidocs/secrets-manager/secrets-manager-v2#get-notifications-registration-test) methods that can be used to manage your connection to the {{site.data.keyword.en_short}} service.

## 31 January 2022
{: #2022-01-31-api}

This release includes the following update: 

- Added `kv` as a secret type to the [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret) method. You can store and manage key-value secrets, including complex JSON documents, that are used to access protected systems that are inside or outside of {{site.data.keyword.cloud_notm}}.

## 22 November 2021
{: #2021-11-22-api}

This release includes the following updates:

- Added the `service_id` string parameter as a request body option to the [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret) method. You can use this field to create IAM credentials with an existing service ID from your account, so that only an API key is generated when the secret is read or accessed.
- Added the `api_key_id` string parameter to the response details of the [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret) and [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata) methods.
- Added the `service_id_is_static` boolean parameter to the response details of the [Create a secret](/apidocs/secrets-manager/secrets-manager-v2#create-secret) and [Get secret metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-metadata) methods. This parameter indicates whether an IAM credential secret was created by using an existing service ID.
- Added the [List versions of a secret](/apidocs/secrets-manager/secrets-manager-v2#list-secret-versions) method that can be used to obtain version history information for a secret. 
- Added `payload_available` and `downloaded` boolean parameters to the response details of the [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret), [Get secret version metadata](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version-metadata), [List versions of a secret](/apidocs/secrets-manager/secrets-manager-v2#list-secret-versions) methods. These parameters can help you to identify whether the a secret version is available to be restored, and whether it has already been previously read or accessed.
- Added the `restore` query parameter as a request option on the Invoke an action on a secret method. You can use this action to [restore the previous version](/docs/secrets-manager?topic=secrets-manager-version-history) of a secret.
- Updated the [Get a version of a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `arbitrary`, `iam_credentials`, and `username_password` secrets, in addition to `public_cert` and `imported_cert`.

## 20 September 2021
{: #2021-09-20-api}

This release includes the following updates:

- Added `public_cert` secret type that can be used to order domain-validated TLS certificates with the service. For more information, see [Ordering certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates).
- Added the [Add a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-configuration), [List configurations](/apidocs/secrets-manager/secrets-manager-v2#list-configurations), [Update a configuration](/apidocs/secrets-manager/secrets-manager-v2#update-configuration), [Get a configuration](/apidocs/secrets-manager/secrets-manager-v2#get-configuration), and [Remove a configuration](/apidocs/secrets-manager/secrets-manager-v2#delete-configuration) methods that can be used to add engine configurations to the service. Currently, these APIs support `public_cert` secrets only.
- Updated the [Get a version of a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `public_cert` secrets in addition to `imported_cert`.

## 11 July 2021
{: #2021-07-11-api}

This release includes the following updates:

- Changed the maximum length for secret names to 240 characters.
- Changed the maximum length for secret descriptions to 1024 characters.

## 20 June 2021
{: #2021-06-20-api}

This release includes the following updates:

- Added `imported_cert` secret type that can be used to store X.509 certificates in the service. For more information, see [Importing certificates](/docs/secrets-manager?topic=secrets-manager-certificates).
- Added the [Get a version of a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret-version) method that can be used to retrieve the previous version of a secret. Currently, this API supports `imported_cert` secrets only.

## 13 April 2021
{: #2021-04-13-api}

This release includes the following updates:

- Added `group={secret_group_ID}` query parameter that can be used to filter a list of secrets by secret group.

## 7 March 2021
{: #2021-03-07-api}

This release includes the following updates:

- Added the `reuse_api_key` boolean parameter for IAM credential secrets.

## 10 February 2021
{: #2021-02-10-api}

This release includes the following updates:

- Added the `search={string}` query parameter that can be used to filter a list of secrets that contain a specified string.
- Added the `sort_by={field_name}` query parameter that can be used to filter a list of secrets by a specified metadata field.

## 25 January 2021
{: #2021-01-25-api}

This release includes the following updates:

- Changed the maximum length for secret names to 128 characters.
- Changed the maximum length for secret group names to 62 characters.
