---

copyright:
  years: 2021
lastupdated: "2024-11-01"

keywords:

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}



# Understanding data portability
{: #data-portability}





[Data Portability](#x2113280){: term} involves a set of tools, and procedures that enable customers to export the digital artifacts that would be needed to implement similar workload and data processing on different service providers or on-prem software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

IBM Cloud services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer then is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures.
This can involve:

- the planning and execution for setting up alternate infrastructure on on different cloud providers or on-prem software that provide similar capabilities to the IBM services
- the planning and execution for the porting of the required application code on the alternate infrastructure, including the adaptation of customer's application code, deployment automation, etc.
- the conversion of the exported data and configuration to format required by the alternate infrastructure and adapted applications


To find out more about responsibility ownership for using {{site.data.keyword.cloud}} products between {{site.data.keyword.IBM_notm}} and customer see [Shared responsibilities for {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities).



For more information about your responsibilities when using {{site.data.keyword.secrets-manager_short}}, see [Shared responsibilities for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-understanding-your-responsabilities).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.secrets-manager_short}} provides mechanisms to export your content that uploaded, stored, and processed using the service.  
All data available within the service can be accessed using the {{site.data.keyword.secrets-manager_short}} service APIs as described in the [API documentation](/apidocs/secrets-manager/secrets-manager-v2).

   - To export a secret group use the [Get a secret group](/apidocs/secrets-manager/secrets-manager-v2#get-secret-group) REST API.
   - To export a secret use the [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-secret) REST API.
   - To export a secret engine configuration use the [Get a configuration](/apidocs/secrets-manager/secrets-manager-v2#get-configuration) REST API.

## Exported data formats
{: #data-portability-data-formats}

The format of the data exported from the {{site.data.keyword.secrets-manager_short}} service APIs is JSON.  
The schema of the exported data is described in the {{site.data.keyword.secrets-manager_short}} service [API documentation](/apidocs/secrets-manager/secrets-manager-v2).


[Swagger UI](/docs/secrets-manager?topic=secrets-manager-endpoints&q=openapi&tags=secrets-manager#public-endpoints). 

## Non exportable data
{: #data-portability-non-exportable-data}

The private keys that are internally created to sign Root and Intermediate Certificate Authorities when using the Private Certificate engine are not exportable for security reasons. 
Use Private Certificate with [HPCS Key management service](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#prepare-hpcs) to reuse your own private keys in HPCS.

## Data onwership
{: #data-portability-data-ownership}

All exported data are classified as Customer content and therefore appliy to them the full customer ownership and licensing rights, as stated in [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS).
