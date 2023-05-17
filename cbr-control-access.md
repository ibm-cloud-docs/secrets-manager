---

copyright:
  years: 2023
lastupdated: "2023-05-12"

keywords: context-based restrictions, access allowlist, network security

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

# Protecting {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions
{: #access-control-cbr}

After you set up your {{site.data.keyword.secrets-manager_full}} service instance, you can manage access by using [context-based restrictions (CBR)](https://cloud.ibm.com/context-based-restrictions/overview){: external}. 


## Managing CBR settings
{: #manage-cbr-settings}

With [context-based restrictions](/docs/account?topic=account-context-restrictions-whatis), you can define and enforce user and service access restrictions to {{site.data.keyword.secrets-manager_short}} resources based on specified criteria.
{: shortdesc} 

You can control {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions and identity and access management (IAM) policies. These resources include Virtual Private Cloud (VPC) references and Internet Protocol (IP) addresses that are linked to your {{site.data.keyword.secrets-manager_short}} instance.

These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Since both IAM access and context-based restrictions enforce access, context-based restrictions offer protection even in the face of compromised or mismanaged credentials. For more information, see [What are context-based restrictions](/docs/account?topic=account-context-restrictions-whatis).

A user must have the `Administrator` role on the {{site.data.keyword.secrets-manager_short}} service to create, update, or delete rules. A user must also have either the `Editor` or `Administrator` role on the context-based restrictions service to create, update, or delete network zones. A user with the `Viewer` role on the context-based restrictions service can add only network zones to a rule. 
{: note}

Any {{site.data.keyword.cloudaccesstraillong_notm}} or audit log events that are generated come from the context-based restrictions service, not {{site.data.keyword.secrets-manager_short}}. For more information, see [Monitoring context-based restrictions](/docs/account?topic=account-cbr-monitor).

To get started with protecting your {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions, see the tutorial for [Leveraging context-based restrictions to secure your resources](/docs/account?topic=account-context-restrictions-tutorial).


## How {{site.data.keyword.secrets-manager_short}} integrates with context-based restrictions
{: #cbr-overview}

To restrict access, you must create [zones](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create) and [rules](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules). 

First, create a zone with the appropriate details for network or resource definitions. Then, attach that zone to the specified resource to restrict access. You can create zones and rules by using a RESTful [API](/apidocs/context-based-restrictions#introduction) or with [context-based restrictions](https://cloud.ibm.com/context-based-restrictions/overview){: external}. After you create or update a zone or a rule, it might take a few minutes for the change to take effect.

CBR rules do not apply to provisioning or deprovision processes. 
{: note}

## Limitations
{: #cbr-limitations}

When a user has instance level IAM access, CBR rules that are applied to specific secret groups do not take effect. To work around this limitation, set the user's IAM access policies to only secret groups. 

Context-based restrictions protect only the actions that are associated with the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager/secrets-manager-v2). Actions that are associated with the following platform APIs are not protected by context-based restrictions. Refer to the API docs for the specific action IDs.

- [Resource Instance APIs](/apidocs/resource-controller/resource-controller#list-resource-instances)
- [Resource Keys APIs](/apidocs/resource-controller/resource-controller#list-resource-keys)
- [Resource Bindings APIs](/apidocs/resource-controller/resource-controller#list-resource-bindings)
- [Resource Aliases APIs](/apidocs/resource-controller/resource-controller#list-resource-aliases)
- [IAM Policy APIs](/apidocs/iam-policy-management#list-policies)
- [Global Search APIs](/apidocs/search)
- Global Tagging [Attach](/apidocs/tagging#attach-tag) and [Detach](/apidocs/tagging#detach-tag) APIs
- [Context-based Restriction Rule APIs](/apidocs/context-based-restrictions#create-rule)
- [Secrets Manager APIs](/apidocs/secrets-manager/secrets-manager-v2)


## Creating network zones
{: #cbr-network-zones}

By creating network zones, you can define an allowlist of network locations where access requests originate to determine when a rule can be applied. The list of network locations can be specified by the following attributes:

* IP addresses, which include individual addresses, ranges, or subnets.
* VPCs
* Service references, which allow access from other {{site.data.keyword.cloud_notm}} services.

Make sure to add {{site.data.keyword.secrets-manager_short}} to network zones for rules that target other {{site.data.keyword.cloud_notm}} resources, or some operations in your workflow might fail. 
{: important}



### Creating network zones by using the API
{: #cbr-create-zones-api}
{: api}

You can create network zones by using the create-zone command. For more information, see the [API docs](/apidocs/context-based-restrictions#create-zone). You can add {{site.data.keyword.secrets-manager_short}} to network zones as a service reference to allow {{site.data.keyword.secrets-manager_short}} to access resources and services in your account that are the subject of a rule.

The serviceRef attribute for {{site.data.keyword.secrets-manager_short}} is `secrets-manager`. {: tip} 

You can determine which services are available by checking for [reference targets](/apidocs/context-based-restrictions#list-available-serviceref-targets). 
{: note}

Example payload to add {{site.data.keyword.secrets-manager_short}} to a network zone. 

```json
{
  "name": "Example zone 1",
  "description": "",
  "addresses": [
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "secrets-manager",
        "account_id": "ACCOUNT-ID"
      }
    }
  ]
}
```
{: codeblock}


Example payload to add multiple services, IP addresses, and VPCs to a network zone.

```json
{
  "name": "zone",
  "description": "",
  "addresses": [
    {
      "type": "ipAddress",
      "value": "192.168.0.0"
    },
    {
      "type": "vpc",
      "value": "crn:v1:bluemix:public:is:us-east:a/CRN"
    },
    {
      "type": "vpc",
      "value": "crn:v1:bluemix:public:is:us-south:a/CRN"
    },
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "cloud-object-storage",
        "account_id": "ACCOUNT-ID"
      }
    },
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "codeengine",
        "account_id": "ACCOUNT-ID"
      }
    },
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "containers-kubernetes",
        "account_id": "ACCOUNT-ID"
      }
    },
    {
      "type": "serviceRef",
      "ref": {
        "service_type": "platform_service",
        "account_id": "ACCOUNT-ID"
      }
    },
    {
      "type": "serviceRef",
      "ref": {
        "service_name": "iam-groups",
        "account_id": "ACCOUNT-ID"
      }
    }
  ],
  "excluded": []
}
```
{: codeblock}

After you create zones, you can [update](/apidocs/context-based-restrictions#replace-zone) or [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

### Creating network zones by using the UI
{: #cbr-create-zone-ui}
{: ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).

1. Determine the resources that you want add to your allowlist.
2. Follow the steps to [create context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create) in the console. Add the {{site.data.keyword.secrets-manager_short}} service to your network zones to allow {{site.data.keyword.secrets-manager_full}} to access services and resources in your account.

After you create zones, you can also [update](/apidocs/context-based-restrictions#replace-zone) and [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.


### Creating network zones by using the CLI
{: #cbr-create-zone-cli}
{: cli}

You can use the `cbr-zone-create` command to add network locations, VPCs, and service references to network zones. For more information, see the CBR [CLI reference](/docs/account?topic=account-cbr-plugin#cbr-zones-cli). Add {{site.data.keyword.secrets-manager_short}} to network zones as a service reference to allow {{site.data.keyword.secrets-manager_short}} to access resources and services in your account that are the subject of a rule.

1. To create network zones from the CLI, [install the CBR CLI plug-in](/docs/cli?topic=cli-cbr-plugin#install-cbr-plugin). 
1. Use the `cbr-zone-create` command to add resources to network zones. For more information, see the CBR [CLI reference](/docs/cli?topic=cli-cbr-plugin#cbr-zones-cli). Note that the `service_name` for {{site.data.keyword.secrets-manager_short}} is `secrets-manager`.


To find a list of available service references, run the `ibmcloud cbr service-ref-targets` [command](/docs/account?topic=account-cbr-plugin#cbr-cli-service-ref-targets-command).
{: tip}


Example command to add the `secrets-manager` service to a network zone.

```sh
ibmcloud cbr zone-create --name example-zone-1 --description "Example zone 1" --service-ref service_name=secrets-manager
```
{: pre}


## Understanding rules
{: #cbr-rules}

After you create your zones, you can attach the zones to your network resources by creating rules. When you add resources to a rule, you can choose from the available [types of endpoints](/docs/account?topic=account-context-restrictions-whatis#context-restrictions-endpint-type) that are specific to your network topology. 

### Create rules by using the API
{: #cbr-create-rules-api}
{: api}

You can define rules with the API by using the information that you collected from creating network zones.

Review the following example to learn how to create rules for {{site.data.keyword.secrets-manager_short}}. For more information, see the [API docs](/apidocs/context-based-restrictions#create-rule).

The following example payload creates a rule that protects the `CLUSTER-ID` cluster. Only resources in the `NETWORK-ZONE-ID` zone can access the cluster. Given that no `operations` are specified, resources in the `NETWORK-ZONE-ID` zone can access both the `cluster` and `management` APIs.

```sh
{
  "description": "Example rule 1",
  "resources": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "ACCOUNT-ID"
        },
        {
          "name": "serviceName",
          "value": "secrets-manager"
        },
        {
          "name": "serviceInstance",
          "value": "CLUSTER-ID"
        }
      ]
    }
  ],
  "contexts": [
    {
      "attributes": [
        {
          "name": "networkZoneId",
          "value": "NETWORK-ZONE-ID"
        },
        {
          "name": "endpointType",
          "value": "private"
        }
      ]
    }
  ]
}
```
{: codeblock}


After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Creating rules by using the UI
{: #cbr-create-rules-ui}
{: ui}

After you set the prerequisites and requirements, you can create rules in the UI. 

1. Determine the resources that you want add to your allowlist.
2. Follow the steps to [create context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules) in the console. Add the {{site.data.keyword.secrets-manager_short}} service to your network zones to allow {{site.data.keyword.secrets-manager_full}} to access services and resources in your account.

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Create rules by using the CLI
{: #cbr-create-rules-cli}
{: cli}

Review the following examples to learn how to create rules for {{site.data.keyword.secrets-manager_short}}. For more information, see the CBR [CLI reference](/docs/account?topic=cli-cbr-plugin).

1. To create rules from the CLI, [install the CBR CLI plug-in](/docs/cli?topic=cli-cbr-plugin#install-cbr-plugin). 
1. You can use the `ibmcloud cbr rule-create` [command](/docs/cli?topic=cli-cbr-plugin#cbr-cli-rule-create-command) to create CBR rules. For more information, see the CBR [CLI reference](/docs/cli?topic=cli-cbr-plugin#cbr-zones-cli). Note that the `service_name` for {{site.data.keyword.secrets-manager_short}} is `secrets-manager`. To find a list of service names, run the `ibmcloud cbr service-ref-targets` command. To find a list of API types for a service, run the `ibmcloud cbr api-types --service-name SERVICE` command.

Example command to create a rule that uses the `addresses` key and the `cluster` API type and the `ipAddress` type.

```sh
ibmcloud cbr rule-create my-rule-1 --service-name secrets-manager --api-type crn:v1:bluemix:public:secrets-manager::::api-type:cluster --zone-id ZONE-ID
```
{: pre}

The following command creates a rule that protects the `CLUSTER-ID` cluster. Only resources in the `NETWORK-ZONE-ID` network zone can access the cluster. This rule includes both the `cluster` and `management` API types.

```sh
ibmcloud cbr rule-create my-rule-2 --service-name secrets-manager --service-instance CLUSTER-ID --zone-id NETWORK-ZONE-ID 
```
{: pre}


## Next steps
{: #cbr-next-steps}

You must follow the creation or modification of zones or rules with adequate testing to ensure access and availability.

Users who attempt to access your resources outside of the defined zones receive `HTTP error 401` when the appropriate rules are not established.
{: note}


