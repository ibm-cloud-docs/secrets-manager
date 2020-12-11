---

copyright:
  years: 2020
lastupdated: "2020-12-11"

keywords: create secrets, add secrets, store secrets, single tenant secret storage, manage secrets 

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# Storing secrets
{: #store-secrets}

With {{site.data.keyword.secrets-manager_full}}, you can create or add secrets that you can centrally manage from the {{site.data.keyword.cloud_notm}} console. Your secrets are stored in a dedicated, single-tenant instance of the service.
{: shortdesc}

## Before you begin
{: #before-store-secrets}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Storing user credentials
{: #store-user-credentials}

You can use {{site.data.keyword.secrets-manager_short}} to store a username and password that you can use to log in to and access a protected service inside or outside of {{site.data.keyword.cloud_notm}}.

### Adding user credentials with the console
{: #user-credentials-ui}

To store a username and password by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Select the **User credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group yet? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Enter the username and password that you want to associate with the secret.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9.  Optional: Enable expiration and rotation options to control the lifespan of the secret.
     1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
     2. To rotate your secret at a 30, 60, or 90-day interval, switch the rotate toggle to **Yes**. 

10. Click **Add**.

### Adding user credentials by using the API
{: #user-credentials-api}

The following example request adds a username and password to your {{site.data.keyword.secrets-manager_short}} service instance.

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/username_password" \
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
          "name": "example-username-password-secret", 
          "description": "Extended description for my secret.",
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "username": "user123", 
          "password": "hHRhuuK2zRRjtehNb_Fj",
          "expiration_date": "2020-12-31T00:00:00Z",
          "labels": [ 
            "dev", 
            "us-south" 
          ] 
        } 
      ] 
    }' 
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

## Storing IAM credentials
{: #store-iam-credentials}

You can use the {{site.data.keyword.secrets-manager_short}} to create dynamic IAM credentials for accessing an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication. When you create your credentials, {{site.data.keyword.secrets-manager_short}} creates a service ID and an API key. Your credentials are dynamically generated only when you or your application needs to access a protected {{site.data.keyword.cloud_notm}} resource. After the credentials reach the end of their lease, {{site.data.keyword.secrets-manager_short}} revokes them automatically on your behalf.

Before you create IAM credentials in {{site.data.keyword.secrets-manager_short}}, you need to configure an IAM secret engine to enable a connection between your {{site.data.keyword.secrets-manager_short}} instance and IAM. For more information, check out [Enabling the IAM secret engine](/docs/secrets-manager?topic=secrets-manager-secret-engines#configure-iam-engine).
{: note}

### Creating IAM credentials with the console
{: #iam-credentials-ui}

To create IAM credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Select the **IAM credentials** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group yet? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Click **Select access group** to determine the scope of access for your IAM credential.

    By selecting an access group from your {{site.data.keyword.cloud_notm}} account, you determine the scope of access to assign to the service ID that is dynamically generated and associated with your new IAM credential. This step ensures that your IAM credentials are scoped with the wanted level of permissions in your {{site.data.keyword.cloud_notm}} account. 
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Set a lease duration or time-to-live (TTL) for the secret. 

    By setting a lease duration for your IAM credential, you determine how long its associated API key remains valid. After the IAM credential reaches the end of its lease, it is revoked automatically.
10. Click **Add**.

### Creating IAM credentials by using the API
{: #iam-credentials-api}

The following example request creates a dynamic service ID and API key, and stores it in your {{site.data.keyword.secrets-manager_short}} service instance.

```bash
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
          "name": "example-IAM-credential", 
          "description": "Extended description for my secret.",
          "access_groups: [
            "e7e1a364-c5b9-4027-b4fe-083454499a20"
          ],
          "secret_group_id": "432b91f1-ff6d-4b47-9f06-82debc236d90",
          "ttl": "12h", 
          "labels": [ 
            "dev", 
            "us-south" 
          ] 
        } 
      ] 
    }' 
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

## Storing arbitrary secrets
{: #store-arbitrary-secrets}

You can use {{site.data.keyword.secrets-manager_short}} to store arbitrary or custom secrets, such as API keys, that you can use inside or outside of {{site.data.keyword.cloud_notm}}.

### Adding arbitrary secrets with the console
{: #arbitrary-ui}

To add an arbitrary secret by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Select the **Other secret type** tile.
5. Add a name and description to easily identify your secret.
6. Select the [secret group](#x9968962){:term} that you want to assign to the secret.

    Don't have a secret group yet? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
7. Select a file or enter the secret value that you want to associate with the secret.
8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Optional: Enable expiration and rotation options to control the lifespan of the secret.
     1. To set an expiration date for the secret, switch the expiration toggle to **Yes**.
10. Click **Add**.

### Adding arbitrary secrets by using the API
{: #arbitrary-api}

The following example request and stores a custom secret in your {{site.data.keyword.secrets-manager_short}} service instance.

```bash
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
          "payload: "secret-data", 
          "expiration_date": "2020-12-31T00:00:00Z",
          "labels": [ 
            "dev", 
            "us-south" 
          ] 
        } 
      ] 
    }' 
```
{: pre}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Create a secret](/apidocs/secrets-manager#create-secret){: external}.

