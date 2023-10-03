---

copyright:
  years: 2020, 2023
lastupdated: "2023-10-03"

keywords: update

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

# Updating secret metadata
{: #updating-secret-metadata}

With {{site.data.keyword.secrets-manager_full}}, you can update an existing secret metadata, such as its name or description, custom metadata and expiration date.  
Note that not all secret types support all options.
{: shortdesc}


## Before you begin
{: #before-manual-update}

Before you get started, be sure that you have the required level of access. To update secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Updating a secret metadata in the UI
{: #update-secret-ui}
{: ui}

1. In the row for the secret that you want to update, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) > **Details**.
2. To update a secret's General, Expiration, or Metadata details, click the respective tab.


## Updating a secret metadata using CLI
{: #update-secret-cli}
{: cli}

To update a secret's metadata by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-metadata-update`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-metadata-update-command) command. For example, the following command updates a secret's name, description, labels, custom metadata, and expiration date.

```sh
ibmcloud secrets-manager secret-metadata-update \
    --id 0b5571f7-21e6-42b7-91c5-3f5ac9793a46 \
    --name updated-arbitrary-secret-name-example \
    --description 'updated Arbitrary Secret description' \
    --labels dev,us-south \
    --custom-metadata '{"anyKey": "anyValue"}' \
    --expiration-date 2033-04-12T23:20:50.520Z
```
{: codeblock}
{: curl}


## Updating a secret metadata using API
{: #update-secret-api}
{: api}

To update a secret's metadata by using the API, execute the following request. In this example the request updates a secret's name, description, labels, custom metadata, and expiration date.

```sh
curl -X PATCH --location --header "Authorization: Bearer {iam_token}" \
   --header "Accept: application/json" \
   --header "Content-Type: application/merge-patch+json" \
   --data '{
      "custom_metadata":{
         "metadata_custom_key": "metadata_custom_value"
      },
      "description": "updated Arbitrary Secret description",
      "labels": ["dev","us-south"],
      "name": "updated-arbitrary-secret-name-example",
      "expiration_date": "2023-10-05T11:49:42Z"
   }' \  
   "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/{id}/metadata"
```
{: codeblock}
{: curl}

When you call the API, replace the `region`, secret `id` variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: note}