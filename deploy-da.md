---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-28"

keywords: Secrets Manager deployable architecture, Secrets Manager automation,

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

# Working with the {{site.data.keyword.secrets-manager_short}} deployable architecture 
{: #deployable-architecture}

**Plan availability**: [Trial and Standard]{: tag-blue} plans only.

{{site.data.keyword.secrets-manager_short}} is available as a deployable architecture called Cloud automation for {{site.data.keyword.secrets-manager_short}}, so you can deploy {{site.data.keyword.secrets-manager_short}} by using the automation available through {{site.data.keyword.cloud_notm}} [projects](#x2035151){: term}. You can use {{site.data.keyword.secrets-manager_short}} as a building block for the solutions that you build and deploy by using projects, which are used to manage related resources and deployments across accounts, embracing an Infrastructure as Code (IaC) approach to deployments.
{: shortdesc}

If you are not managing your resource deployments with projects, you can deploy {{site.data.keyword.secrets-manager_short}} directly from the catalog. For more information, see [Creating a {{site.data.keyword.secrets-manager_short}} instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
{: note}

## What is Cloud automation for {{site.data.keyword.secrets-manager_short}}?
{: #cloud-automation-service-name}

Cloud automation for {{site.data.keyword.secrets-manager_short}} is a complete {{site.data.keyword.secrets-manager_short}} solution. It is a deployable architecture coded in Terraform, and you can configure it in a project by specifying input variables to achieve the behavior that you want. A deployable architecture is a preconfigured IaC asset that is deployed and configured based on the recommended best practices.

By using the {{site.data.keyword.secrets-manager_short}} deployable architecture, your organization can securely store all your secrets in one location, improving rotation processes and reducing outages caused by expired secrets. This architecture enables you to simplify your organization's secrets management processes and automate their maintenance. With this architecture, you can:

Inventory secrets
:   By storing all secrets in a central location, you can quickly identify any secrets in use or in need of rotation.

Automate rotation
:   Using the built-in secrets generation ability, you can automatically cycle secrets and never have to deal with an expired secret.

Easily include in your existing systems
:   Incorporate {{site.data.keyword.secrets-manager_short}} into your existing key management and Event Notifications systems.

Cloud automation for {{site.data.keyword.secrets-manager_short}} can provision the following resources in your account:

1. A resource group to contain all newly provisioned resources. You can also use a pre-existing resource group.
2. A {{site.data.keyword.secrets-manager_short}} service instance. You can also re-use a pre-existing service instance.
3. (optional) A key management service root key for encrypting secrets created by the {{site.data.keyword.secrets-manager_short}} instance.
   - An authorization policy to connect the {{site.data.keyword.secrets-manager_short}} instance with a key management service instance in another {{site.data.keyword.cloud_notm}} account.
4. (optional) Secrets engines for creating the following types of secrets:
   - IAM Credentials and authorization policies allowing s2s authorizations.
   - Public certificates.
   - Private certificates.
5. (optional) An Event Notifications topic and email subscription for the newly created {{site.data.keyword.secrets-manager_short}} instance.

### Variations
{: #variations-da}

Cloud automation for {{site.data.keyword.secrets-manager_short}} includes two variations to choose between:

Fully configurable
:   This variation provides complete control over deploying {{site.data.keyword.secrets-manager_short}}. With it, you can customize all variables to suit specific needs. Ideal for teams that require fine-grained control over configuration, integrations, and infrastructure settings.

Security-enforced
:   This variation offers a secure-by-default deployment of {{site.data.keyword.secrets-manager_short}}. It includes secure defaults such as encryption by using key management services, along with secure networking and failure handling. Use this variation to help ensure that your {{site.data.keyword.secrets-manager_short}} instance meets enterprise security and compliance standards.

## Deploying Cloud automation for {{site.data.keyword.secrets-manager_short}} with projects
{: #deploy-service-name}

1. From the {{site.data.keyword.cloud_notm}} catalog, search for Cloud automation for {{site.data.keyword.secrets-manager_short}}.
1. Add it to an existing project or create a project.
1. Depending on your use case, you can refer to the following guidance for configuring your architecture or building more comprehensive solutions:
   - [Configure](/docs/secure-enterprise?topic=secure-enterprise-config-project) and [deploy](/docs/secure-enterprise?topic=secure-enterprise-deploy-project) from your project.
   - You can [stack deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack) together in a project to create a robust end-to-end solution architecture. You don't need to code Terraform to connect the deployable architectures together. As you configure input values in a deployable architecture, you can reference inputs or outputs from another deployable architecture to link them together. After you deploy the deployable architectures, you can add them to a private catalog to easily share your end-to-end solution with others in your organization.

## Resources for working with Cloud automation for {{site.data.keyword.secrets-manager_short}}
{: #deployment-resources}

Using {{site.data.keyword.secrets-manager_short}} to manage your secrets is the same regardless of if you deployed it by using the catalog or a project. However, there are some differences regarding responsibilities and managing your deployment through projects. Use the following resources to learn more.

* After you deploy Cloud automation for {{site.data.keyword.secrets-manager_short}} from a project, you can use the [{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager) documentation for information about creating and managing your secrets.
* If you encounter any issues working with projects to validate or deploy your resources, refer to the [Troubleshooting for projects](/docs/secure-enterprise?topic=secure-enterprise-troubleshoot-project-access) documentation.
* Review the [Understanding your responsibilities when you use {{site.data.keyword.IBM_notm}} deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-deployable-architectures) to learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.IBM_notm}} deployable architectures.
