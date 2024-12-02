---

copyright:
years: 2024
lastupdated: "2024-12-01"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective, secrets manager

subcollection: content-kit

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



# Understanding high availability and disaster recovery for {{site.data.keyword.secrets-manager_short}}
{: #secrets-manager-ha-dr}





[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}



{{site.data.keyword.secrets-manager_short}} is a regional service that fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard plan. For more information about the available {{site.data.keyword.cloud_notm}} regions and data centers for {{site.data.keyword.secrets-manager_short}}, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).


## High availability architecture
{: #sm-ha-architecture}



A {{site.data.keyword.secrets-manager_short}} service instance is provisioned across three zones in a multi-zone region with no single point of failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failure, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance. Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/resiliency?topic=resiliency-client-retry-logic-for-ha) to maintain availability. There is no noticeable degradation of service during a zonal failure.

{{site.data.keyword.secrets-manager_full}} instances are highly available with no configuration required.

## Disaster recovery architecture
{: #sm-disaster-recovery-intro}





To recover from a service instance outage a recovery service instance should be created in a recovery region. In general the recovery service instance should be configured with same data as the source service instance, but there are exceptions to this guidance.

Some secrets will need to be adjusted for the recovery region, for example connection strings or API keys may reference regional specific service instances. These values will be different in the recovery service instance.

The Secret Manager service instance may have customer created dependencies on these optional services, make sure these exist in the recovered region.

Such an instance should be created before the fact (before any potential disater) and kept in-sync with the source instance.
{: #note}

### Disaster recovery features
{: #sm-dr-features}

{{site.data.keyword.secrets-manager_short}} supports the following disaster recovery features:



Plan for the recovery into a recovery region. The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud.](/docs/resiliency?topic=resiliency-dr-approaches) The recovery instance should track data changes to primary service instance for data including groups, secrets, secret versions, certificates, event notifications.

If the disaster does not impact the production service instance, for example data corruption, it may be possible for a customer to repair the data in the service instance in place.

The service supports the following disaster recovery options:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Rotation | Restore previous secret version | The production service instance must be available. There are a limited number of versions in the version history. See [known issues and limits](/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits&interface=ui). |
{: caption="DR features for {{site.data.keyword.secrets-manager_short}}" caption-side="bottom"}

All other disaster recovery options are created and supported by the customer.

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| External source of truth | All secrets created via a script, described below. | Customer must create the script and persist the configuration where it can be used during disaster |
| Backup and restore | Backup a service instance using customer written script. | Customer must create the script and persist the backup copy where it can be used during recovery |
| Live synchronization | Secret changes in production are automatically observed and propagated to recover service instance, see description below | Customer must create and maintain tools. Data corruption will be synchronized to the recovery instance. |
{: caption="Customer DR features for {{site.data.keyword.secrets-manager_short}}" caption-side="bottom"}

#### Rotation feature
{: #sm-rotation-feature}

Secret Manager secrets are generally updated via “rotation” where writing a value results in the creation of a new version of the secret. It may be possible to restore data corruption by restoring secrets from older versions in the production instance. Only a fixed number of versions are persisted. See [managing secret versions](docs/secrets-manager?topic=secrets-manager-version-history).


#### External source of truth customer provided feature
{: #sm-external-source-of-truth-feature}

The customer must create and use some combination of terraform, script or program as the source of truth. First update the source of truth and then use the source of truth to create/update the primary service instance and the recovery service instance. The source must be available to the restore version and is a single point of failure.

In the event of a customer declared disaster in the primary instance the service in the recovery region will be used (Minimal Operation) or the created (Zero Footprint). Redirect your workload components to the recovered instance or optionally insert into the retry code within your application to redirect requests to the second instance (Minimal Operation). 

The repository that contains the source of truth should have point in time recovery like {{site.data.keyword.cos_short}} versioned buckets or Github repositories.

#### Backup and restore customer provided feature
{: #sm-backup-and-restore-feature}

To manually back up your secrets across regions, you must first have an instance of {[sm-short]} in another region. Then, use the following steps to ensure cross-region availability.

List and download secrets from your instance by using the [{[sm-short]} API](/apidocs/secrets-manager/secrets-manager-v2) or CLI.

If you have existing configurations on secrets engines in your instance, you can also retrieve the information programmatically so that it can be re-created in a new instance. For more information, see the [Get the configuration of a secret type API](/apidocs/secrets-manager/secrets-manager-v2#get-configuration).

Add your downloaded secrets to the newly created instance.

Creating an automatic backup of your secrets is possible by automating the manual flow, which can be done in various ways. Check out some of the following examples to see whether one of them might work for you.

#### Live synchronization customer provided feature
{: #sm-live syncrhonization-feature}

It is possible for the customer to create a script or program to download secrets from your primary service instance by using the {[sm-short]} API or and populate the recovery service instance with the data. The script can take advantage of Activity Tracker audit events of the primary instance to keep the recovery instance in sync along with Code Engine. Customer managed backups should be kept to restore from the disaster.

Create a script that periodically downloads all of your secrets and then imports them into your backup instance.

Create a destination and subscription in Event Notifications that points to an IBM Cloud Code Engine action. Configure the action to listen for lifecycle events such as secret_created and secret_rotated. Then, when the action receives the event, the action downloads the secret from one instance and adds it to the backup instance.

{[sm-short]} supports notifications for the different secret types it provides. To learn about the various available lifecycle event types, see [Enabling event notifications](/docs/secrets-manager?topic=secrets-manager-event-notifications).
{: note}
  
### Planning for disaster recovery
{: #sm-features-for-disaster-recovery-feature}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.



| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | IBM provides an instance that is resilient from single point of hardware failure within a zone - no configuration required. |
| Zone failure | IBM provides an instance that is resilient from a zone failure - no configuration required. |
| Data corruption | Use rotation to restore previous secret version in an available service instance. |
| Data corruption | Restore a point in time uncorrupted version from the external source of truth or backup and restore. |
| Regional failure | Switch critical workloads to use the restored version in a recovery region. Restore the instance using external source of truth, backup and restore, or live synchronization. |
{: caption="DR scenarios for {{site.data.keyword.secrets-manager_short}}" caption-side="bottom"}


## Your responsibilities for HA and DR
{: #sm-feature-responsibilities}

The following information can help you create and continuously practice your plan for HA and DR.



The following check list associated with each feature can help you create and practice your plan.

- Rotation
   - Create a test resource instance and practice rotating secrets versions and restoring a secret version.
- External source of truth
   - Verify the source of truth is in a repository that is available at the restore location.
   - Verify the source of truth is not dependent on the disaster region to avoid dependency on a failed region.
- Backup and restore
   - Verify the backup is in a repository that is available at the restore location.
   - Verify the customer written script to restore the data is available at the restore region.
   - Verify the script and backup not dependent on the disaster region to avoid dependency on a failed region. Consider a {{site.data.keyword.cos_short}} cross regional bucket.
- Live syncrhonization
   - Verify the recovery service instance is currently available in the restore region
- For both external source of truth and live syncrhonization:
   - Verify recovery workloads in the recovery region are integrated with the recover service instance
   - Verify regional specific secrets are available in the recovery service instance.


To find out more about responsibility ownership between the customer and {{site.data.keyword.cloud_notm}} for using {{site.data.keyword.secrets-manager_short}}, see [link to your products Shared responsibilities topic](). 

Disaster recovery steps must be practiced on a regular basis. When building your plan consider the following failure scenarios and resolution.

### Customer recovery from BYOK loss

If your service instance was provisioned by using the root key from either {{site.data.keyword.keymanagementservicefull}} or {{site.data.keyword.hscrypto}} and you accidentally deleted the root key, open a support case for the respective service, and include the following information:
- Your service instance's CRN
- Your backup Key Protect or HPCS instance's CRN
- The new Key Protect or HPCS root key ID
- The original Key Protect or HPCS instance's CRN and key ID, if available

See Recovering from an accidental key loss for authorization in the Key Protect and HPCS docs.


## Recovery Time Objective (RTO) and Recovery Point Objective (RPO)
{: #sm-rto-rpo-features}



| Feature | RTO and RPO |
| -------------- | -------------- |
| Restore previous secret version | RTO = minutes, practice and potentially scripting will improve RTO times, RPO = 0. |
|External source of truth - zero footprint | RTO = few minutes. Amount of time to provision and populate with data. Also consider the amount of time to adjust recovered workloads to the new service instance endpoint. RPO = 0, the source of truth is changed before making production changes. |
| External source of truth - fully operational backup | RTO = few seconds, RPO = 0. Enhance the zero footprint description to keep a live service instance in the recovery region. |
| Backup and restore | RTO = few minutes. Amount of time to provision and populate with data. Also consider the amount of time to adjust recovered workloads to the new service instance endpoint. RPO = time of last backup. |
| Live synchronization | RTO = minutes, RPO = minutes if event driven or the period, for example daily, if using periodic backups. |
{: caption="RTO/RPO features for {{site.data.keyword.secrets-manager_short}}" caption-side="bottom"}

When creating a new service instance the RTO of the workload using {{site.data.keyword.secrets-manager_short}} will include the time required to adjust recovered workloads to the new service instance endpoint.

## Change management
{: #sm-change-management}



Change management includes tasks such as upgrades, configuration changes, and deletion.

It is recommended that you grant users and processes the IAM roles and actions with the least privilage required for their work. For example, limit the ability to delete production resources.



## How {{site.data.keyword.IBM}} helps ensure disaster recovery
{: #sm-ibm-disaster-recovery}



{{site.data.keyword.IBM}} takes specific recovery actions in the case of a disaster.

### How {{site.data.keyword.IBM}} recovers from zone failures
{: #sm-ibm-zone-failure}

In the event of a zone failure IBM Cloud will resolve the zone outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action.

### How {{site.data.keyword.IBM}} recovers from regional failures
{: #sm-ibm-regional-failure}

When a region is restored after a failure, IBM will attempt to restore the service instance from the regional state resulting in no loss of data and the service instance restored with the same connection strings.

- RTO = few minutes
- RPO = 0 minutes

If regional state is corrupted the service will be restored to the state of the last internal backup.  All data associated with the service is backed up once daily by the service in a cross-region Cloud Object Storage bucket managed by the service. There is a potential for 24-hour’s worth of data loss. **These backups are not available for customer managed disaster recovery.** When a service is recovered from backups the instance ID will be restored as well so clients using the endpoint will not need to be updated with new connection strings.

- RTO = 2 hours
- RPO = 24 hours maximum

In the event that IBM can not restore the service instance, the customer must restore as described in the disaster recovery section above.

## How {{site.data.keyword.IBM}} maintains services
{: #sm-ibm-service-maintenance}


All upgrades follow the {{site.data.keyword.IBM}} service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that are handled by [client availability retry logic](/docs/resiliency?topic=resiliency-client-retry-logic-for-ha). Changes are rolled out sequentially, region by region and zone by zone within a region. Updates are backed out at the first sign of a defect.


Complex changes are enabled and disabled with feature flags to control exposure.


Changes that impact customer workloads are detailed in notifications. For more information, see [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
