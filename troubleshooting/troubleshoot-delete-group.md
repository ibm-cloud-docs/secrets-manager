---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-26"

keywords: troubleshoot secrets manager, troubleshooting for secret group, delete secret group, can't delete secret group, unable to delete secret group, troubleshooting Secrets Manager

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}


# Why can't I delete my secret group?
{: #troubleshoot-secret-group}
{: troubleshoot}

You try to use {{site.data.keyword.secrets-manager_full}} to delete a secret group, but you're unable to complete the action.
{:shortdesc}


You have a secret group that you no longer need. When you try to delete it in the {{site.data.keyword.secrets-manager_short}} UI, you get the following error:
{: tsSymptoms}

```
Delete group failed
An error occurred and the secret group couldn't be deleted.
```
{: screen}
   
Secret groups are a way to organize and assign access policies to your secrets. By deleting a secret group, you render all of the secrets in that group useless. For that reason, you cannot delete a secret group that contains secrets.
{: tsCauses}

To delete a secret group, delete all of the secrets that are associated with it and then delete the group itself.
{: tsResolve}

To learn more about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).
