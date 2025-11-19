---

copyright:
  years: 2020, 2025
lastupdated: "2025-11-19"

keywords: secrets, secret types, supported secrets, static secrets, dynamic secrets,

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

# What is a secret?
{: #what-is-secret}

A secret is a piece of sensitive information. For example, an API key, password, or any type of credential that you might use to access a confidential system.
{: shortdesc}

By using secrets, you're able to authenticate to protected resources as you build your applications. For example, when you try to access an external service API, you're asked to provide a unique credential. After you supply your credential, the external service can understand who you are and whether you're authorized to interact with it.

## Working with secrets of different types
{: #secret-types}

Secrets that you create in {{site.data.keyword.secrets-manager_short}} can be static or dynamic in nature. A static secret has its expiration date and time enforced at secret creation or rotation time. In contrast, a [dynamic secret](#x9968958){: term} has its expiration date and time enforced when its secret data is read or accessed.

{{site.data.keyword.secrets-manager_short}} further classifies static and dynamic secrets by their general purpose or function. For example, each secret type is identified by programmatic name, such as `username_password`. If you're looking to manage your secret by using the {{site.data.keyword.secrets-manager_short}} API or CLI, you can use these programmatic names to run operations on secrets according to their type.

Review the following table to understand the types of static and dynamic secrets that you can create and manage with the service.

| Type | Programmatic name | Kind | Description |
| --- | --- | -- | -- |
| [Arbitrary secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets) | `arbitrary` | Static | Arbitrary pieces of sensitive data, including any type of structured or unstructured data, that you can use to access an application or resource. |
| [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials) | `iam_credentials`* | Dynamic | A dynamically generated service ID and API key that can be used to access an {{site.data.keyword.cloud_notm}} service that requires IAM authentication. |
| [Key-value secrets](/docs/secrets-manager?topic=secrets-manager-key-value) | `kv` | Static | Pieces of sensitive data that is structured in JSON format that you can use to access an application or resource. |
| [SSL/TLS certificates](/docs/secrets-manager?topic=secrets-manager-certificates) | `imported_cert`  \n `public_cert`\*  \n `private_cert`\* | Static | A type of digital certificate that can be used to establish communication privacy between a server and a client. In {{site.data.keyword.secrets-manager_short}}, you can store the following types of certificates.\n  \n - **Imported certificates**: Certificates that you import to the service. \n - **Public certificates**: Certificates that you order from a third-party certificate authority, for example Let's Encrypt.\n - **Private certificates**: Certificates that you generate by using a private certificate authority that you manage in {{site.data.keyword.secrets-manager_short}}. |
| [User credentials](/docs/secrets-manager?topic=secrets-manager-user-credentials) | `username_password` | Static | Username and password values that you can use to log in or access an application or resource. |
| [Service credentials](/docs/secrets-manager?topic=secrets-manager-service-credentials) | `service_credentials` | Static | A JSON containing service-defined sensitive data such as keys, certificates, and URLs. |
| [Custom credentials](/docs/secrets-manager?topic=secrets-manager-custom-credentials) | `custom_credentials` | Static | A JSON containing user-defined sensitive data such as keys, certificates, URLs, passwords, and any kind of arbitrary pieces of data as determined the credentials provider. |
{: caption="Secret types in {{site.data.keyword.secrets-manager_short}}" caption-side="top"}

_*Requires an [engine configuration](/docs/secrets-manager?topic=secrets-manager-secrets-engines) before secrets can be created in the service._


### Supported features by secret type
{: #compare-features-by-type}

The following table compares and contrasts some common characteristics between the secret types. 

| | Arbitrary secrets | IAM credentials | User credentials | Key-value secrets | Imported certificates | Public certificates | Private certificates | Service credentials | Custom credentials |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Automatic rotation](/docs/secrets-manager?topic=secrets-manager-automatic-rotation) | | ![Checkmark icon](../icons/checkmark-icon.svg)[^iam] | ![Checkmark icon](../icons/checkmark-icon.svg) |  | | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Manual rotation](/docs/secrets-manager?topic=secrets-manager-manual-rotation) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Expiry setting | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications)| ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |![Checkmark icon](../icons/checkmark-icon.svg) | | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Version history](/docs/secrets-manager?topic=secrets-manager-version-history) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Restore to the previous version](/docs/secrets-manager?topic=secrets-manager-version-history&interface=ui#restore-secrets) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  |  |  | |
| [SDK support](/docs/secrets-manager?topic=secrets-manager-integrate-with-apps) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [CLI plug-in support](/docs/secrets-manager?topic=secrets-manager-integrate-with-apps) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [HashiCorp Vault HTTP API compatibility](/docs/secrets-manager?topic=secrets-manager-vault-api) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Feature comparison between secret types" caption-side="top"}

[^iam]: Because IAM credentials are dynamic secrets, automatic rotation is a built-in feature. The API key that is associated with the secret is deleted automatically when the secret reaches the end of its lease. A new API key is created the next time that the secret is read.


## What's in a secret?
{: #secret-components}

Secrets that you store with the service consist of metadata attributes and a secret value. While the metadata attributes help you to identify a secret, the secret value is the data that protected services need to authenticate and authorize you or your application.

Check out the following image to see how a secret is structured.

```json
{
    "name": "my-username-password",
    "id": "cb123456-8e73-4857-594839587438",
    "description": "Description for this secret",
    "secret_type": "username_password",
    "secret_group_id": "ab654321-3958-9484-1395840384754",
    "state": 1,
    "state_description": "Active",
    "create_by": "iam-ServiceId-gj403948-3048-6059-304958674930",
    "created_at": "2023-03-08-T20:44:11Z",
    "labels": [
        "dev",
        "us-south"
    ],
    "username": "user123",
    "password": "cloudy-rainy-coffee-book"
}
```
{: screen}

1. The `name`, `id`, and `description`, and other common fields hold identifying information about a secret. These fields store the general attributes of your secret that you can use to understand its purpose and history.

2. When you retrieve the value of a secret the fields that are returned differ based on the secret type.
    
    The following truncated example shows how secret data is represented for an `arbitrary` secret.

    ```json
    {
        "name": "my-secret",
        "secret_type": "arbitrary",
        ...
        "payload": "The quick brown fox jumped over the lazy dog."
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for a `username_password` secret.

    ```json
    {
        "name": "my-secret",
        "secret_type": "username-password",
        ...
        "username": "foo",
        "password": "bar"
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for an `IAM credential` secret. 

    ```json
    {
        "name": "my-iam-credentials",
        "secret_type": "iam_credentials",
        ...
        "api_key": "RmnPBn6n1dzoo0v3kyznKEpg0WzdTpW9lW7FtKa017_u",
        "api_key_id": "ApiKey-dcd0b857-b590-4507-8c64-ae89a23e8d76",
        "service_id": "ServiceId-bb4ccc31-bd31-493a-bb58-52ec399800be",
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for a `KV` secret.

    ```json
    {
        "name": "my-kv-secret",
        "secret_type": "kv",
        ...
        "data": '{"key":"value"}'
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for `imported_cert` and `public_cert` secrets.

    ```json
    {
        "name": "my-certificate",
        "secret_type": "imported_cert/public_cert",
        ...
        "certificate":"...",
        "intermediate":"...",
        "private_key":"..."
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for a `private_cert` secret.

    ```json
    {
        "name": "my-certificate",
        "secret_type": "private_cert",
        ...
        "certificate":"...",
        "ca_chain":"...",
        "private_key":"..."
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for a `service_credentials` secret.

    ```json
    {
        "name": "my-secret",
        "secret_type": "service_credential",
        ...
        "credentials": {
            "key": "The quick brown fox jumped over the lazy dog."
            }
    }
    ```
    {: screen}

    The following truncated example shows how secret data is represented for a `custom_credentials` secret.

    ```json
    {
        "name": "my-secret",
        "secret_type": "custom_credential",
        ...
        "credentials_content": {
            "key": "The quick brown fox jumped over the lazy dog."
            }
    }
    ```
    {: screen}

## Secret states and transitions
{: #secret-states-transitions}

Secrets, in their lifetime, transition through several states that are a function of how long the secrets are in existence and whether its associated resources can be accessed.

{{site.data.keyword.secrets-manager_short}} provides a graphical user interface and a REST API for tracking secrets as they move through several states in their lifecycle. The following diagram shows how a secret passes through states between its generation and its destruction.

| State | Description |
| --- | --- |
| Pre-activation |  Secrets are initially created in the _Pre-activation_ state. Secrets in this state are displayed with a **Pre-activation** status in the UI to indicate that they aren't ready for use yet. |
| Active | After a secret is ready for use, it moves to the **Active** state.  Secrets remain active until they expire or are destroyed. If a secret was either manually rotated, or has  automatic rotation enabled, the following status indicators also apply:  \n  \n - _Rotation pending._ Automatic rotation for the secret is being processed.  \n - _Rotation failed._ Automatic rotation for the secret was not completed. |
| Deactivated | The secret was not created or processed. Secrets in this state are not recoverable and can only be deleted from the instance. |
| Destroyed | When the data that is associated with a secret expires, it moves to the **Destroyed** state. Secrets in this state are not recoverable and can only be deleted from the instance. Metadata that is associated with a secret, such as the secret's transition history and name, is kept in the {{site.data.keyword.secrets-manager_short}} database. If a secret expires after an automatic rotation starts, the following status indicators also apply:  \n  \n - _Rotation pending._ Automatic rotation for the secret is being processed.  \n - _Rotation failed._ Automatic rotation for the secret was not completed. |
{: caption="Describes secret states and transitions" caption-side="top"}


## How do I get started?
{: #what-is-secret-get-started}

To get started with secrets, you can go to the **Secrets** page of the {{site.data.keyword.secrets-manager_short}} UI, or check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2) to learn more about creating secrets programmatically.
