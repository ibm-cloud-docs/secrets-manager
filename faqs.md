---

copyright:
  years: 2020
lastupdated: "2020-09-29"

keywords: faqs, Frequently Asked Questions, question, Secrets Manager, dynamic what is a secret, what is an arbitrary secret, what is an IAM credential, arbitrary secret, IAM credential, what happens when secret expires 

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
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}

# FAQs for {{site.data.keyword.secrets-manager_short}}
{: #faqs}


FAQs for {{site.data.keyword.secrets-manager_full}} might include questions about secrets or credentials. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}


## What is a secret?
{: #faq-secret}
{: faq}
{: support}

A secret is a piece of sensitive information. For example, a secret might be a username and password combination or an API key that you use while you develop your applications. To keep your applications secure, it is important to regulate which secrets can access what and who has access to them. 

In addition to the static secrets described, there are other types of secrets that you might work with in the {{site.data.keyword.secrets-manager_short}} service. To learn more about secret types, check out [Types of secrets](/docs/secrets-manager?topic=secrets-manager-secret-basics#secret-types).



## What is a secret engine?
{: #faq-secret-engine}
{: faq}

A secret engine is a component that enables a connection between a {{site.data.keyword.secrets-manager_short}} instance and another service, such as IAM. The connection grants authorization between the services and allows the {{site.data.keyword.secrets-manager_short}} instance to create time-based secrets for the target service on demand.

For more information about secret engines, see [Configuring secret engines](/docs/secrets-manager?topic=secrets-manager-secret-engines).

## What is a secret group?
{: #faq-secret-group}
{: faq}

A secret group is a means to organize and control access to the secrets that you store within {{site.data.keyword.secrets-manager_short}}. There are several different strategies that you might use to approach secret groups. For more information and recommendations, see [Best practices for organizing secrets and assigning access](/docs/secrets-manager?topic=secrets-manager-best-practices-organize-secrets).


## What is an IAM credential?
{: #faq-credential}
{: faq}

An IAM credential is a type of dynamic secret that you can use to access an {{site.data.keyword.cloud_notm}} resource that requires IAM authentication. When you create an IAM credential through {{site.data.keyword.secrets-manager_short}}, the service creates a service ID and an API key on your behalf. For more information about creating and storing IAM credentials, see [Creating IAM credentials](/docs/secrets-manager?topic=secrets-manager-store-secrets#store-user-credentials).


## What happens when I rotate my secret?
{: #faq-secret-rotate}
{: faq}
{: support}

When a secret is rotated, a new version of its value becomes available for use. You can choose to manually add a value or automatically generate one at regular intervals by enabling automatic rotation. The previous values are stored by the service and can be retrieved by using the {{site.data.keyword.secrets-manager_short}} APIs if you ever need to audit the version history of your secret. 

For more information about secret rotation, see [Rotating secrets](/docs/secrets-manager?topic=secrets-manager-rotate-secrets).

## What happens when my secret expires?
{: #faq-secret-expire}
{: faq}
{: support}

If you are storing an arbitrary secret or user credentials, you can choose to set the date and time at which your secret expires. When the secret reaches its expiration date, it transitions to a *Destroyed* state. When that happens, the value that is associated with the secret is no longer recoverable.

For more information about how your information is protected, see [Securing your data](/docs/secrets-manager?topic=secrets-manager-mng-data).

