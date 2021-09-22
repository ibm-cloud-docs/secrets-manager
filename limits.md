---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-22"

keywords: known issues for Secrets Manager, known limitations for Secrets Manager

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

# Known issues and limits
{: #known-issues-and-limits}

{{site.data.keyword.secrets-manager_full}} includes the following known issues and limits that might impact your experience.
{: shortdesc}

## Known issues
{: #issues-and-limitations}

Review the following known issues that you might encounter as you use {{site.data.keyword.secrets-manager_short}}.

| Issue | Workaround |
| --- | --- |
| Multiple secrets of the same type can't be created with the same name. | It is not possible to create more than one secret of the same type with the same name. This limitation applies at the instance level. To organize similar secrets of the same type across multiple secret groups in your instance, try adding a prefix or suffix to the names of those secrets. |
| Secrets can't be transferred between secret groups. | If you accidentally assign a secret to the wrong secret group, or if you don't want a secret to belong to the default secret group, you must delete the secret and create a new one. |
| API keys that are associated with an IAM secret aren't valid immediately after they are generated. | If you have automation in place that calls the {{site.data.keyword.secrets-manager_short}} API to get the API key for an IAM secret, add a wait delay of 2 seconds to allow the new API key to be recognized by IAM. |
| Secrets with a time-to-live (TTL) don't immediately expire. | After a secret with a TTL reaches the end of its lease duration, expect a tolerance of 1 - 2 minutes before the secret's associated service ID is deleted by IAM. |
| Community plug-ins for Vault are not supported. | It is not possible to integrate a community plug-in for Vault with {{site.data.keyword.secrets-manager_short}}, unless it is written against a secrets engine that {{site.data.keyword.secrets-manager_short}} supports. To manage {{site.data.keyword.cloud_notm}} secrets by using the full Vault native experience, use the [stand-alone {{site.data.keyword.cloud_notm}} plug-ins for Vault](/docs/secrets-manager?topic=secrets-manager-faqs#faq-vault-community-plugins). |
{: caption="Table 1. Known issues and limitations that apply to the {{site.data.keyword.secrets-manager_short}} service" caption-side="top"}

## Limits
{: #limits}

Consider the following service limits as you use {{site.data.keyword.secrets-manager_short}}.

### General limits
{: #general-limits}

The following limits apply to {{site.data.keyword.secrets-manager_short}} service instances.

| Resource | Limit|
| --- | --- |
| {{site.data.keyword.secrets-manager_short}} service instances | 1 per {{site.data.keyword.cloud_notm}} account |
| Total secret groups | 200 per {{site.data.keyword.secrets-manager_short}} service instance |
| Total secrets | - |
{: caption="Table 2. {{site.data.keyword.secrets-manager_short}} service limits" caption-side="top"}

## API rate limits
{: #api-rate-limits}

The following rate limits apply to {{site.data.keyword.secrets-manager_short}} APIs.

| APIs | Limit |
| --- | --- |
| [All service APIs](/apidocs/secrets-manager#rate-limits) | 20 requests per second on a per-service-instance basis |
{: caption="Table 3. Rate limits that apply to {{site.data.keyword.secrets-manager_short}} service APIs" caption-side="top"}

### Resource limits
{: #secret-limits}

Review the following table to understand the limits that apply to secrets of different types.

#### Limits for secret groups
{: #secret-group-limits}

The following limits apply to secret groups.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 64 characters |
| Description | 2 - 1024 characters |
| Labels | 2 - 30 characters  /n  /n 30 labels per secret group |
| Total secrets | - |
{: caption="Table 4. Secret group limits" caption-side="top"}

#### Limits for arbitrary secrets
{: #arbitrary-secret-limits}

The following limits apply to arbitrary secrets.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 256 characters  /n  /n The name of the secret can contain only alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 1024 characters |
| Secret value | 1 MB |
| Labels | 2 - 30 characters  /n  /n 30 labels per secret |
{: caption="Table 5. Arbitrary secret limits" caption-side="top"}

#### Limits for IAM credentials
{: #iam-credential-limits}

The following limits apply to IAM credentials.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 256 characters  /n  /n The name of the secret can contain only alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 1024 characters |
| Access groups | 1 - 10 groups |
| Labels | 2 - 30 characters  /n  /n 30 labels per secret |
| Maximum lease duration | 90 days |
{: caption="Table 6. IAM credential limits" caption-side="top"}



#### Limits for TLS certificates
{: #certificates-limits}

The following limits apply to TLS certificates.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 256 characters  /n  /n The name of the secret can contain only alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 1024 characters|
| Certificate | 100 KB  /n  /n Supported file type is `.pem`. The certificate must be a valid, X.509-based certificate. |
| Private key | 100 KB  /n  /n Private key file is limited to PEM-formatted content. If provided, the private key must match the certificate that you are importing. Only unencrypted private keys are supported. |
| Intermediate certificate | 100 KB  /n  /n Supported file type is `.pem`. If provided, the intermediate certificate must be a valid, X.509-based certificate. |
| Labels | 2 - 30 characters  /n  /n 30 labels per secret |
| Versions | 2 versions per certificate (current and previous) |
{: caption="Table 7. TLS certificate limits" caption-side="top"}



#### Limits for user credentials
{: #user-credential-limits}

The following limits apply to user credentials.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 256 characters  /n  /n The name of the secret can contain only alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 1024 characters |
| Username | 2 - 64 characters |
| Password | 64 characters |
| Labels | 2 - 30 characters  /n  /n 30 labels per secret |
{: caption="Table 8. User credential limits" caption-side="top"}





