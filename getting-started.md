---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-02"

keywords: secrets management, manage secrets, manage credentials, getting started tutorial, store username and password, add secrets, add credentials, get started with Secrets Manager

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
{:generic: .ph data-hd-programlang='generic'}
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

# Getting started with {{site.data.keyword.secrets-manager_short}}
{: #getting-started}
{: toc-content-type="tutorial"} 
{: toc-services="secrets-manager"}
{: toc-completion-time="10m"}

With {{site.data.keyword.secrets-manager_full}}, you can create, lease, and centrally manage secrets that are used in {{site.data.keyword.cloud_notm}} services or your custom-built applications. Secrets are stored in a dedicated {{site.data.keyword.secrets-manager_short}} instance, built on open source HashiCorp Vault.
{:shortdesc}

## Objective
{: #gs-objective}

This tutorial focuses on storing and managing a username and password in {{site.data.keyword.secrets-manager_short}} by using the {{site.data.keyword.secrets-manager_short}} UI.

Looking for a different secret type? You can also create Identity and Access Management (IAM) credentials to access an {{site.data.keyword.cloud_notm}} resource, or you can add custom secrets that can hold structured or unstructured data. 

[![This image is a visual link to the instructions for creating IAM credentials](images/gs-iam-credentials.svg)](/docs/secrets-manager?topic=secrets-manager-store-secrets#store-iam-credentials)     [![This image is a visual link to the instructions on adding custom secrets.](images/gs-custom.svg)](/docs/secrets-manager?topic=secrets-manager-store-secrets#store-arbitrary-secrets)

## Before you begin
{: #gs-prereqs}

Before you begin, be sure to [create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance) in your {{site.data.keyword.cloud_notm}} account. To complete this tutorial, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Choose a type of secret
{: #gs-secret-type}
{: step}

A secret is a piece of sensitive information, such as an API key, a username and password, or any type of credential that you use to access a protected or confidential system. 

You can get started with {{site.data.keyword.secrets-manager_short}} by choosing the type of secret that is required by the resource that you want to access. For this tutorial, complete the following steps to select a secret that contains a username and password.

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu** ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets** table, click **Add**.
4. Select the **User credentials** tile.

  You're all set to enter the details of your new secret. To describe and store your secret, continue to the next step.

## Store the secret securely
{: #gs-add-secret}
{: step}

When you're working with secrets, it's important to organize them in a single location so that you help to reduce the risk of compromised credentials. By storing a secret in {{site.data.keyword.secrets-manager_short}}, you can manage a secret centrally, use [secret groups](#x9968962){:term} to control access, and avoid coding it directly into your apps or version control systems.

Complete the following steps to enter the details of a secret and store it securely in your instance.

1. In the **Add user credentials** page, add a name and description to easily identify your secret.
2. Add the secret to a group to control who on your team has access to it.

   You can click **Create** to provide a name and a description for a new group. Later, you can assign an access policy to the group so that you control who on your team has access to its contained secret. 
3. Optional: Add labels to help you to search for similar secrets in your instance.
4. Supply the username and password values that you want to associate with the secret.
5. Optional: Set an expiration date for the secret.
6. Click **Add**.

  You did it! The username and password are now stored in your dedicated, single-tenant instance of {{site.data.keyword.secrets-manager_short}}. 

## Manage its lifecycle
{: #gs-manage-lifecycle}
{: step}

After you add a secret to your instance, you can establish a regular cadence for managing its lifecycle. For example, you might need to adhere to an internal requirement or regulatory control in your business for rotating secrets every 30 days. 

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
   1. To view and edit details about the secret, click **Edit details**.
   2. To rotate the secret, click **Rotate**.
   3. If you no longer need the secret, click **Delete**.

## Next steps
{: #gs-next-steps}

Now you can add more secrets and design a secrets management strategy to control who has access to them.

- To find out more about organizing secrets, check out [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).

