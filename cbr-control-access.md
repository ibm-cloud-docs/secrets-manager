---

copyright:
  years: 2023
lastupdated: "2023-02-13"

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

# Managing access with context-based restrictions
{: #access-control-cbr}

After you set up your {{site.data.keyword.secrets-manager_full}} service instance, you can manage access by using [context-based restrictions (CBR)](https://cloud.ibm.com/context-based-restrictions/overview){: external}. 
{: shortdesc}

## Managing CBR settings
{: #manage-cbr-settings}

With [context-based restrictions](/docs/account?topic=account-context-restrictions-whatis), you can define and enforce user and service access restrictions to {{site.data.keyword.secrets-manager_short}} resources based on specified criteria. These resources include Virtual Private Cloud (VPC) references and Internet Protocol (IP) addresses that are linked to your {{site.data.keyword.secrets-manager_short}} instance.

To restrict access, you must be the account owner or have an access policy with the administrator role on all account management services.
{: note}


## Overview
{: #cbr-overview}

To restrict access, you must create [zones](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create) and [rules](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules). 

First, create a zone with the appropriate details for network or resource definitions. Then, attach that zone to the specified resource to restrict access. You can create zones and rules by using a RESTful [API](/apidocs/context-based-restrictions#introduction) or with [context-based restrictions](https://cloud.ibm.com/context-based-restrictions/overview){: external}. After you create or update a zone or a rule, it might take a few minutes for the change to take effect.

CBR rules do not apply to provisioning or deprovision processes. 
{: note}


## Understanding network zones
{: #cbr-network-zones}

By creating network zones, you can define an allowlist of network locations where access requests originate, to determine when a rule can be applied. The list of network locations can be specified by using IP addresses, such as individual addresses, ranges or subnets, and Virtual Private Cloud (VPC) IDs. 

After you create a network zone, you can add it to a rule.

### Creating network zones by using the CBR API
{: #cbr-create-zones-api}


The API supports defining [network zones](/apidocs/context-based-restrictions#introduction) by connecting to public (for example, cbr.cloud.ibm.com) and private endpoints (for example, private.cbr.cloud.ibm.com).

Use `GET /v1/zones` to list the zones. By using `POST /v1/zones`, you can create a new zone with the appropriate information. For more information, including a request body example, see [Creating network zones by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#network-zones-create-api).

You can determine which services are available by checking for [reference targets](/apidocs/context-based-restrictions#list-available-serviceref-targets). 
{: note}


After you create zones, you can [update](/apidocs/context-based-restrictions#replace-zone) or [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

### Creating network zones by using the CBR UI
{: #cbr-create-zone-ui}


After you set the prerequisites and requirements, you can create zones in the UI. For more information about the steps to follow, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).

Instead of creating a zone by using UI inputs, you can use the JSON code form to create a zone by clicking **Enter as JSON code**. 
{: note}


After you create zones, you can also [update](/apidocs/context-based-restrictions#replace-zone) and [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

## Understanding network rules
{: #cbr-network-rules}

After you create your zones, you can attach the zones to your network resources by creating rules. When you add resources to a rule, you can choose from the available [types of endpoints](/docs/account?topic=account-context-restrictions-whatis#context-restrictions-endpint-type) that are specific to your network topology. 

### Create network rules by using the CBR API
{: #cbr-create-rules-api}

You can define network rules with the API by using the information that you collected from creating network zones.

By using `GET /v1/rules` with the endpoints that you chose, you can view a list of current rules. Use `POST /v1/rules` to create new rules. For more information, including a request body example, see [Creating rules by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#context-restrictions-create-rules-api).

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Creating network rules by using the CBR UI
{: #cbr-create-rules-ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information about the steps to follow, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).


You can use the CBR UI to [add resources and contexts](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules) to your network rules. Keep in mind the limitations.

For example, when you create context-based restrictions for the IAM Access Groups service, users who don't satisfy the rule can't view any groups in the account, including the public access group. 

Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Also, the rules might not take effect immediately due to synchronization and resource availability.  
{: important}

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.


## Next steps
{: #cbr-next-steps}

You must follow the creation or modification of zones or rules with adequate testing to ensure access and availability.

Users who attempt to access your resources outside of the defined zones receive `HTTP error 401` when the appropriate rules are not established.
{: note}


