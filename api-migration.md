---

copyright:
  years: "2023"
lastupdated: "2023-09-07"

keywords: api, api v2, secrets manager api, migration, new version

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

# Migrating to {{site.data.keyword.secrets-manager_short}} API v2
{: #api-migration-v2}

A new version of the {{site.data.keyword.secrets-manager_full}} API is available. Review the following sections to migrate your services and resources to v2. To learn more, see the [API docs](/apidocs/secrets-manager/secrets-manager-v2).
{: shortdesc}


If you use only the service UI, you don't need to migrate your services and resources. For more information about migrating your services and resources if you use the API, SDKs, or CLI, review the following sections.
{: important}

## General changes in v2
{: #api-migration-v2-changes}

Review the following list for the biggest changes that you can expect in v2. 

* You must use the [create secret version API](/apidocs/secrets-manager/secrets-manager-v2#create-secret-version) to rotate secrets. 
* Define policies as part of the secret instead of as their own entity.
* You can update the resource metadata by changing only the fields that you want to modify. 
* To create and delete locks, you must use `locks_bulk`.
* Use the lock modes `remove_previous` and `remove_previous_and_delete` instead of `exclusive` and `exclusive_delete`.
* With the Reader role, you can list configurations of IAM permissions.
* Review the response structure, as it might not be the same as v1. 
* For those who created custom roles in IAM - some action names were changed.

## Migrating Curl applications
{: #migrate-api-v2-curl}

If you use `curl`, review the following steps and examples to migrate to the {{site.data.keyword.secrets-manager_short}} API v2.

1. Review the documentation.
2. Replace v1 with v2 and remove the secret type.
3. If the secret or configuration resource has a body, you must add the type of secret into the body. 
4. Remove the metadata and resources wrapper from the resource JSON object.
5. If you are using `jq` or other tools, review the difference in the response structure.

The secret data is at the root of the secret and not under the `secret_data` field. 
{: note}


### Comparing v1 and v2 Curl examples
{: #migrate-curl-examples}

Compare the following examples to see the changes between the v1 Curl code snippet and how the code must look like in v2.

v1 example

```sh
curl -X POST \
    --location --header "Authorization: Bearer ${iam_token}" 
    --header "Accept: application/json"   
    --header "Content-Type: application/json" 
    --data '{ 
            "metadata": { 
                "collection_type": "application/vnd.ibm.secrets-manager.secret+json",
                "collection_total": 1 
                }, 
                "resources": [ 
                    {
                         "name": "example-arbitrary-secret", 
                         "description": "Extended description for this secret.", 
                         "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
                         "payload": "secret-data", 
                         "labels": [ 
                            "dev", "us-south" 
                            ], 
                            "expiration_date": "2030-01-01T00:00:00Z" 
                        } 
                    ] 
                }'   
            "{base_url}/api/v1/secrets/{secret_type}"
```
{: codeblock}


v2 example


```sh
curl -X POST \
    --location 
    --header "Authorization: Bearer {iam_token}" 
    --header "Accept: application/json"   
    --header "Content-Type: application/json" 
    --data '{ 
                "description": "Extended description for this secret.",
                "expiration_date": "2030-01-01T00:00:00Z", 
                "labels": [ 
                    "dev", "us-south" 
                    ], 
                "name": "example-arbitrary-secret", 
                "payload": "secret-data", 
                "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
                "secret_type": "arbitrary"
                }' \ 
        "{base_url}/api/v2/secrets"
```
{: codeblock}


## Migrating to CLI v2
{: #migrate-api-v2-cli}

You can migrate to the {{site.data.keyword.secrets-manager_short}} CLI v2. Review the following steps and examples to complete the process.

1. Review the documentation.
2. Download the latest version of the CLI plug-in.
3. Remove the secret type from the command. In some cases, you might need to add it into the resource.
4. Remove the metadata and resources wrapper from the resource JSON object.
5. If you are using `jq` or other tools, review the difference in the response structure.

### Comparing v1 and v2 CLI examples
{: #migrate-cli-examples}

Compare the following examples to see the changes between the v1 CLI code snippet and how the code must look like in v2.

v1 example

```sh
ibmcloud secrets-manager secret-create 
    --secret-type=arbitrary 
    --resources='[
        {
            "name": "example-arbitrary-secret", 
            "description": "Extended description for this secret.", 
            "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
            "labels": [
                "dev","us-south"
                ], 
                "custom_metadata": {
                    "anyKey": "anyValue"
                    }, 
                    "version_custom_metadata": {
                        "anyKey": "anyValue"
                        }, 
                        "expiration_date": "2030-01-01T00:00:00Z", 
                        "payload": "secret-data"
                        }
                    ]'
```
{: pre}


v2 example 


```sh
ibmcloud secrets-manager secret-create 
    --secret-prototype='{
        "custom_metadata": {
            "anyKey": "anyValue"
            }, 
            "description": "Extended description for this secret.",
            "expiration_date": "2030-01-01T00:00:00Z", 
            "labels": [
                "dev","us-south"
                ], 
                "name": "example-arbitrary-secret", 
                "secret_group_id": "bc656587-8fda-4d05-9ad8-b1de1ec7e712", 
                "secret_type": "arbitrary", 
                "payload": "secret-data", 
                "version_custom_metadata": {
                    "anyKey": "anyValue"
                    }
                }'
```
{: pre}


## Migrating Node applications 
{: #migrate-api-v2-node}

If you use Node, review the following steps and examples to migrate to {{site.data.keyword.secrets-manager_short}} API v2.

1. Update your package.json file to use the latest version (2.0.0).
2. Change the required command to `@ibm-cloud/secrets-manager/secrets-manager/v2`.
3. For each operation that you're using, remove the type and the metadata and resources wrapper from the resource JSON object.

Responses contain only the secret JSON object or a secrets field in case of a list. They do not contain the resources object.
{: note}

### Comparing v1 and v2 Node examples
{: #migrate-node-examples}

Compare the following examples to see the changes between the v1 Node code snippet and how the code must look like in v2.

v1 example

```sh
let res = await secretsManager.createSecret({
secretType: 'username_password',
'metadata': {
    'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1,
},
'resources': [
    {
        'name': 'example-username-password-secret',
        'description': 'Extended description for this secret.'
        'username': 'user123'
        'password': '123456789',
        'labels': ['label1', 'label2'],
        'expiration_date': '2030-04-01T09:30:00Z'
    },
],
});
```
{: codeblock}


v2 example


```sh
let res = await secretsManager.createSecret({
  secretPrototype: { 
    description: 'Extended description for this secret.',
    experation_date: '2030-04-01T09:30:00Z",
    labels: ['label1', 'label2'],
    name: 'example-username-password-secret',
    secret_group_id: 'default',
    secret_type: 'username_password',
    'username': 'user123'
    'password': '123456789'
    }
    });
```
{: codeblock}


## Migrating Java applications
{: #migrate-api-v2-java}

If you use Java, review the following steps and examples to migrate to {{site.data.keyword.secrets-manager_short}} API v2.

1. Update your pom.xml file to use the latest version (2.0.0).
2. Update your import statements to use v2.
3. Remove the secret type, and the metadata and resources wrapper from your resource for each operation that you are using. 

The response type is unified across operations. For example, `Response<Secret>` is the response to `<CreateSecret>` and `<GetSecret>`. With the changes to how response fields are handled in v2, now you have dedicated getters for each field. 
{: note}


### Comparing v1 and v2 Java examples
{: #migrate-java-examples}

Compare the following examples to see the changes between the v1 Java code snippet and how the code must look like in v2.

v1 example

```sh
CollectionMetadata collectionMetadata = new CollectionMetadata.Builder()
    .collectionType("application/vnd.ibm.secrets-manager.secret+json")
    .collectionTotal(Long.parseLong("1"))
    .build();
ArbitrarySecretResource arbitrarySecretResource = new ArbitrarySecretResource.Builder()
    .name("example-arbitrary-secret")
    .description("Extended description for this secret.")
    .payload("secret-data")
    .build();
CreateSecretOptions createSecretOptions = new CreateSecretOptions.Builder()
    .secretType ("arbitrary")
    .resources(new java.util.ArrayList<>(Collections.singletonList(arbitrarySecretResource)))
    .metadata(collectionMetadata)
    .build();
Response<CreateSecret> createResp = sm.createSecret(createSecretOptions).execute();
```
{: codeblock}


v2 example


```sh
ArbitrarySecretPrototype arbitrarySecretResource = new ArbitrarySecretResource.Builder()
    .name("example-arbitrary-secret")
    .description("Extended description for this secret.")
    .payload("secret-data")
    .secretType("arbitrary")
    .build();
CreateSecretOptions createSecretOptions = new CreateSecretOptions.Builder()
    .secretPrototype(arbitrarySecretResource)
    .build();
Response<Secret> createResp = sm.createSecret(createSecretOptions).execute();    

```
{: codeblock}


## Migrating Python applications
{: #migrate-api-v2-python}

If you use Python, review the following steps and examples to migrate to {{site.data.keyword.secrets-manager_short}} API v2.

1. Update your environment to use the latest version (2.0.0) by updating the requirements.txt or running `pip install --upgrade "ibm-secrets-manager-sdk"`.
2. Update your import statements to use v2.
3. Remove the secret type, and the metadata and resources wrapper from your resource for each operation that you're using. 

Responses don't contain the resources object. They contain only the secret JSON, or a `secrets` field, in case of a list. 
{: note}


### Comparing v1 and v2 Python examples
{: #migrate-python-examples}

Compare the following examples to see the changes between the v1 Python code snippet and how the code must look like in v2.

v1 example


```sh
response = secretsManager.create_secret(
    'arbitrary'
    {'collection_type: 'application/vnd.ibm.secrets-manager.secret+json', 'collection_total': 1}, 
    [{'name': 'example-arbirary-secret', 'description': 'Extended description for this secret.', 'payload': 'secret-data'}]
)
```
{: codeblock}


v2 example


```sh
secret_prototype_model = {
    'description': 'Extended description for this secret.',
    'expiration_date': '2023-10-05T11:49:42Z',
    'name': 'example-arbitrary-secret',
    'secret_group_id': 'default'
    'secret_type':'arbitrary'
    'payload': 'secret-data'
    }
response = secretsManager.create_secret(
    secret_prototype=secret_prototype_model,
)
```
{: codeblock}


## Migrating Go applications
{: #migrate-api-v2-go}

If you use Go, review the following steps and examples to migrate to {{site.data.keyword.secrets-manager_short}} API v2.

1. Update your go.mod file to use the latest version (2.0.0).
2. Update your import statements to use v2.
3. For each operation that you're using, remove the type and the metadata and resources wrapper from the resource JSON object.

The response type matches the created response. For example, with v2, you use `ArbitrarySecret` instead of `SecretResourceIntf`.
{: note}

### Comparing v1 and v2 Go examples
{: #migrate-go-examples}

Compare the following examples to see the changes between the v1 Go code snippet and how the code must look like in v2.

v1 example

```sh
createRes, resp, err := secretsManager.CreateSecret(&sm.CreateSecretOptions{
    SecretType: core.StringPtr(sm.CreateSecretOptionsSecretTypeArbitraryConst),
    Metadata: &sm.CollectionMetadata{
        CollectionType: core.StringPtr(sm.CollectionMetadataCollectionTypeApplicationVnd)
        CollectionTotal: core.Int64Ptr(1)
    },
    Resources: []sm.SecretResourceIntf{
        &sm.ArbitrarySecretResource{
            Name:   core.StringPtr("example-arbitrary-secret"),
            Description: core.StringPtr("Extended description for this secret."),
            Payload:    core.StringPtr("secret-data"),
        },
    },
})
```
{: codeblock}


v2 example


```sh
secretPrototypeModel :=&sm.ArbitrarySecretPrototype{
    Description:    core.StringPtr("Extended description for this secret."),
    Labels:     []string{"dev", "us-south"},
    Name:   core.StringPtr("example-arbitrary-secret"),
    SecretGroupID: core.StringPtr("default")
    SecretType:     core.StringPtr("arbitrary")
    Payload:    core.StringPtr("secret-data"),
}

createSecretOptions := secretsManager.NewCreateSecretOptions(
    secretPrototypeModel,
)


secret, _, err := secretsManager.CreateSecret(createSecretOptions)
```
{: codeblock}


