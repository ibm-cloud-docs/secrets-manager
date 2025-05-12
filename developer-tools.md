---

copyright:
  years: 2020, 2025
lastupdated: "2025-05-12"

keywords: Secrets Manager developer tools, integrate with application, API, SDK, CLI

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

# Integrating {{site.data.keyword.secrets-manager_short}} with your apps
{: #integrate-with-apps}

Ready to integrate {{site.data.keyword.secrets-manager_full}} into your existing apps or services? Take advantage of our supported developer tools.
{: shortdesc}

## Supported developer tools
{: #dev-tool-list}

### {{site.data.keyword.secrets-manager_short}} SDKs
{: #dev-tool-sdks}

{{site.data.keyword.secrets-manager_short}} offers software development kits (SDKs) that you can use to connect with the service in various programming languages. For more information about getting started with {{site.data.keyword.secrets-manager_short}} SDKs, check out the following repositories on GitHub:

- [Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}
- [Node.js SDK](https://github.com/IBM/secrets-manager-node-sdk){: external}
- [Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}
- [Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}

### {{site.data.keyword.secrets-manager_short}} CLI plug-in
{: #dev-tool-cli}

If you're already using the [{{site.data.keyword.cloud_notm}} Command Line Interface (CLI)](/docs/cli?topic=cli-getting-started), you can install the {{site.data.keyword.secrets-manager_short}} plug-in so that you can manage secrets in your instance from the command line. For example, after you add secrets to your service instance, you can run the following CLI command to obtain a list of secrets:

```sh
ibmcloud secrets-manager secrets
```
{: pre}

To install the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run `ibmcloud plugin install secrets-manager`. For more information, check out the [{{site.data.keyword.secrets-manager_short}} CLI reference](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli).
{: note}

### {{site.data.keyword.secrets-manager_short}} API
{: #dev-tool-api}

If you're trying out {{site.data.keyword.secrets-manager_short}} for the first time, you might want to use the {{site.data.keyword.secrets-manager_short}} API to evaluate the service and test out workflows for your applications. Start by copying the service endpoint URL from the **Endpoints** page in your {{site.data.keyword.secrets-manager_short}} service dashboard. Then, generate an [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token](/docs/account?topic=account-iamtoken_from_apikey) to include in your request. For example, after you store secrets in your service instance, the following API request can be used to obtain your list of secrets:


```sh
curl -X GET  \
  -H "Authorization: Bearer {access_token}" \
  -H "Accept: application/json" 
  "{base_url}/api/v2/secrets"
```
{: codeblock}


Replace `{base_url}` with your service endpoint URL, and `{access_token}` with your IAM token. To run the API request, you can paste it into your command line or preferred API testing tool. 

For more information about using {{site.data.keyword.secrets-manager_short}} APIs, check out the [{{site.data.keyword.secrets-manager_short}} API reference](/apidocs/secrets-manager/secrets-manager-v2).
{: note}

### Vault
{: #dev-tool-vault}

Already using HashiCorp Vault? You can interact with your {{site.data.keyword.secrets-manager_short}} service instance by using the Vault HTTP API or CLI format.

{{site.data.keyword.secrets-manager_short}} doesn't support all components that are available for Vault. Instead, the service builds on a custom version of open source Vault to support operations in {{site.data.keyword.secrets-manager_short}} for various secret types.
{: note}

For more information, check out the following resources:

- [Vault API reference](/docs/secrets-manager?topic=secrets-manager-vault-api)
- [Vault CLI reference](/docs/secrets-manager?topic=secrets-manager-vault-cli)

### Integrating with the service UI
{: #service-ui-integration}

You can also link to pages of {{site.data.keyword.secrets-manager_short}}, such as the Create secret panel for a particular secret type, or the details panel of a secret.

To programmatically create the URL use the following URL pattern. Then, URL encode it and embed in your code.

Create secret panel:
```
https://CLOUD_ENV/services/secrets-manager/INSTANCE_CRN?paneId=manageSecrets#/manageSecrets/addSecret/SECRET_TYPE
```

[List of programmatic secret type names](/docs/secrets-manager?topic=secrets-manager-what-is-secret#secret-types).

Secret details panel:
```
https://CLOUD_ENV/services/secrets-manager/INSTANCE_CRN?paneId=manageSecrets#/manageSecrets/editSecret/SECRET_ID
```
