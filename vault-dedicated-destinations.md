---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: Vault Dedicated destinations, destination resource, private network link, IBM Cloud Databases private endpoint, Vault Dedicated outbound connectivity, create destination, delete destination, destination lifecycle states, ibm_cloud_database, Vault database secrets engine

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
{{site.data.keyword.attribute-definition-list}}

# Configuring outbound connectivity destinations
{: #vault-dedicated-destinations}

Vault Dedicated instances block all outbound connectivity by default. A destination resource is how you explicitly enable the service to establish a private network link between your Vault cluster and a supported external service. Only approved destination types are supported and it is not general-purpose egress.

Destination resources are control-plane objects. You create and manage them through the {{site.data.keyword.secrets-manager_short}} API or CLI, not through the Vault API. When a destination is active, it provides only the network reachability prerequisite for your Vault configuration. Authentication, authorization, certificates, and application-level configuration remain your responsibility.

## Before you begin
{: #vault-dedicated-destinations-before-you-begin}

- You must have the IAM **Writer** or **Manager** role on the Vault Dedicated instance.
- The target {{site.data.keyword.cloud_notm}} Databases instance must be Gen 1 (Classic) and have private endpoints enabled. If the instance is Gen 2 or has no private endpoints, the API returns an HTTP 422 error.
- Have the CRN of the target {{site.data.keyword.cloud_notm}} Databases instance ready.

## Supported destination types
{: #vault-dedicated-destinations-supported-types}

The following table lists the supported destination types that the Vault Dedicated instance supports:

| Type | Description |
|---|---|
| `ibm_cloud_database` | Private network link to an {{site.data.keyword.cloud_notm}} Database service instance (Gen 1 with private endpoints enabled). |

## Destination lifecycle states
{: #vault-dedicated-destinations-lifecycle-states}

Creating and deleting a destination are asynchronous operations. To track progress, check the destination status by sending a `GET /v2/instances/{instance_id}/destinations/{destination_id}` request.

The following table describes each state that a destination can have during its lifecycle:

| State | Meaning |
|---|---|
| `not_started` | Destination created; the first provisioning attempt has not yet begun. |
| `provisioning` | Service is setting up the private network link. Check the destination status until the state changes to `succeeded` or `failed`. |
| `succeeded` | Destination is ready; your Vault configuration can now use the private endpoint. |
| `failed` | Provisioning or deletion failed. Check the `message` field for details. A `failed` destination still counts against your per-instance quota until it is deleted. |
| `deleting` | Deletion accepted and in progress; the destination is removed from the list once complete. |

## Creating a destination
{: #vault-dedicated-destinations-create}

Create a destination to establish a private network link between your Vault Dedicated cluster and an {{site.data.keyword.cloud_notm}} Databases instance.

**Rate limit:** 10 create requests per instance per minute.
**Quota:** Maximum 20 destinations per instance.

### API
{: #vault-dedicated-destinations-create-api}

```sh
POST /v2/instances/{instance_id}/destinations
```

Request body:

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Unique name for the destination within the instance. |
| `type` | Yes | Destination type. Must be `ibm_cloud_database`. |
| `crn` | Yes | CRN of the target {{site.data.keyword.cloud_notm}} Databases service instance. |
| `description` | No | Optional description. |

The API returns a `202 Accepted` response and sets the destination state to `not_started` or `provisioning`. Send a `GET /v2/instances/{instance_id}/destinations/{destination_id}` request until the state changes to `succeeded` or `failed`.

#### Example request
{: #vault-dedicated-destinations-create-example-request}

The following example creates a destination named `my-postgres` that connects to an {{site.data.keyword.cloud_notm}} Databases for PostgreSQL instance by using the CLI.

```sh
ibmcloud secrets-manager-instance-management destination-create \
  --id=<instance_id> \
  --name=my-postgres \
  --type=ibm_cloud_database \
  --crn=crn:v1:bluemix:public:databases-for-postgresql:us-south:a/<account_id>:<db_instance_id>::
```

#### Example response (`202 Accepted`)
{: #vault-dedicated-destinations-create-example-response}

The following example shows the response that the API returns after the destination is created.

```json
{
  "id": "ce8aa9a4-8cde-4b63-8156-d4a8b252beeb",
  "name": "my-postgres",
  "type": "ibm_cloud_database",
  "description": "Production PostgreSQL database",
  "crn": "crn:v1:bluemix:public:databases-for-postgresql:us-south:a/...",
  "state": "provisioning",
  "created_at": "2026-07-09T15:00:00Z",
  "updated_at": "2026-07-09T15:00:00Z",
  "created_by": "iam-ServiceId-abc12345-1234-1234-1234-abc123456789",
  "href": "https://us-south.secrets-manager.cloud.ibm.com/v2/instances/<instance_id>/destinations/ce8aa9a4-8cde-4b63-8156-d4a8b252beeb"
}
```

## Checking destination status
{: #vault-dedicated-destinations-check-status}

Retrieve the current state of a destination at any time.

```sh
GET /v2/instances/{instance_id}/destinations/{destination_id}
```

You can also filter the destination list by state:

```sh
GET /v2/instances/{instance_id}/destinations?state=failed
```

**If a destination is in `failed` state:**

1. Read the `message` field in the response for details about the failure.
2. Delete the failed destination to free up quota.
3. Resolve the underlying issue (for example, ensure the target database instance has private endpoints enabled).
4. Create a new destination.

## Updating a destination
{: #vault-dedicated-destinations-update}

Only `name` and `description` can be updated. The `crn` (connectivity target) is immutable after creation.

```sh
PATCH /v2/instances/{instance_id}/destinations/{destination_id}
```

To change the target, delete the destination and create a new one.

## Deleting a destination
{: #vault-dedicated-destinations-delete}

Deleting a destination revokes the private network path for that instance and returns it to the secure default.

```sh
DELETE /v2/instances/{instance_id}/destinations/{destination_id}
```

The API returns a `202 Accepted` response and sets the destination state to `deleting`. Deletion is asynchronous. After it completes, the destination is removed from the destinations list.

Remove the corresponding Vault configuration, such as the database secrets engine mount, before or alongside deletion to avoid Vault errors caused by a broken network path.
{: important}

## Limitations
{: #vault-dedicated-destinations-limitations}

- Maximum 20 destinations per instance. Destinations in `failed` state count against this quota until deleted.
- Only `ibm_cloud_database` (Gen 1 with private endpoints) is supported.
- The `crn` field is immutable after creation. To change the target, delete and re-create the destination.
- General outbound egress to arbitrary endpoints is not supported.
