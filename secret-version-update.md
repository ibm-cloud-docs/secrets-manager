---

copyright:
  years: 2020, 2023
lastupdated: "2023-10-02"

keywords: update, metadata, secret version

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

# Updating secret version metadata
{: #updating-secret-version}

With {{site.data.keyword.secrets-manager_full}}, you can also update an existing secret's version custom metadata.
{: shortdesc}

## Updating a secret version metadata in the UI
{: #update-secret-version-ui}
{: ui}

1. In the row for the secret that you want to update, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) > **Version history**.
2. Select the version to update its custom metadata.


## Updating a secret version metadata using CLI
{: #update-secret-version-cli}
{: cli}

To update a secret version by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-version-metadata-update`**](/docs/secrets-manager-cli-plugin?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-version-metadata-update-command) command. For example, the following command updates a secret's name, description, labels, custom metadata, and expiration date.

```sh
ibmcloud secrets-manager secret-version-metadata-update \
    --secret-id 0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --id eb4cf24d-9cae-424b-945e-159788a5f535 \
    --version-custom-metadata '{"anyKey": "anyValue"}'
```
{: codeblock}
{: curl}


## Updating a secret version metadata using API
{: #update-secret-version-api}
{: api}

To update a secret version metadata by using the API, execute the following request. In this example the request updates a secret version's custom metadata.

```sh
curl -X PATCH --location --header "Authorization: Bearer {iam_token}" \
    --header "Accept: application/json" \
    --header "Content-Type: application/merge-patch+json" \
    --data ''
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{secret_id}/versions/{id}/metadata""
```
{: codeblock}
{: curl}

When you call the API, replace the `region`, `secret_id`, secret version `id` variables, and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: note}

