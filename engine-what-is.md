---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-06"

keywords: secrets engines, engine configurations, IAM credentials engine, public certificates engine

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

# What is a secrets engine?
{: #secrets-engines}

[Secrets engines](#x9968967){: term} are components in {{site.data.keyword.secrets-manager_full}} that are used to process operations for secrets of different types.
{: shortdesc}

## Learning about engine configurations
{: #secrets-engines-about}

In {{site.data.keyword.secrets-manager_short}}, a secrets engine serves as the back end for a specific type of secret, such as a password or an API key. Depending on its type, a secrets engine can store data, generate secrets on-demand, and more.

Some engines require a configuration before you can enable them for your instance. For example, when you try to [order SSL/TLS certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates) or create [IAM credentials](/docs/secrets-manager?topic=secrets-manager-iam-credentials) for the first time, a message is displayed to show that an engine configuration is required.

![The image shows the secrets selection screen in the Secrets Manager UI.](images/engine-required.svg){: caption="Example - Engine configuration required" caption-side="bottom"}

## What's in a configuration?
{: #secrets-engine-components}

Engine configurations that you add to your instance contain the properties that the service can use to enable operations for a secret type. For example, if you define [certificate authority (CA) and DNS provider configurations](/docs/secrets-manager?topic=secrets-manager-add-certificate-authority) for your instance, you can use {{site.data.keyword.secrets-manager_short}} to order public certificates (`public_cert`). 

Check out the following image to see how a configuration is structured.

![This image shows the components of a secrets engine configuration. The information in the image is detailed in the surrounding content.](/images/example-config.svg){: caption="JSON representation of {{site.data.keyword.secrets-manager_short}} engine configuration" caption-side="bottom"}


1. The `name` and `type` fields hold identifying information about a configuration.

2. The `config` object contains the actual configuration details.

    When you use the {{site.data.keyword.secrets-manager_short}} API to set the configuration of a secret type, the properties that you add to the `config` object differ depending on the type of configuration that you are adding. For example, the following truncated example shows how the configuration model is represented for a certificate authority (CA) configuration.

    ```json
    {
        "name": "my-lets-encrypt-config",
        "type": "letsencrypt",
        "config": {
            "private_key": "-----BEGIN PRIVATE KEY-----\nMIICdgIBADANB...(redacted)"
        }
    }
    ```
    {: screen}

## How do I get started?
{: #secrets-engines-get-started}

To get started with secrets, you can go to the **Secrets engines** page of the {{site.data.keyword.secrets-manager_short}} UI, or check out the [API reference](/apidocs/secrets-manager/secrets-manager-v2) to learn more about managing secret types programmatically.
