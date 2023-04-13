---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-13"

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

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} API](/apidocs/secrets-manager). The change log lists changes that have been made, ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

To learn about general updates and improvements to the {{site.data.keyword.secrets-manager_short}} service, see [Release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).





## 17 April 2023
{: #2023-04-17-api}

Version 2.0.0 was released on 17 April 2023. This release includes the following updates:

* You no longer need to include `secret_type` in the API URL to identify a secret. 
* The secret group name must be unique per {{site.data.keyword.secrets-manager_short}} instance. 
* Resources updates are defined as HTTP patch operations.
* The configurations API follows the pattern of the [sm-short]} API. `config_type` acts as the API discriminator, similarly to `secret_type`.
* Configurations are modeled as openAPI composites with metadata and data parts, similarly to the [sm-short]} model. Mappings between IAM roles and configurations API follow the same pattern for the [sm-short]} API. For example, an IAM viewer can list configurations to view their metadata.
* List operations return metadata only for secret, secret version, and config resources.
* The action to rotate a secret is now the create a new secret version API: `POST/v2/secrets/{id}/versions`.
* The action to restore secret version is now the create a new secret version API with the `restored_from_version` body parameter.
* The action to delete IAM credentials is now the delete a secret version data API: `DELETE /v2/secrets/{id}/versions/{version_id}/secret_data`.
* Policies API is now embedded into the metadata API in version 2.0.
* The actions to list Secrets and get secret metadata return the `versions_total field`. The version's content is not included.
* Current and previous secret versions can be referenced by using the `current` and `previous` aliases in version APIs.




