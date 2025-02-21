---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-21"

keywords: change log for Secrets Manager CLI, CLI changelog, updates to Secrets Manager CLI

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

# {{site.data.keyword.secrets-manager_short}} CLI change log
{: #cli-change-log}

In this change log, you can learn about the latest changes, improvements, and updates for the [{{site.data.keyword.secrets-manager_full}} CLI plug-in](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) (`ibmcloud secrets-manager`). The change log lists changes that have been made, ordered by the date they were released. Changes to existing CLI versions are designed to be compatible with existing client applications.
{: shortdesc}

To learn about general updates and improvements to the {{site.data.keyword.secrets-manager_short}} service, see [Release notes](/docs/secrets-manager?topic=secrets-manager-release-notes).

## Version 2.0.10
{: #2.0.10}

Version 2.0.10 was released on 18 February 2025. This release includes the following updates:
- A new flag option `--imported-cert-managed-csr` was added to support the Imported certificated with Managed CSR feature.

## Version 2.0.9
{: #2.0.9}

Version 2.0.9 was released on 8 October 2024. This release includes the following updates:
- A new configuration `action_type`, `private_cert_configuration_action_rotate_intermediate`, is now available to enable rotation of an intermediate CA's certificate. Learn more about [rotating an intermediate CA](/docs/secrets-manager?topic=secrets-manager-rotating-ca-certificates).

## Version 2.0.8
{: #2.0.8}

Version 2.0.8 was released on 23 September 2024. This release includes the following updates:
- The IAM credentials engine can now be configured with IAM service authorization instead of an IAM API key. 
- IAM credentials secrets can be created and managed also from other {{site.data.keyword.cloud_notm}} accounts.

## Version 2.0.7
{: #2.0.7}

Version 2.0.7 was released on 9 August 2024. This release includes the following updates:
- The `expiration_date` field is now included in the response when getting a secret's version using the `ibmcloud secrets-manager secret-version` command.

## Version 2.0.6
{: #2.0.6}

Version 2.0.6 was released on 5 August 2024. This release includes the following updates:
- Use the `--secret-types` flag in `ibmcloud secrets-manager configurations` to list configurations for a specific engine. Supported values are: `iam_credentials`, `public_cert`, and `private_cert`. 
- Use `--labels "[]"` to remove all labels from a secret.
- Use `--expiration-date null` to remove an expiration date from a secret.
- New flag `--private-cert-crypto-key` in `configuration-create` to provide your own HSM for private certificate.

## Version 2.0.5
{: #2.0.5}

Version 2.0.5 was released on 12 March 2024. This release includes the following updates:
- Support for new List secrets parameters, `secret_types` and `match_all_labels`.

## Version 2.0.4
{: #2.0.4}

Version 2.0.4 was released on 7 February 2024. This release includes the following updates:
- Support for random password generation in User credentials. The User credentials secret type now supports generating a random password on secret creation. In addition you can control the password's length, and whether to include numbers, symbols and upper-case letters. To learn more, see [Storing user credentials](/docs/secrets-manager?topic=secrets-manager-user-credentials).

## Version 2.0.3
{: #2.0.3}

Version 2.0.3 was released on 13 December 2023. This release includes the following updates:
- Support for Service credentials secret type. [Learn more](/docs/secrets-manager?topic=secrets-manager-service-credentials).
- Strings updates
- Command name changes:
    * `--private-cert-alt-names` changes to `--certificate-alt-names` (applicable for `public_cert` and `private_cert` secret types)
    * `--private-cert-common-name` changes to `--certificate-common-name` (applicable for `public_cert` and `private_cert` secret types)
    * `--iam-credentials-ttl` changes to `--secret-ttl` (applicable for `iam_credentials` and `service_credentials` secret types)

## Version 2.0.2
{: #2.0.2}

Version 2.0.2 was released on 20 September 2023. This release includes the following updates:
- Added a new command to get a secret value by name instead of by ID.
- Added a new way to create secrets, groups, and engine configurations by using flags as an input instead of a JSON object.

## Version 2.0.1
{: #2.0.1}

Version 2.0.1 was released on 19 April 2023. In this release, the CLI was updated to address an error that Windows users faced when they attempted to install the plug-in. 

## Version 2.0.0
{: #2.0.0}

Version 2.0.0 was released on 17 April 2023. All commands have been updated in this release. For more information, review the [CLI reference](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli).
