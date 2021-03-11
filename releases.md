---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-08"

keywords: release notes for Secrets Manager, what's new, enhancements, fixes, improvements, Secrets Manager

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

# Release notes
{: #release-notes}

Use these release notes to learn about the latest changes to {{site.data.keyword.secrets-manager_full}} that are grouped by date.
{:shortdesc}



## 15 February 2021
{: #2021-02-15}

You can now use the {{site.data.keyword.secrets-manager_full_notm}} Java SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Java SDK repository in GitHub](https://github.com/IBM/secrets-manager-java-sdk){: external}.

## 27 January 2021
{: #2021-01-27}

You can now use the {{site.data.keyword.secrets-manager_full_notm}} Python SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Python SDK repository in GitHub](https://github.com/IBM/secrets-manager-python-sdk){: external}.

## 26 January 2021
{: #2021-01-26}

You can now use the {{site.data.keyword.secrets-manager_full_notm}} Go SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Go SDK repository in GitHub](https://github.com/IBM/secrets-manager-go-sdk){: external}.


## 18 December 2020
{: #2020-12-18}

{{site.data.keyword.secrets-manager_short}} is now generally available in the {{site.data.keyword.cloud_notm}} catalog!

To find out more about this release, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-secrets-manager-is-now-generally-available){: external}.

## 15 December 2020
{: #2020-12-15}

You can now create a {{site.data.keyword.secrets-manager_short}} service instance in the Sydney (`au-syd`) region.

For more information, see [Regions and endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints).

## 14 December 2020
{: #2020-12-14}

The {{site.data.keyword.secrets-manager_short}} CLI plug-in is now available for download!

You can use the {{site.data.keyword.secrets-manager_short}} CLI to interact with the secrets that you store in your {{site.data.keyword.secrets-manager_short}} instance. To install the plug-in, log in to the IBM Cloud CLI and run `ibmcloud plugin install secrets-manager`.

- To see CLI usage examples for creating secrets of different types, check out [Creating secrets](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets).
- To find out more about the CLI commands and options that are available for {{site.data.keyword.secrets-manager_short}}, see the [CLI reference](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli).

## 24 November 2020
{: #2020-11-24}

You can now use the {{site.data.keyword.secrets-manager_full_notm}} Node.js SDK to connect to your {{site.data.keyword.secrets-manager_short}} service instance.

For more information, check out the [{{site.data.keyword.secrets-manager_full_notm}} Node.js SDK repository in GitHub](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}.

## 13 November 2020
{: #2020-11-13}

Need to manage {{site.data.keyword.cloud_notm}} secrets by using an on-premises Vault? You can now integrate stand-alone {{site.data.keyword.cloud_notm}} plug-ins for Vault. These open source plug-ins can be used independently from each other so that you can manage {{site.data.keyword.cloud_notm}} secrets through your on-premises Vault server.

- To set up authentication between Vault and your {{site.data.keyword.cloud_notm}} account, you can use the [{{site.data.keyword.cloud_notm}} Auth Method plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-auth-ibmcloud){:external}.
- To dynamically create API keys for {{site.data.keyword.cloud_notm}} service IDs, you can use the [{{site.data.keyword.cloud_notm}} Secrets Backend plug-in for Vault](https://github.com/ibm-cloud-security/vault-plugin-secrets-ibmcloud){: external}.

For more information, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/hashicorp-enterprise-vault-plugins-for-ibm-cloud){: external}

## 9 November 2020
{: #2020-11-09}

You can now choose between the Reader and SecretsReader IAM roles for better control over access to the payload of your secrets.

- As a reader, you can browse a high-level view of secrets in your instance. Readers can't access the payload of a secret.
- As a secrets reader, you can browse a high-level view of secrets, and you can access the payload of a secret. A secrets reader can't create secrets or modify the value of an existing secret.

To learn more about service access roles, see [Managing IAM access for {{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-iam).

## 24 September 2020
{: #2020-09-24}

{{site.data.keyword.secrets-manager_short}} is now available as a beta service in the {{site.data.keyword.cloud_notm}} catalog!

In this release, {{site.data.keyword.secrets-manager_short}} offers support for the following types of secrets:

- IAM credentials, which consist of a service ID and API key that are generated dynamically on your behalf.
- Arbitrary secrets, such as custom credentials that can be used to store any type of structured or  unstructured data.
- User credentials, such as usernames and passwords that you can use to log in to applications.

To find out more about capabilities and use cases for {{site.data.keyword.secrets-manager_short}}, check out the [announcement blog](https://www.ibm.com/cloud/blog/announcements/introducing-ibm-cloud-secrets-manager-beta){: external}.

