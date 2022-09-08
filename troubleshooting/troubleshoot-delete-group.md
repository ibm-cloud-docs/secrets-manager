---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-08"

keywords: can't delete secret group, unable to delete secret group

subcollection: secrets-manager

content-type: troubleshoot

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


# Why can't I delete my secret group?
{: #troubleshoot-secret-group}
{: troubleshoot}

You try to use {{site.data.keyword.secrets-manager_full}} to delete a secret group, but you're unable to complete the action.
{: shortdesc}


You have a secret group that you no longer need. When you try to delete it in the {{site.data.keyword.secrets-manager_short}} UI, you get the following error:
{: tsSymptoms}

```plaintext
Delete group failed
An error occurred and the secret group couldn't be deleted.
```
{: screen}

Secret groups are a way to organize and assign access policies to your secrets. By deleting a secret group, you render all the secrets in that group useless. For that reason, you cannot delete a secret group that contains secrets.
{: tsCauses}

To delete a secret group, delete all the secrets that are associated with it and then delete the group itself.
{: tsResolve}

To learn more about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).


