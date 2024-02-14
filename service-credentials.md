---

copyright:
  years: 2020, 2024
lastupdated: "2024-02-14"

keywords: Service credentials, App ID, App Config, Cloudant, Cloud Object Storage, Event Notifications, Event Streams, etcd, ElasticSearch, PostgreSQL, Redis, MongoDB

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

# Creating service credentials
{: #service-credentials}

You can use {{site.data.keyword.secrets-manager_full}} to create service credentials that you might use to access an {{site.data.keyword.cloud_notm}} resource that requires authentication.
{: shortdesc}

Service credentials can hold Identity and Access Management issued credentials. The credentials can also be service-specific native credentials such as HMAC keys, a database user ID and password, SASL credentials, or TLS certificates. To learn more about the types of secrets that you can manage by using {{site.data.keyword.secrets-manager_short}}, see [What is a secret?](/docs/secrets-manager?topic=secrets-manager-what-is-secret).

Note that in the case of service credentials created for Databases, if in addition to the credential you are also altering the database permissions for the created credential, these will not be synced once the service credential was rotated. When rotating a Databases service credential, this is considered an identity rotation.
{: important}


## Supported {{site.data.keyword.cloud_notm}} services
{: #service-credentials-supported-services}

You can create service credentials through {{site.data.keyword.secrets-manager_short}} for the following {{site.data.keyword.cloud_notm}} services:

- [App ID](/docs/account?topic=account-service_credentials)
- [App Config](/docs/app-configuration?topic=app-configuration-ac-service-credentials)
- [Cloudant](/docs/Cloudant?topic=Cloudant-locating-your-service-credentials)
- [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials)
- [Databases for Datastax](/docs/databases-for-cassandra?topic=databases-for-cassandra-user-management&interface=ui)
- [Databases for EDB](/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-connection-strings&interface=ui)
- [Databases for Elasticsearch](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-user-management&interface=ui#user-management-ui)
- [Databases for etcd](/docs/databases-for-etcd?topic=databases-for-etcd-user-management&interface=ui#user-management-service-cred-users)
- [Databases for PostgreSQL](/docs/databases-for-postgresql?topic=databases-for-postgresql-user-management&interface=ui#user-management-service-cred)
- [Databases for MySQL](/docs/databases-for-mysql?topic=databases-for-mysql-user-management&interface=ui)
- [Databases for MongoDB](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management&interface=ui#user-management-ui)
- [Event Notifications](/docs/event-notifications?topic=event-notifications-en-service-credentials)
- [Event Streams](/docs/EventStreams?topic=EventStreams-connecting)


## Before you begin
{: #before-service-credentials}

Before you get started, be sure that you have the required level of access. To create or add secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


An account administrator, or any entity with the required level of access, can externally alter service credentials that are created and managed by {{site.data.keyword.secrets-manager_short}}. If such a credential is deleted outside of {{site.data.keyword.secrets-manager_short}}, the service might behave unexpectedly. For example, you might be unable to create, or rotate credentials.
{: important}


Note that in the case of service credentials created for Databases, if in addition to the credential you are also altering the database permissions for the created credential, these will not be synced once the service credential was rotated.
{: important}


### Assigning IAM service access role for Service credentials
{: #service-credentials-iam-access-role}

In order to create a service credential, an **IAM service access role** must be selected. The available roles to select from may differ between the supported services. [See list of supported services](/docs/secrets-manager?topic=secrets-manager-service-credentials&interface=ui#supported-ibm-cloud-services) for related documentation. The selected role is then attached to an **IAM Service ID** that can be either an existing Service ID, or an auto-generated one.

The  Service ID continues to be used once secret rotation takes place. If deleting a secret, a pre-existing Service ID will not be deleted, however an auto-generated Service ID will be deleted.
{: note}

If selecting to use a pre-existing Service ID, you can also pre-configure its service access policies. In such a case, select **None** as the Role when creating the secret. [Learn more about IAM policies](/docs/account?topic=account-iamusermanpol).
{: note}


### Service credentials best practices
{: #service-credentials-best-practices}

It is recommended to apply the principle of least privilege access for production use-cases:

1. When setting service-to-service authorization, it should be defined between concrete source and target service **instances**, not between **all** services.
2. When manually setting access for Service IDs to be used with a service credential, it should be applied for a concrete service instance.


## Creating Service credentials in the UI
{: #service-credentials-ui}
{: ui}

To create Service credentials by using the {{site.data.keyword.secrets-manager_short}} UI, complete the following steps.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. From the list of secret types, click the **Service credentials** tile.
5. Click **Next**.
6. Add a name and description to easily identify your secret.
7. Select the [secret group](#x9968962){: term} that you want to assign to the secret.

    Don't have a secret group? In the **Secret group** field, you can click **Create** to provide a name and a description for a new group. Your secret is added to the new group automatically. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups). 

8. Optional: Add labels to help you to search for similar secrets in your instance.
9. Select the desired service and service instance to create a credential for

   If this is the first time the service instance is selected or a service CRN was provided, first authorize {{site.data.keyword.secrets-manager_short}} to access it
   1. Click on **Authorize** and select **Key Manager**
   
10. Click **Next**.
11. Provide the requested input, depending on the selected service. 
12. Optional: Add metadata to your secret or to a specific version of your secret.
    1. Upload a file or enter the metadata and the version metadata in JSON format.  
13. Optional: Set a lease duration or time-to-live (TTL) for the secret.

    By setting a lease duration for your Service credential, you determine how long its associated credential remains valid. After the Service credential reaches the end of its lease, it is revoked automatically.
    {: note}

14. Optional: Enable automatic rotation of your secret.
14. Click **Next**.
15. Review the details of your secret. 
16. Click **Add**.


## Creating Service credentials from the CLI
{: #service-credentials-cli}
{: cli}

To create a Service credential secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. You can specify the type of secret by using the `--secret-type service_credentials` option. For example, the following command creates a Service credential for a Cloud Object Storage instance, with HMAC support. As well as specifying the IAM role to provide for this credential, eg `Writer`.

```sh
ibmcloud secrets-manager secret-create --secret-type="service_credentials" --secret-name="example-service-credentials-secret" --secret-source-service='{"instance": {"crn": "CRN of the instance to create a credential for"},"parameters": {"HMAC": true},"role": {"crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer"}}'
```
{: pre}


The command outputs the ID value of the secret, along with other metadata. For more information about the command options, see [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command).


## Creating Service credentials with the API
{: #service-credentials-api}
{: api}

You can create Service credentials programmatically by calling the {{site.data.keyword.secrets-manager_short}} API.

You can store metadata that are relevant to the needs of your organization with the `custom_metadata` and `version_custom_metadata` request parameters. Values of the `version_custom_metadata` are returned only for the versions of a secret. The custom metadata of your secret is stored as all other metadata, for up to 50 versions, and you must not include confidential data.
{: curl}

In the below example, a service crednetial for Cloud Object Storage is created, with custom parameters with an existing Service ID's IAM ID, and enabling HMAC. As well as specifying the IAM role to provide for this credential, eg `Writer`.

```sh
curl -X POST  
    -H "Authorization: Bearer {iam_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{ 
      {
        "name": "example-service-credentials-secret",
        "description": "Description of my Service Credentials secret",
        "secret_type": "service_credentials",
        "secret_group_id": "bfc0a4a9-3d58-4fda-945b-76756af516aa",
        "labels": [
          "dev",
          "us-south"
        ],
        "source_service": {
          "instance": {
            "crn": "CRN of the instance to create a credential for"
          },
          "parameters": {
            "serviceid_crn": "Existing Service ID's IAM ID",
            "HMAC": true
          },
          "role": {
            "crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer"
          }
        }
      }' \ "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets"
```
{: codeblock}
{: curl}



A successful response returns the ID value of the secret, along with other metadata. For more information about the required and optional request parameters, check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2#create-secret).



