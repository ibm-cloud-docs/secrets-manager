---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-11"

keywords: secret locks, lock secret, prevent deletion, prevent rotation, unlock secret, create lock, delete lock

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

# Locking secrets
{: #secret-locks}

When you work with {{site.data.keyword.secrets-manager_full}}, you can create locks on your secrets to prevent them from being deleted or modified while they're in use by your applications.
{: shortdesc}

By default, an authorized user or application can modify the secrets that you manage in {{site.data.keyword.secrets-manager_short}} at any time. Sometimes, for example during a security audit, you might want to prevent someone on your team from accidentally deleting a secret. Or, if you plan to rotate your secrets regularly, you might be looking for a way to safely deploy the newest version of a secret after a rotation takes place. With locks, you can build automated workflows that help you to:

- Indicate that a secret is in use by one or more applications or services.
- Prevent secret data from being deleted even after a secret expires.
- Safely delete older versions of secrets after the newest version is fully deployed to your applications.
- Avoid inadvertent downtime in your applications.

To learn about the suggested guidelines for using locks to avoid application downtime, check out [Best practices for rotating and locking secrets](/docs/secrets-manager?topic=secrets-manager-best-practices-rotate-secrets#best-practices-lock-secrets).
{: tip}

## Before you begin
{: #before-secret-locks}

Before you begin, be sure that you have the required level of access. To manage locks on your secrets, you need the [**Manager** service role](/docs/secrets-manager?topic=secrets-manager-iam).

## Locking secrets
{: #create-secret-locks}

Locking a secret prevents any operation that can result in modifying or deleting its secret data. To lock a secret, you attach one or more locks to its current or previous version.

- When you try to modify or delete a secret while it is locked, {{site.data.keyword.secrets-manager_short}} denies the request with an HTTP `412 Precondition Failed` response. You see an error message similar to the following example:
    
  ```plaintext
  The requested action can't be completed because the secret version is locked.
  ```
  {: screen}

  If you're working with [dynamic secrets](#x9968958){: term}, such as IAM credentials, locking your secrets also means that by default, those secrets can't be read or accessed. For more information, see [Why can't I read a locked IAM credentials secret?](/docs/secrets-manager?topic=secrets-manager-locked-iam-credentials)
  {: note}

- If a locked secret reaches its expiration date, it stays in the **Active** state and its data remains accessible to your applications. {{site.data.keyword.secrets-manager_short}} moves the secret to the **Destroyed** state and permanently deletes the expired secret data only after all locks on the secret are removed.

  SSL/TLS certificates still reach their defined expiration dates and move into a **Destroyed** state even if they are locked. For more information, see [Why did my locked certificate move to the Destroyed state?](/docs/secrets-manager?topic=secrets-manager-locked-certificates) 
  {: important}

- If you try to rotate a secret while its current version is locked and the previous version is unlocked (or if an automatic rotation is scheduled), the request to rotate the secret is allowed. The current secret version becomes the new previous version, retaining its existing locks. A new current version is created without any locks.
- If you try to rotate a secret while its previous version is locked (or if an automatic rotation is scheduled), your request to rotate the secret is denied. Rotation is allowed only after all locks on the previous secret version are removed.


### Creating locks in the UI
{: #create-lock-ui}
{: ui}

You can create up to 1000 locks on a secret by using the {{site.data.keyword.secrets-manager_short}} UI. Each lock can be used to represent a single application or service that uses your secret.

A secret is considered locked after you attach one or more locks to it. A lock can be applied only on a secret version that contains active payload, or secret data.
{: note}

To help you to create a new lock and remove older locks in a single operation, you can also specify an optional mode at lock creation. 

| Mode | Description |
| --- | --- |
| Lock a secret exclusively | Removes any other locks that match the name that you specify. If any matching locks are found in the previous version of the secret, those locks are deleted when your new lock is created.  \n  \n For example, suppose that the previous version of your secret contains a lock `lock-x`. Creating a lock on the current version of your secret and enabling the **Make this lock exclusive** option results in removing `lock-x` from the previous version. |
| Lock a secret exclusively and delete previous version data  | Same as the previous option, but also permanently deletes the data of the previous secret version if it doesn't have any locks that are associated with it.  \n  \n Suppose that the previous version of your secret contains a lock `lock-z`. Creating a lock on the current version of your secret with both the **Make this lock exclusive** and **Delete previous version data** options results in removing `lock-z` from the previous version. Additionally, because the previous version doesn't have any other locks that are attached to it, the secret data that is associated with the previous version is also deleted. |
{: caption="Table 1. Optional lock modes and their descriptions" caption-side="top"}



#### Creating a lock on the current secret version
{: #create-lock-current-version-ui}

You can lock the current version of a secret by using the {{site.data.keyword.secrets-manager_short}} UI. A successful request attaches a new lock to the current version of your selected secret, or replaces a lock of the same name if it already exists.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to lock, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Locks > Create lock**.
5. Add a name and description to easily identify the lock.
6. From the list of versions to lock, select **Current**.
7. Optional: Attach JSON attributes to your lock.

   You can include a JSON object with each lock to hold any information that you might need for an automated flow. For example, a key-value pair that identifies the resource that you want to associate with this lock.

8. Optional: Make the lock exclusive.

   Choose this option to remove any other locks that match the name that you provided. If any matching locks are found in the previous version of the secret, those locks are deleted when your new lock is created.

9. Optional: Delete previous version data.

   Choose this option to also permanently delete the data of the previous secret version if it doesn't have any locks that are associated with it.

10. Click **Create**.

    A new lock is created for your selected secret version. 

#### Creating a lock on the previous secret version
{: #create-lock-previous-version-ui}

You can lock the previous version of a secret by using the {{site.data.keyword.secrets-manager_short}} UI. A successful request attaches a new lock to the previous version of your selected secret, or replaces a lock of the same name if it already exists.

3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to lock, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Locks > Create lock**.
5. Add a name and description to easily identify the lock.
6. From the list of versions to lock, select **Previous**.
7. Optional: Attach JSON attributes to your lock.

   You can include a JSON object with each lock to hold any information that you might need for an automated flow. For example, a key-value pair that identifies the resource that you want to associate with this lock.

8. Click **Create**.

    A new lock is created for your selected secret version.



### Creating locks with the API
{: #create-lock-api}
{: api}

You can create up to 1000 locks on a secret by using the {{site.data.keyword.secrets-manager_short}} API. Each lock can be used to represent a single application or consumer that uses your secret. A successful request attaches a new lock to your secret, or replaces a lock of the same name if it already exists.

A secret is considered locked after you attach one or more locks to it. A lock can be applied only on a secret version that contains active payload, or secret data.
{: note}

To help you to create a new lock and remove older locks in a single operation, you can also specify an optional mode at lock creation. 

| Mode | Query parameter | Description |
| --- | --- | --- |
| Lock a secret exclusively | `mode=exclusive` | Removes any other locks that match the name that you specify. If any matching locks are found in the previous version of the secret, those locks are deleted when your new lock is created.  \n  \n For example, suppose that the previous version of your secret contains a lock `lock-x`. Creating a lock with the `exclusive` mode on the current version of your secret results in removing `lock-x` from the previous version. |
| Lock a secret exclusively and delete previous version data | `mode=exclusive_delete` | Same as the `exclusive` option, but also permanently deletes the data of the previous secret version if it doesn't have any locks that are associated with it.  \n  \n Suppose that the previous version of your secret contains a lock `lock-z`. Creating a lock with the `exclusive_delete` mode on the current version of your secret results in removing `lock-z` from the previous version. Additionally, because the previous version doesn't have any other locks that are attached to it, the secret data that is associated with the previous version is also deleted. |
{: caption="Table 1. Optional lock modes and their descriptions" caption-side="top"}

To use an optional lock mode, include it as a query parameter on the URI path in your API request. For example, `https://{base_url}/api/v1/secrets/{secret_type}/{id}/lock?mode=exclusive`. For more information, see the [API reference](/apidocs/secrets-manager#lock-secret).
{: tip}



#### Creating locks on the current secret version
{: #create-lock-current-version-api}

The following request creates two locks on the current version of a secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. Allowable values for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.



```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}/lock" \
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -d '{
      "locks": [
         {
            "name": "lock-1",
            "description": "lock for consumer-1",
            "attributes": {
              "key": "value"
            }
         },
         {
            "name": "lock-2",
            "description": "lock for consumer-2",
            "attributes": {
              "key": "value"
            }
         }
      ]
    }'
```
{: codeblock}
{: curl}




If you're building an automated flow, you can use the `attributes` object to specify key-value data with each lock on your secret. For example, you can include a resource identifier, such as an ID or Cloud Resource Name (CRN).
{: tip}

A successful response returns details about the new locks, along with other metadata.



```json
{
  "metadata": {
    "collection_type": "application/vnd.ibm.secrets-manager.secret.lock+json",
    "collection_total": 1
  },
  "resources": [
    {
      "secret_id": "24ec2c34-38ee-4038-9f1d-9a629423158d",
      "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
      "versions": [
        {
          "id": "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
          "alias": "current",
          "locks": [
            "lock-1",
            "lock-2"
          ],
          "payload_available": true
        },
        {
          "id": "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
          "alias": "previous",
          "locks": [],
          "payload_available": true
        }
      ]
    }
  ]
}
```
{: screen}




For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager#lock-secret).

#### Creating locks on the previous secret version
{: #create-lock-previous-version-api}

The following request creates two locks on the previous version of a secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance. Allowable values for `{secret_type}` are: `arbitrary`, `iam_credentials`, `imported_cert`, `kv`, `private_cert`, `public_cert`, and `username_password`.



```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}/versions/previous/lock" \
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -d '{
      "locks": [
         {
            "name": "lock-3",
            "description": "lock for consumer-3",
            "attributes": {
              "key": "value"
            }
         },
         {
            "name": "lock-4",
            "description": "lock for consumer-4",
            "attributes": {
              "key": "value"
            }
         }
      ]
    }'
```
{: codeblock}
{: curl}



A successful response returns details about the new locks, along with other metadata.



```json
{
  "metadata": {
    "collection_type": "application/vnd.ibm.secrets-manager.secret.lock+json",
    "collection_total": 1
  },
  "resources": [
    {
      "secret_id": "24ec2c34-38ee-4038-9f1d-9a629423158d",
      "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712",
      "versions": [
        {
          "id": "7bf3814d-58f8-4df8-9cbd-f6860e4ca973",
          "alias": "current",
          "locks": [
            "lock-1",
            "lock-2"
          ],
          "payload_available": true
        },
        {
          "id": "5bf89b0c-df55-c8d5-7ad6-8816951c6784",
          "alias": "previous",
          "locks": [
            "lock-3",
            "lock-4"
          ],
          "payload_available": true
        }
      ]
    }
  ]
}
```
{: screen}





For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager#lock-secret).



## Unlocking secrets
{: #delete-secret-locks}

A secret is considered unlocked and able to be modified or deleted only after all of its associated locks are removed. You can use the {{site.data.keyword.secrets-manager_short}} UI or APIs to delete locks that are associated with a secret.

### Deleting locks in the UI
{: #delete-lock-ui}
{: ui}

You can delete a lock that is attached to an existing secret by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to the **Secrets** list.
4. In the row for the secret that you want to update, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Locks**.
5. In the row for the lock that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.
6. To confirm the deletion, type the name of the secret. Click **Delete**.

   Your lock is now deleted. To completely unlock the secret, you can remove all existing locks.

### Deleting locks with the API
{: #delete-lock-api}
{: api}

You can use the {{site.data.keyword.secrets-manager_short}} API to delete one or more locks that are associated with the specific secret version.

A successful request deletes the locks that you specify. To remove all locks, you can pass `{"locks": ["*"]}` in the request body. Otherwise, specify the names of the locks that you want to delete. For example, `{"locks": ["lock-1", "lock-2"]}`.

To understand whether a secret contains locks, check the `locks_total` field that is returned as part of the metadata of your secret.
{: note}



```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/{secret_type}/{id}/versions/{id}/unlock" \
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -d '{
    "locks": [
      "lock-1",
      "lock-2"
    ]
  }'
```
{: codeblock}
{: curl}



For more information about the required and optional request parameters, see the [API reference](/apidocs/secrets-manager#unlock-secret).



