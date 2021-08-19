---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-19"

keywords: security and compliance for {{site.data.keyword.secrets-manager_short}}, security for {{site.data.keyword.secrets-manager_short}}, compliance for {{site.data.keyword.secrets-manager_short}},

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

# Managing security and compliance with {{site.data.keyword.secrets-manager_short}}
{: #manage-security-compliance}



{{site.data.keyword.secrets-manager_full}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization.
{: shortdesc}



With the {{site.data.keyword.compliance_short}}, you can monitor your {{site.data.keyword.cloud_notm}} accounts to understand whether they adhere to controls and goals that pertain to {{site.data.keyword.secrets-manager_short}}.




## Monitoring the security and compliance posture of {{site.data.keyword.secrets-manager_short}} resources
{: #monitor-secrets-manager}

As a security or compliance focal, you can use the {{site.data.keyword.secrets-manager_short}} [goals](#x2117978){: term} to help ensure that your organization is adhering to the external and internal standards for your industry. By using the {{site.data.keyword.compliance_short}} to validate the resource configurations in your account against a [profile](#x2034950){: term}, you can identify potential issues as they arise.

Before you can monitor your accounts for compliance, you need to [install a collector](/docs/security-compliance?topic=security-compliance-collector) that enables a connection between the {{site.data.keyword.compliance_short}} and your IT resources. For more information, check out the [{{site.data.keyword.compliance_short}} docs](/docs/security-compliance?topic=security-compliance-getting-started).
{: note}

### Available goals for {{site.data.keyword.secrets-manager_short}}
{: #secrets-manager-available-goals}

The following goals are available in {{site.data.keyword.compliance_short}} for monitoring {{site.data.keyword.secrets-manager_short}} resources.

* *Check whether {{site.data.keyword.secrets-manager_short}} arbitrary secrets are not expired*
* *Check whether {{site.data.keyword.secrets-manager_short}} user credentials are not expired*
* *Check whether {{site.data.keyword.secrets-manager_short}} IAM credentials are not expired*
* *Check whether {{site.data.keyword.secrets-manager_short}} is provisioned in authorized regions only*
* *Check whether {{site.data.keyword.secrets-manager_short}} default secret group contains no secrets*
* *Check whether {{site.data.keyword.secrets-manager_short}} arbitrary secrets are rotated at least every # days*
* *Check whether {{site.data.keyword.secrets-manager_short}} user credentials are rotated at least every # days*
* *Check whether {{site.data.keyword.secrets-manager_short}} IAM credentials are rotated at least every # days*
* *Check whether {{site.data.keyword.secrets-manager_short}} instance contains no empty secret groups*
* *Check whether {{site.data.keyword.secrets-manager_short}} has no more than # users with the IAM administrator role*
* *Check whether {{site.data.keyword.secrets-manager_short}} has no more than # service IDs with the IAM manager role*
* *Check whether {{site.data.keyword.secrets-manager_short}} has at least # users with the IAM manager role*
* *Check whether {{site.data.keyword.secrets-manager_short}} has at least # service IDs with the IAM manager role*
* *Check whether {{site.data.keyword.secrets-manager_short}} access is managed only by IAM access groups*




