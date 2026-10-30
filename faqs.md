---

copyright:
  years: 2020, 2026
lastupdated: "2026-10-30"

keywords: faqs, Frequently Asked Questions, question, Secrets Manager, what is a secret, what is an arbitrary secret, what is an IAM credential, arbitrary secret, IAM credential, what happens when secret expires

subcollection: secrets-manager

content-type: faq

---

{:faq: data-hd-content-type='faq'}
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

# FAQ for {{site.data.keyword.secrets-manager_short}}
{: #faqs}

Frequently asked questions for {{site.data.keyword.secrets-manager_full}} might include questions about secrets or credentials. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}

## What is a secret?
{: #faq-secret}
{: faq}
{: support}

A secret is a piece of sensitive information. For example, a secret might be a username and password combination or an API key that you use while you develop your applications. To keep your applications secure, it is important to regulate which secrets can access what and who has access to them.

In addition to the static secrets described, there are other types of secrets that you might work with in the {{site.data.keyword.secrets-manager_short}} service. To learn more about secret types, check out [Secret types and components](/docs/secrets-manager?topic=secrets-manager-secret-types).



## What are differences between the Reader and SecretsReader roles?
{: #faq-differences-secretsreader-roles}
{: faq}

Both the Reader and SecretsReader roles help you to assign read-only access to {{site.data.keyword.secrets-manager_short}} resources.

- As a reader, you can browse a high-level view of secrets in your instance. Readers can't access the payload of a secret.
- As a secrets reader, you can browse a high-level view of secrets, and you can access the payload of a secret. A secrets reader can't create secrets or modify the value of an existing secret.

## How is {{site.data.keyword.secrets-manager_short}} different from {{site.data.keyword.keymanagementserviceshort}}?
{: #faq-differences-key-protect}
{: faq}

There are a few key differences between using {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.secrets-manager_short}} to store your sensitive data. {{site.data.keyword.secrets-manager_short}} offers flexibility with the types of secrets that you can create and lease to applications and services, whereas {{site.data.keyword.keymanagementserviceshort}} delivers on encryption keys that are rooted in FIPS 140-2 Level 3 [hardware security modules (HSMs)](#x6704988){: term}.

## Where can I find information about compliance certifications for {{site.data.keyword.secrets-manager_short}}?
{: #faq-compliance-certifications}
{: faq}

To view a complete list of certifications for {{site.data.keyword.secrets-manager_short}}, see section 5.4 of the [{{site.data.keyword.secrets-manager_short}} software product compatibility report](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=FDDE7600BDD511EA92B4FC8223E18670){: external}.

For more information on Trial and Standard FAQs, see [FAQ for the Trial and Standard plans](/docs/secrets-manager?topic=secrets-manager-faqs-trial-standard).

For more information on Vault Dedicated FAQs, see [FAQ for the Vault Dedicated plan](/docs/secrets-manager?topic=secrets-manager-faqs-vault-dedicated).
