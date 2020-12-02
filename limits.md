---

copyright:
  years: 2020
lastupdated: "2020-12-02"

keywords: limits, known issues, secrets manager limits, Secrets Manager resource limitations

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

# {{site.data.keyword.secrets-manager_short}} limits
{: #limits}

Consider the following service limits as you use {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

## General limits
{: #general-limits}

The following limits apply to {{site.data.keyword.secrets-manager_short}} service instances.

| Resource | Limit|
| --- | --- |
| {{site.data.keyword.secrets-manager_short}} service instances | 1 per {{site.data.keyword.cloud_notm}} account |
| Total secret groups | 200 per {{site.data.keyword.secrets-manager_short}} service instance |
| Total secrets | - |
{: caption="Table 1. Limits that apply to the {{site.data.keyword.secrets-manager_short}} service" caption-side="top"}

## Secret limits
{: #secret-limits}

Review the following table to understand the limits that apply to secrets of different types.

### Limits for user credentials
{: #user-credential-limits}

The following limits apply to user credentials.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 50 characters</br></br>The name of the secret can only contain alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 240 characters |
| Username | 2 - 64 characters |
| Password | 64 characters |
| Labels | 2 - 30 characters</br></br>30 labels per secret |
| Versions | 50 versions per secret |
{: caption="Table 2. Limits that apply to user credentials" caption-side="top"}

### Limits for IAM credentials
{: #iam-credential-limits}

The following limits apply to IAM credentials.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 50 characters</br></br>The name of the secret can only contain alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 240 characters |
| Access groups | 1 - 10 groups |
| Labels | 2 - 30 characters</br></br>30 labels per secret |
| Maximum lease duration | 90 days |
{: caption="Table 3. Limits that apply to IAM credentials" caption-side="top"}

### Limits for arbitrary secrets
{: #arbitrary-secret-limits}

The following limits apply to arbitrary secrets.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 50 characters</br></br>The name of the secret can only contain alphanumeric characters, dashes, and dots. It must start and end with an alphanumeric character. |
| Description | 2 - 240 characters |
| Secret value | 1 MB |
| Labels | 2 - 30 characters</br></br>30 labels per secret |
| Versions | 50 versions per secret |
{: caption="Table 4. Limits that apply to arbitrary secrets" caption-side="top"}

## Secret group limits
{: #secret-group-limits}

Review the following table to understand the limits that apply to secret groups.

| Attribute | Limit |
| --- | --- |
| Name | 2 - 64 characters |
| Description | 1024 characters |
| Labels | 2 - 30 characters</br></br>30 labels per secret group |
| Total secrets | - |
{: caption="Table 4. Limits that apply to secret groups" caption-side="top"}
