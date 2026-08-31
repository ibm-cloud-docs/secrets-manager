---

copyright:
  years: 2026
lastupdated: "2026-08-31"

keywords: Secrets Manager CLI, Vault Dedicated, instance management CLI, command line, terminal

subcollection: secrets-manager

content-type: cli-docs

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


# {{site.data.keyword.secrets-manager_short}} instance management CLI
{: #secrets-manager-management-cli}

You can use the {{site.data.keyword.secrets-manager_full}} instance management command-line interface (CLI) to manage your {{site.data.keyword.secrets-manager_short}} Vault Dedicated instance from the command line.
{: shortdesc}

Current version: **`2.0.7`**

## Prerequisites
{: #secrets-manager-management-cli-prereq}

Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started), and then the {{site.data.keyword.secrets-manager_short}} instance management CLI plug-in by running the following command:

```sh
ibmcloud plugin install secrets-manager-instance-management
```
{: pre}

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}

## Targeting a {{site.data.keyword.secrets-manager_short}} instance
{: #target-sm-management-instance}

To target the {{site.data.keyword.secrets-manager_short}} Vault Dedicated instance, use one of the following options.

* Run the `ibmcloud secrets-manager-instance-management config set` command.

   ```sh
   ibmcloud secrets-manager-instance-management config set service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
   ```
   {: pre}

* Export an environment variable with your {{site.data.keyword.secrets-manager_short}} control plane service endpoint URL.

   ```sh
   export SECRETS_MANAGER_INSTANCE_MANAGEMENT_URL=https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
   ```
   {: pre}

* Set the service endpoint in the command.

   ```sh
   ibmcloud secrets-manager-instance-management instance --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud
   ```
   {: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} Vault Dedicated service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)
