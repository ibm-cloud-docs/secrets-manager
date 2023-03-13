---

copyright:
  years: 2023
lastupdated: "2023-03-13"

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
{: shortdesc}

## Managing CBR settings
{: #manage-cbr-settings}

With [context-based restrictions](/docs/account?topic=account-context-restrictions-whatis), you can define and enforce user and service access restrictions to {{site.data.keyword.secrets-manager_short}} resources based on specified criteria. You can control {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions and identity and access management (IAM) policies. These resources include Virtual Private Cloud (VPC) references and Internet Protocol (IP) addresses that are linked to your {{site.data.keyword.secrets-manager_short}} instance.

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

Context-based restrictions protect only the actions that are associated with the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager/secrets-manager-v2). Actions that are associated with the following platform APIs are not protected by context-based restrictions. Refer to the API docs for the specific action IDs.

- [Resource Instance APIs](/apidocs/resource-controller/resource-controller#list-resource-instances)
- [Resource Keys APIs](/apidocs/resource-controller/resource-controller#list-resource-keys)
- [Resource Bindings APIs](/apidocs/resource-controller/resource-controller#list-resource-bindings)
- [Resource Aliases APIs](/apidocs/resource-controller/resource-controller#list-resource-aliases)
- [IAM Policy APIs](/apidocs/iam-policy-management#list-policies)
- [Global Search APIs](/apidocs/search)
- Global Tagging [Attach](/apidocs/tagging#attach-tag) and [Detach](/apidocs/tagging#detach-tag) APIs
- [Context-based Restriction Rule APIs](/apidocs/context-based-restrictions#create-rule)
- [Secrets Manager APIs](/apidocs/secrets-manager)



***If your service has other limitations, list them here_***



## Understanding network zones
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

The API supports defining [network zones](/apidocs/context-based-restrictions#introduction) by connecting to public (for example, cbr.cloud.ibm.com) and private endpoints (for example, private.cbr.cloud.ibm.com).

Use `GET /v1/zones` to list the zones. By using `POST /v1/zones`, you can create a new zone with the appropriate information. For more information, see [Creating network zones by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#network-zones-create-api).

You can determine which services are available by checking for [reference targets](/apidocs/context-based-restrictions#list-available-serviceref-targets). 
{: note}

***Insert your examples here.***


After you create zones, you can [update](/apidocs/context-based-restrictions#replace-zone) or [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

### Creating network zones by using the UI
{: #cbr-create-zone-ui}
{: ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).

Instead of creating a zone by using UI inputs, you can use the JSON code form to create a zone by clicking **Enter as JSON code**. 
{: note}

***Insert your examples here.***



After you create zones, you can also [update](/apidocs/context-based-restrictions#replace-zone) and [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.


### Creating network zones by using the CLI
{: #cbr-create-zone-cli}
{: cli}

You can use the cbr-zone-create command to add network locations, VPCs, and service references to network zones. For more information, see the CBR CLI reference. Add {{site.data.keyword.secrets-manager_short}} to network zones as a service reference to allow {{site.data.keyword.secrets-manager_short}} to access resources and services in your account that are the subject of a rule.

To find a list of available service references, run the `ibmcloud cbr service-ref-targets` [command](/docs/account?topic=cli-cbr-plugin#cbr-cli-service-ref-targets-command). The `service_name` for {{site.data.keyword.secrets-manager_short}} is `_secretsManager_`.
{: tip}


***Insert your examples here.***


## Understanding rules
{: #cbr-rules}

After you create your zones, you can attach the zones to your network resources by creating rules. When you add resources to a rule, you can choose from the available [types of endpoints](/docs/account?topic=account-context-restrictions-whatis#context-restrictions-endpint-type) that are specific to your network topology. 

### Create rules by using the API
{: #cbr-create-rules-api}
{: api}

You can define rules with the API by using the information that you collected from creating network zones.

By using `GET /v1/rules` with the endpoints that you chose, you can view a list of current rules. Use `POST /v1/rules` to create new rules. For more information, see [Creating rules by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#context-restrictions-create-rules-api).


Review the following examples to learn how to create rules for {{site.data.keyword.secrets-manager_short}}. For more information, see the [API docs](/apidocs/context-based-restrictions#create-rule).

***Insert your examples here. Include more complex use cases, like scoping a rule to protect specific APIs.***

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Creating rules by using the UI
{: #cbr-create-rules-ui}
{: ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).


You can use the CBR UI to [add resources and contexts](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules) to your rules. Keep in mind that, when you create context-based restrictions for the IAM Access Groups service, users who don't satisfy the rule can't view any groups in the account, including the public access group. 

Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Also, the rules might not take effect immediately due to synchronization and resource availability.  
{: important}

***Insert your examples here.***

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Create rules by using the CLI
{: #cbr-create-rules-cli}
{: cli}

Review the following examples to learn how to create rules for {{site.data.keyword.secrets-manager_short}}. For more information, see the CBR [CLI reference](/docs/account?topic=cli-cbr-plugin).

***Insert your examples here. Include more complex use cases, like scoping a rule to protect specific APIs.***





## Next steps
{: #cbr-next-steps}

You must follow the creation or modification of zones or rules with adequate testing to ensure access and availability.

Users who attempt to access your resources outside of the defined zones receive `HTTP error 401` when the appropriate rules are not established.
{: note}