## 12 September 2022
{: #2022-09-12-api}

This release includes the following updates:

- Added the [Update the metadata of a secret version](/apidocs/secrets-manager#update-secret-version-metadata) method that can be used to store version custom metadata that is relevant to the needs of your organization.
- Updated the [Create a secret](/apidocs/secrets-manager#create-secret), [Invoke an action on a secret](/apidocs/secrets-manager#update-secret), [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata), [Get secret version metadata](/apidocs/secrets-manager#get-secret-version-metadata), and [Update secret metadata](/apidocs/secrets-manager#update-secret-metadata) methods to include `custom_metadata` and `version_custom_metadata` fields.

## 10 July 2022
{: #2022-07-10-api}

This release includes the following updates: 

- Added the [Lock a secret](/apidocs/secrets-manager#lock-secret) and [Lock a secret version](/apidocs/secrets-manager#lock-secret-version) methods that can be used to create locks on a secret in your instance. For more information, see [Locking secrets](/docs/secrets-manager?topic=secrets-manager-secret-locks).
- Added the [Unlock a secret](/apidocs/secrets-manager#unlock-secret) and [Unlock a secret version](/apidocs/secrets-manager#unlock-secret-version) methods that can be used to remove locks on a secret or specific secret version.
- Added the [List secret locks](/apidocs/secrets-manager#get-locks), [List secret version locks](/apidocs/secrets-manager#get-secret-version-locks), and [List all secrets and locks](/apidocs/secrets-manager#list-instance-secrets-locks).
- Updated all secrets operations to return a `locks_total` field as part of the metadata of a secret.

## 25 April 2022
{: #2022-04-25-api}

This release includes the following updates: 

- Added the `private_cert` secret type that can be used to generate TLS certificates with the service. For more information, see [Creating private certificates](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates).
- Added the [Invoke an action on a version of a secret](/apidocs/secrets-manager#update-secret-version) method that can be used to revoke a version of a private certificate. Currently, this API supports `private_cert` secrets only.
- Updated the [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) method to include `revoke` as a supported action. Currently, the `revoke` action is supported for `private_cert` secrets only.
- Updated the [Get a version of a secret](/apidocs/secrets-manager#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `private_cert` secrets in addition to `imported_cert` and `public_cert`.
- Updated the [Add a configuration](/apidocs/secrets-manager#create-config-element), [List configurations](/apidocs/secrets-manager#get-config-elements), [Update a configuration](/apidocs/secrets-manager#update-config-element), [Get a configuration](/apidocs/secrets-manager#get-config-element), and [Remove a configuration](/apidocs/secrets-manager#delete-config-element) methods. These APIs now support `private_cert` secrets in addition to `public_cert`.
- Added the [Invoke an action on a configuration](/apidocs/secrets-manager#action-on-config-element) method that be used to run operations on specific configuration elements, for example root or intermediate certificate authorities. Currently, this API supports `private_cert` secrets only.


## 3 February 2022
{: #2022-02-03-api}

This release includes the following update: 

- Added the [Register with {{site.data.keyword.en_short}}](/apidocs/secrets-manager#create-notifications-registration), [Get {{site.data.keyword.en_short}} registration details](/apidocs/secrets-manager#get-notifications-registration), [Unregister from {{site.data.keyword.en_short}}](/apidocs/secrets-manager#delete-notifications-registration), and [Send test event](/apidocs/secrets-manager#send-test-notification) methods that can be used to manage your connection to the {{site.data.keyword.en_short}} service.

## 31 January 2022
{: #2022-01-31-api}

This release includes the following update: 

- Added `kv` as a secret type to the [Create a secret](/apidocs/secrets-manager#create-secret) method. You can store and manage key-value secrets, including complex JSON documents, that are used to access protected systems that are inside or outside of {{site.data.keyword.cloud_notm}}.

## 22 November 2021
{: #2021-11-22-api}

This release includes the following updates:

- Added the `service_id` string parameter as a request body option to the [Create a secret](/apidocs/secrets-manager#create-secret) method. You can use this field to create IAM credentials with an existing service ID from your account, so that only an API key is generated when the secret is read or accessed.
- Added the `api_key_id` string parameter to the response details of the [Create a secret](/apidocs/secrets-manager#create-secret) and [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata) methods.
- Added the `service_id_is_static` boolean parameter to the response details of the [Create a secret](/apidocs/secrets-manager#create-secret) and [Get secret metadata](/apidocs/secrets-manager#get-secret-metadata) methods. This parameter indicates whether an IAM credential secret was created by using an existing service ID.
- Added the [List versions of a secret](/apidocs/secrets-manager#list-secret-versions) method that can be used to obtain version history information for a secret. 
- Added `payload_available` and `downloaded` boolean parameters to the response details of the [Get a secret](/apidocs/secrets-manager#get-secret), [Get secret version metadata](/apidocs/secrets-manager#get-secret-version-metadata), [List versions of a secret](/apidocs/secrets-manager#list-secret-versions) methods. These parameters can help you to identify whether the a secret version is available to be restored, and whether it has already been previously read or accessed.
- Added the `restore` query parameter as a request option on the [Invoke an action on a secret](/apidocs/secrets-manager#update-secret) method. You can use this action to [restore the previous version](/docs/secrets-manager?topic=secrets-manager-version-history) of a secret.
- Updated the [Get a version of a secret](/apidocs/secrets-manager#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `arbitrary`, `iam_credentials`, and `username_password` secrets, in addition to `public_cert` and `imported_cert`.

## 20 September 2021
{: #2021-09-20-api}

This release includes the following updates:

- Added `public_cert` secret type that can be used to order domain-validated TLS certificates with the service. For more information, see [Ordering certificates](/docs/secrets-manager?topic=secrets-manager-certificates#order-certificates).
- Added the [Add a configuration](/apidocs/secrets-manager#create-config-element), [List configurations](/apidocs/secrets-manager#get-config-elements), [Update a configuration](/apidocs/secrets-manager#update-config-element), [Get a configuration](/apidocs/secrets-manager#get-config-element), and [Remove a configuration](/apidocs/secrets-manager#delete-config-element) methods that can be used to add engine configurations to the service. Currently, these APIs support `public_cert` secrets only.
- Updated the [Get a version of a secret](/apidocs/secrets-manager#get-secret-version) method that can be used to retrieve the previous version of a secret. This API now supports `public_cert` secrets in addition to `imported_cert`.

## 11 July 2021
{: #2021-07-11-api}

This release includes the following updates:

- Changed the maximum length for secret names to 240 characters.
- Changed the maximum length for secret descriptions to 1024 characters.

## 20 June 2021
{: #2021-06-20-api}

This release includes the following updates:

- Added `imported_cert` secret type that can be used to store X.509 certificates in the service. For more information, see [Importing certificates](/docs/secrets-manager?topic=secrets-manager-certificates#import-certificates).
- Added the [Get a version of a secret](/apidocs/secrets-manager#get-secret-version) method that can be used to retrieve the previous version of a secret. Currently, this API supports `imported_cert` secrets only.

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

