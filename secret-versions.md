---

copyright:
  years: 2020, 2021
lastupdated: "2021-11-19"

keywords: secret version history, view versions, secret versions

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

# Viewing your version history
{: #version-history}

When you rotate a secret in {{site.data.keyword.secrets-manager_full}}, you create a new version of its value. You can quickly examine the version history of your secrets using the UI.
{: shortdesc}

## Before you begin
{: #before-versions}

Before you get started, be sure that you have the required level of access. To view the version history of a secret, you need the [**Reader** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Viewing version history in the UI
{: #versions-ui}
{: ui}

If you're auditing the version history of a secret, you can use the {{site.data.keyword.secrets-manager_short}} UI to view the general characteristics of each rotation.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the {{site.data.keyword.secrets-manager_short}} UI, go to your **Secrets** list.
4. In the row for the secret that you want to inspect, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Version history**.

    If the secret was rotated previously, the page displays information about the current and previous versions, for example the date that each version was created. Up to 50 versions can be listed for a secret.
    
    If you're inspecting the version history of a public or imported certificate, you can also [download the certificate contents](/docs/secrets-manager?topic=secrets-manager-access-secrets#download-certificate-ui).
    {: tip}


