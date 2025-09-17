---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-17"

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



## Video transcript
{: #secrets-mgmt-video-transcript}
{: notoc}

How are you making sure that your secrets are securely stored so that you can avoid data breaches and chaos in your DevOps workflows?

Hi, I'm Alex Greer with the IBM Cloud team, and before I get started make sure to like and subscribe now.

What is a secret?

A secret is a digital credential that is going to allow entities to communicate and perform actions on a service. This discrete piece of information keeps that access point secure. So let's look at how this paradigm exists.

Let's start with an entity here that needs to access some sort of service. We'll leave it ambiguous for now, but some sort of service. To properly communicate with the service and be able to take the action that it needs to get its job done, this entity is going to need to communicate to the service two things: One, who it is, so that service can understand what or who is interacting with it. Two, it's going to have to know the set of permissions that it should grant in the context of its service. With these, the service can now properly allow that entity to interact with them. How we enable this interaction is with something we call a "secret."

Now, with this dynamic established, let's move into an example with users. For users, we'll say that this user here is our entity, and we'll say our service here – let's say that it's a developer, and they happen to need read or write access. They're interacting with the development repo to do that. To gain that access again, coming back to the need, we have authorization and permission. How it's going to communicate, specifically in this circumstance, is by giving it user credentials. Now, that user can interact with the development repo in the way that they need to get their job done.

Now, looking at a cloud native application story, we have many microservices that must talk to each other. So, let's look at that. Let's call it service "A," that needs to interact with a database called "DB" and grab a piece of specific information that it needs to get its job done. What it needs, in the form of the secret here, is what we call "DB configs." Again, this DB config is going to allow it to have the correct communication with the service by saying, this is who I am and this is what I came here to accomplish.

But now that we have our user credentials in our DB configs as examples of secrets, we realize the vulnerability that can be created here if these credentials were to fall into the wrong hands. And this is why it's so important to establish a centralized place to manage all these things as we build out more applications and microservices. As we have more of these, the problem becomes more complex. If it falls into the wrong hands, how is protected? How do we block it from getting to that point? How was that data isolated?

When we look at the damage that this can cause, we're looking in the millions of dollars, for example, for a data breach. In terms of developer operations, if you're not properly managing these, forget even the case in which a bad actor hasn't been involved, but it can be confusing for teams to use this. So what we need to do is make sure that we have it, again, centralized improperly stored so that we can leverage these secrets in the correct way and they can properly communicate with services.

OK, now, let's look at the next layer of the onion in a more complicated example.

Now let's go back to Jane, the enterprise developer. Jane – our "E dev" here – needs again to have access to that development repo that she's referring to earlier. Let's go ahead and call this, maybe it's GitHub. So what she needs to be able to do is have write access and so she's going to need to request the information that's going to give her access to that role. So that's where a secrets manager service comes in in a perfect, complementary fashion.

A secrets manager service can securely store these credentials, along with other types of secrets, in a centralized way for her to be able to access or maybe other services to access, like we'll see in a second. But when it's done, it's given her the peace of mind that her user credentials are securely stored and now she can worry about authenticating with the service and getting her job done because secrets manager is going to take care of that for her.

However, what's really important for secrets manager service to do is interact with the cloud service provider's IAM, or identity and access management service. IAM is going to be the source of truth allowing secrets manager to one, authenticate who she is so that it can pass it down to GitHub, and secondarily also allow for her to get the correct set of roles based on the paradigm that they have within their IAM service. With this, we now understand what it's like for a user to get the correct permission and be able to access the tool of their choice of their service, and be able to do this in the context of using a secrets manager service.

But now, let's look what it's like for a service to interact with another service and potentially where data breaches could be harmful.

Let's look at our service to service example. What we have to start with is a lending application. This lending app is going to want to request permissions to be able to access – again, we were talking about a database earlier. Let's be a bit more specific: this database that it needs to access is a given table within the database has necessary information to give to its model in order to be able to make a judgment on whether they want to provide a consumer alone. So we're going to call this a profile database. So within here, we have profile DB, and we know that the set of permissions that we want to grant are going to be read permissions for table A. So again, where are we going to store the secret that's ultimately going to give us access to give us access to authentication and ultimately to the set of permissions that we need here? So that's again going to be the secrets manager service provided, and that service needs to talk to Cloud IAM again. Now that we've got this established, what type of credential do we have here? The credential that we have is called DB config. So let's think about the scenario we just walked through: this DB config allows this lending application a service to be able to have read access or be able to take a specific set of information from a given table within a database that has some IP data and some other highly sensitive information in it. So, in a scenario in which these DB configs are not stored properly or the service that they stored in is compromised, what we ultimately get is a pretty catastrophic scenario for the provider, because the provider of this service of this lending application then has to go and tell its customer that a bad actor was able to take advantage of access that it got wrongfully to its DB config. And, that DB config ultimately allowed that bad actor to steal their data and do whatever they wanted with it against that customer. What we can do to mitigate it again is stored in a safe location or bad actors do not have access and you have the level of data isolation that you're comfortable with as an enterprise. That's where we have secrets manager services.

Again, secrets manager services help to ensure the secure storage of secrets so that you don't have to worry about data breaches from last credentials or from other types of secrets. And, ultimately, it makes it more efficient for the management of your secrets while you're going through your DevOps operations.

Thank you. If you have questions, please drop us a line. If you want to see more videos like this in the future, please like and subscribe, and don't forget you can grow your skills and earn a badge with IBM Cloud Labs which, are free browser-based interactive Kubernetes labs.

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
| [Custom credentials](/docs/secrets-manager?topic=secrets-manager-custom-credentials) | `custom_credentials` | Static | A JSON containing user-defined sensitive data such as keys, certificates, URLs, passwords, and any kind of artbitrary pieces of data as determined the credentials provider. |
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
