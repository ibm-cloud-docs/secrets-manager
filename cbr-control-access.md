---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: context-based restrictions, access allowlist, network security, Vault Dedicated, API types, standard API, vault dedicated management API, vault dedicated runtime API

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

# Protecting {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions
{: #access-control-cbr}

After you set up your {{site.data.keyword.secrets-manager_full}} service instance, you can manage access by using [context-based restrictions (CBR)](https://cloud.ibm.com/context-based-restrictions/overview){: external}.


## Managing CBR settings
{: #manage-cbr-settings}

With [context-based restrictions](/docs/iam?topic=iam-context-restrictions-create), you can define and enforce user and service access restrictions to {{site.data.keyword.secrets-manager_short}} resources based on specified criteria.
{: shortdesc}

You can control {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions and identity and access management (IAM) policies. These restrictions work with traditional IAM policies, which are based on identity, to provide another layer of protection. For more information, see [What are context-based restrictions](/docs/iam?topic=iam-context-restrictions-whatis).

A user must have the `Administrator` role on the {{site.data.keyword.secrets-manager_short}} service to create, update, or delete rules. A user must also have either the `Editor` or `Administrator` role on the context-based restrictions service to create, update, or delete network zones. A user with the `Viewer` role on the context-based restrictions service can add only network zones to a rule.
{: note}

Any {{site.data.keyword.cloudaccesstraillong_notm}} or audit log events that are generated come from the context-based restrictions service, not {{site.data.keyword.secrets-manager_short}}. For more information, see [Monitoring context-based restrictions](/docs/iam?topic=iam-cbr-monitor).

To get started with protecting your {{site.data.keyword.secrets-manager_short}} resources with context-based restrictions, see the tutorial for [Leveraging context-based restrictions to secure your resources](/docs/iam?topic=iam-context-restrictions-tutorial).

## How {{site.data.keyword.secrets-manager_short}} integrates with context-based restrictions
{: #cbr-overview}

You can create context-based restrictions (CBR) for {{site.data.keyword.secrets-manager_short}} service APIs and platform APIs. With context-based restrictions, you can protect the following API types.

[Trial and Standard]{: tag-blue} APIs
:   Protect access to the APIs used by applications and clients to manage and access standard {{site.data.keyword.secrets-manager_short}} resources and perform secret management operations. For example, you can protect the APIs used to manage secret groups, configurations, and notifications registrations, as well as the APIs used to create, read, rotate, or lock secrets and their versions. This API type applies only to instances on the Trial and Standard plans.

[Vault Dedicated]{: tag-green} management APIs
:   Protect access to the APIs used by applications and clients to manage, configure, and perform administrative operations on Vault Dedicated resources. For example, you can protect the APIs used for admin token creation and revocation. This API type applies only to instances on the Vault Dedicated plan.

[Vault Dedicated]{: tag-green} runtime APIs
:   Protect access to the APIs used by applications and clients to access and consume Vault Dedicated resources during runtime. This API type applies only to instances on the Vault Dedicated plan.

Platform APIs — Resource management
:   Protect access to the platform-level APIs used to manage the lifecycle of your {{site.data.keyword.secrets-manager_short}} service instance, such as provisioning, de-provisioning, and managing resource keys and bindings.

To restrict access, you must create [zones](/docs/iam?topic=iam-context-restrictions-create&interface=ui#network-zones-create) and [rules](/docs/iam?topic=iam-context-restrictions-create&interface=ui#context-restrictions-create-rules). After you create or update a zone or a rule, it might take a few minutes for the change to take effect.

### Protecting specific APIs
{: #cbr-specific-apis}

You can create CBR rules to protect the following API types for {{site.data.keyword.secrets-manager_short}}.

#### Standard and Trial plan APIs
{: #cbr-api-type-standard}

[Trial and Standard]{: tag-blue} APIs are used by applications and clients to manage and access {{site.data.keyword.secrets-manager_short}} resources and perform secret management operations.

CBR rules that apply to the [Trial and Standard]{: tag-blue} API type control access to secret management and service administration operations, which include managing secret groups, configurations, destinations, and notifications registrations, viewing instance details and endpoints, and creating, reading, rotating, importing, revoking, and deleting secrets and their versions, managing secret version data, metadata, and policies, and managing locks on secrets and secret versions.

This API type applies only to {{site.data.keyword.secrets-manager_short}} instances on the [Trial and Standard]{: tag-blue} plans.
{: note}

If you use the CLI, you can specify the `--api-types` option and the `crn:v1:bluemix:public:secrets-manager::::api-type:standard` type.

If you use the API, you can specify `"api_type_id": "crn:v1:bluemix:public:secrets-manager::::api-type:standard"` in the `"operations"` spec.

#### Vault Dedicated management APIs
{: #cbr-api-type-vault-dedicated-management}

[Vault Dedicated]{: tag-green} management APIs are used by applications and clients to manage, configure, and perform administrative operations on Vault Dedicated resources.

CBR rules that apply to the Vault Dedicated management API type control access to Vault Dedicated administration operations, configuration operations, which include admin token creation and revocation.

This API type applies only to {{site.data.keyword.secrets-manager_short}} instances on the Vault Dedicated plan.
{: note}

If you use the CLI, you can specify the `--api-types` option and the `crn:v1:bluemix:public:secrets-manager::::api-type:vault-dedicated-management` type.

If you use the API, you can specify `"api_type_id": "crn:v1:bluemix:public:secrets-manager::::api-type:vault-dedicated-management"` in the `"operations"` spec.

#### Vault Dedicated runtime APIs
{: #cbr-api-type-vault-dedicated-runtime}

Protect access to the APIs used by applications and clients to access and consume Vault Dedicated resources during runtime.

This API type applies only to {{site.data.keyword.secrets-manager_short}} instances on the Vault Dedicated plan.
{: note}

If you use the CLI, you can specify the `--api-types` option and the `crn:v1:bluemix:public:secrets-manager::::api-type:vault-dedicated-runtime` type.

If you use the API, you can specify `"api_type_id": "crn:v1:bluemix:public:secrets-manager::::api-type:vault-dedicated-runtime"` in the `"operations"` spec.

#### Platform APIs — Resource management
{: #cbr-api-type-resource-management}

Protect access to the platform-level APIs used to manage the lifecycle of your {{site.data.keyword.secrets-manager_short}} service instance.

If you use the CLI, you can specify the `--api-types` option and the `crn:v1:bluemix:public:secrets-manager::::api-type:platform-resource-management` type.

If you use the API, you can specify `"api_type_id": "crn:v1:bluemix:public:secrets-manager::::api-type:platform-resource-management"` in the `"operations"` spec.

## Creating network zones
{: #cbr-network-zones}

To create network zones, follow the steps in [Creating context-based restrictions](/docs/iam?topic=iam-context-restrictions-create). When you add {{site.data.keyword.secrets-manager_short}} as a service reference to a network zone, use `secrets-manager` as the `serviceRef` value.

The `serviceRef` attribute for {{site.data.keyword.secrets-manager_short}} is `secrets-manager`.
{: tip}

Make sure to add {{site.data.keyword.secrets-manager_short}} to network zones for rules that target other {{site.data.keyword.cloud_notm}} resources, or some operations in your workflow might fail.
{: important}

## Understanding rules
{: #cbr-rules}

To create rules, follow the steps in [Creating context-based restrictions](/docs/iam?topic=iam-context-restrictions-create). When you create a rule for {{site.data.keyword.secrets-manager_short}}, select **Secrets Manager** as the service, then choose the API types you want to protect under **Service APIs** or **Platform APIs**:

**Service APIs**
- **Standard and Trial** — Applies only to instances on the Trial and Standard plans.
- **Vault Dedicated Management** — Applies only to Management API's of instances on the Vault Dedicated plan.
- **Vault Dedicated Runtime** — Applies only to Runtime API's of instances on the Vault Dedicated plan.

**Platform APIs**
- **Resource Management** - Applies only to Resource Controller and Global Search APIs.

## Limitations
{: #cbr-limitations}

Review the following limitations before you create CBR rules for {{site.data.keyword.secrets-manager_short}}.

**Secret group rules require group-level IAM access**
:   When a user has instance-level IAM access, CBR rules that are applied to specific secret groups do not take effect. To work around this limitation, set the user's IAM access policies to only secret groups.

**CBR rules do not apply to provisioning or de-provisioning**
:   CBR rules do not restrict provisioning or de-provisioning operations. Use IAM policies to control who can create or delete {{site.data.keyword.secrets-manager_short}} instances.

**Some platform API actions are not protected**
:   Context-based restrictions protect actions associated with the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager/secrets-manager-v2) and the Resource Management API type. The following platform API actions are not protected by context-based restrictions. Refer to the API docs for the specific action IDs.
   - [Resource instance APIs](/apidocs/resource-controller/resource-controller)
   - [Resource keys APIs](/apidocs/resource-controller/resource-controller)
   - [IAM policy APIs](/apidocs/iam-policy-management#list-policies)
   - [Global search APIs](/apidocs/search)
   - Global tagging [Attach](/apidocs/tagging#attach-tag) and [Detach](/apidocs/tagging#detach-tag) APIs
   - [Context-based restriction rule APIs](/apidocs/context-based-restrictions#create-rule)
   - [Secrets Manager APIs](/apidocs/secrets-manager/secrets-manager-v2)

## Next steps
{: #cbr-next-steps}

You must follow the creation or modification of zones or rules with adequate testing to ensure access and availability.

Users who attempt to access your resources outside of the defined zones receive `HTTP error 401` when the appropriate rules are not established.
{: note}
