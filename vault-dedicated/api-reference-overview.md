---

copyright:
  years: 2026
lastupdated: "2026-09-01"

keywords: Vault Dedicated API, Vault Dedicated CLI, HashiCorp Vault API, instance management API, Vault Dedicated reference

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

# API and CLI reference overview
{: #vault-dedicated-reference-overview}

The Vault Dedicated plan uses two complementary sets of APIs and CLIs: the {{site.data.keyword.secrets-manager_short}} Instance Management API and CLI for control plane operations, and the native HashiCorp Vault API and CLI for data plane operations such as secrets management.
{: shortdesc}

## {{site.data.keyword.secrets-manager_short}} Instance Management API and CLI
{: #vault-dedicated-ibm-reference}

Use the {{site.data.keyword.secrets-manager_short}} Instance Management API and CLI to manage your Vault Dedicated service instance in {{site.data.keyword.cloud_notm}}. These tools cover control plane operations such as retrieving instance details and generating admin tokens.

| Reference | Description |
|-----------|-------------|
| [Instance Management API reference](https://{DomainName}/apidocs/secrets-manager/secrets-manager-instance-management-v2){: external} | Interactive API reference with SDK examples for managing your Vault Dedicated service instance. |
| [Instance Management API reference (docs)](/docs/secrets-manager?topic=secrets-manager-vault-dedicated-apis) | Conceptual reference with usage examples for instance details and admin token operations. |
| [Instance Management CLI reference](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli) | {{site.data.keyword.cloud_notm}} CLI plug-in for managing your Vault Dedicated instance from the command line. |
| [Instance Management CLI change log](/docs/secrets-manager?topic=secrets-manager-secrets-manager-management-cli-change-log) | History of changes to the Instance Management CLI plug-in. |
{: caption="IBM Cloud secrets-manager_full_notm Instance Management reference" caption-side="bottom"}

## HashiCorp Vault API and CLI
{: #vault-dedicated-hashicorp-reference}

For Vault runtime operations — such as managing secrets, configuring authentication methods, and working with secrets engines — use the native HashiCorp Vault API and CLI directly against your Vault Dedicated cluster endpoint.

| Reference | Description |
|-----------|-------------|
| [HashiCorp Vault API reference](https://developer.hashicorp.com/vault/api-docs){: external} | Full API reference for Vault runtime operations, including secrets engines, auth methods, and policies. |
| [HashiCorp Vault CLI reference](https://developer.hashicorp.com/vault/docs/commands){: external} | Full CLI reference for interacting with your Vault cluster. |
| [HashiCorp Terraform reference](https://developer.hashicorp.com/terraform){: external} | Terraform provider documentation for automating Vault configuration. |
{: caption="HashiCorp Vault reference" caption-side="bottom"}
