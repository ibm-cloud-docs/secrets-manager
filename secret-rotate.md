---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-13"

keywords: rotate secrets, manually rotate, new secret, automatically rotate, automatic rotation, set rotation policy

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

# Rotating secrets
{: #rotate-secrets}

You can rotate your secrets manually by using {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

When you rotate a secret in {{site.data.keyword.secrets-manager_short}}, you create a new version of its value. By rotating your secret at regular intervals, you limit its lifespan and protect against inadvertent exposure of your sensitive data. Rotation is supported in {{site.data.keyword.secrets-manager_short}} for the following secret types:

- Arbitrary secrets (`arbitrary`)
- TLS certificates (`imported_cert`)
- User credentials (`username_password`)

IAM credentials (`iam_credentials)` are recreated dynamically on your behalf so that you don't have to manually rotate them.
{: note}



## Before you begin
{: #before-rotate-secrets}

Before you get started, be sure that you have the required level of access. To rotate secrets, you need the [**Writer** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

## Rotating your secrets manually
{: #manual-rotate-secret}

You can manually rotate user credentials and arbitrary secrets or reimport TLS certificates by using the console or APIs.

### Rotating secrets manually in the UI
{: #manual-rotate-secret-ui}
{: ui}

You can use the {{site.data.keyword.secrets-manager_short}} UI to manually rotate your secrets.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. Use the **Secrets** table to view the secrets in your instance.
4. In the row for the secret that you want to rotate, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Rotate**.

    If you initially provided a value for your secret, you can select a new file or enter a new value, depending on the type of secret that you are rotating.

    For user credentials, you can choose to have the service generate a new password on your behalf. {{site.data.keyword.secrets-manager_short}} replaces the existing value with a randomly generated 32-character password that contains uppercase letters, lowercase letters, digits, and symbols.
    {: note}

5. Click **Rotate**.

    The previous version of your secret is now replaced by its latest value. If you need to audit your version history, you can use the {{site.data.keyword.secrets-manager_short}} API to retrieve the secret. To learn more, check out the [API docs](/apidocs/secrets-manager#get-secret){: external}.

### Rotating secrets manually with the API
{: #manual-rotate-secret-api}
{: api}


You can rotate secrets by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request creates a new version of your secret. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `updateSecret` method to rotate a secret. The following code shows an example call to rotate an arbitrary secret.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `updateSecret(params)` method to rotate a secret. The following code shows an example call to rotate an arbitrary secret.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `update_secret(params)` method to rotate a secret. The following code shows an example call to rotate an arbitrary secret.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `UpdateSecret` method to rotate a secret. The following code shows an example call to rotate an arbitrary secret.
{: go}

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/arbitrary/{id}?action=rotate" \
    -H "Authorization: Bearer $IAM_TOKEN" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{
        "payload": "new-secret-data"
    }'
```
{: codeblock}
{: curl}

```java
SecretActionOneOfRotateArbitrarySecretBody secretActionOneOfModel = new SecretActionOneOfRotateArbitrarySecretBody.Builder()
    .payload("new-secret-data")
    .build();
UpdateSecretOptions updateSecretOptions = new UpdateSecretOptions.Builder()
    .secretType("arbitrary")
    .id(secretIdLink)
    .action("rotate")
    .secretActionOneOf(secretActionOneOfModel)
    .build();

Response<GetSecret> response = sm.updateSecret(updateSecretOptions).execute();
GetSecret getSecret = response.getResult();

System.out.println(getSecret);
```
{: codeblock}
{: java}

```javascript
const params = {
    secretType: 'arbitrary',
    id: secretId,
    action: 'rotate',
    payload: 'new-secret-data',
};

secretsManagerApi.updateSecret(params)
    .then(res => {
        console.log('Rotate secret:\n', JSON.stringify(result.resources, null, 2));
    })
    .catch(err => {
        console.warn(err)
    });
```
{: codeblock}
{: javascript}

```python
secret_data = {
    'payload': 'new-secret-data'
}

response = secretsManager.update_secret(
    secret_type='arbitrary',
    id=secret_id_link,
    action='rotate'
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
secretAction := &sm.SecretActionOneOfRotateArbitrarySecretBody{
    Payload: core.StringPtr("new-secret-data"),
}

updateSecretOptions := secretsManagerApi.NewUpdateSecretOptions(
    "arbitrary", secretIdLink, "rotate", secretAction,
)

result, response, err := secretsManagerApi.UpdateSecret(updateSecretOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

A successful response returns the ID value for the secret, along with other metadata. For more information about the required and optional request parameters, see [Invoke an action on a secret](/apidocs/secrets-manager#update-secret){: external}.

## Rotating your secrets automatically
{: #auto-rotate-secret}

You can enable automatic rotation for specific secret types so that you can replace their values automatically at regular intervals.

Currently, you can enable automatic rotation only for the user credentials (`username_password`) and public certificates (`public_cert`) secret types.
{: note}

### Enabling automatic rotation for user credentials in the UI
{: #auto-rotate-secret-ui}
{: ui}

You can enable automatic rotation for your secret at its creation, or by editing the details of an existing secret. Choose between a 30, 60, or 90-day rotation interval.

If you need more control over the rotation frequency of a secret, you can use the {{site.data.keyword.secrets-manager_short}} API to set a custom interval by using `day` or `month` units of time. For more information, see [Set secret policies](/apidocs/secrets-manager#put-policy){: external}.
{: tip}

1. If you're [adding a secret](/docs/secrets-manager?topic=secrets-manager-user-credentials#user-credentials-ui), enable the rotation option by selecting a 30, 60, or 90-day rotation interval.
2. If you're editing an existing secret, enable automatic rotation by updating its details.
    1. In the **Secrets** table, view a list of your existing secrets.
    2. In the row for the secret that you want to edit, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Edit details**.
    3. Use the **Automatic rotation** option to enable or disable automatic rotation for the secret.

### Enabling automatic rotation for user credentials with the API
{: #auto-rotate-secret-api}
{: api}


You can set rotation policies by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example request sets a monthly rotation policy for a `username_password` secret type. When you call the API, replace the ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance.
{: curl}


If you're using the [{{site.data.keyword.secrets-manager_short}} Java SDK](https://github.com/IBM/secrets-manager-java-sdk){: external}, you can call the `putPolicy` method to set a rotation policy for a secret. The following code shows an example call.
{: java}


If you're using the [{{site.data.keyword.secrets-manager_short}} Node.js SDK](https://github.com/IBM/secrets-manager-nodejs-sdk){: external}, you can call the `putPolicy(params)` method to set a rotation policy for a secret. The following code shows an example call.
{: javascript}


If you're using the [{{site.data.keyword.secrets-manager_short}} Python SDK](https://github.com/IBM/secrets-manager-python-sdk){: external}, you can call the `put_policy(params)` method to set a rotation policy for a secret. The following code shows an example call.
{: python}


If you're using the [{{site.data.keyword.secrets-manager_short}} Go SDK](https://github.com/IBM/secrets-manager-go-sdk){: external}, you can call the `PutPolicy` method to set a rotation policy for a secret. The following code shows an example call.
{: go}

```bash
curl -X POST "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v1/secrets/username_password/{id}/policies?policy=rotation" \
    -H "Authorization: Bearer $IAM_TOKEN"
    -H "Accept: application/json"
    -H "Content-Type: application/json"
    -d '{
    "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.secret.policy+json",
    "collection_total": 1
    },
    "resources": [
        {
        "type": "application/vnd.ibm.secrets-manager.secret.policy+json",
        "rotation": {
        "interval": 1,
        "unit": "month"
        }
    }
    ]
}'
```
{: codeblock}
{: curl}

```java
CollectionMetadata collectionMetadataModel = new CollectionMetadata.Builder()
    .collectionType("application/vnd.ibm.secrets-manager.secret.policy+json")
    .collectionTotal(Long.valueOf("1"))
    .build();
SecretPolicyRotationRotation secretPolicyRotationRotationModel = new SecretPolicyRotationRotation.Builder()
    .interval(Long.valueOf("1"))
    .unit("month")
    .build();
SecretPolicyRotation secretPolicyRotationModel = new SecretPolicyRotation.Builder()
    .type("application/vnd.ibm.secrets-manager.secret.policy+json")
    .rotation(secretPolicyRotationRotationModel)
    .build();
PutPolicyOptions putPolicyOptions = new PutPolicyOptions.Builder()
    .secretType("username_password")
    .id(secretIdLink)
    .metadata(collectionMetadataModel)
    .resources(new java.util.ArrayList<SecretPolicyRotation>(java.util.Arrays.asList(secretPolicyRotationModel)))
    .build();

Response<GetSecretPoliciesOneOf> response = sm.putPolicy(putPolicyOptions).execute();
GetSecretPoliciesOneOf getSecretPoliciesOneOf = response.getResult();

System.out.println(getSecretPoliciesOneOf);
```
{: codeblock}
{: java}

```javascript
const params = {
    secretType: 'username_password',
    id: secretId,
    'metadata': {
        'collection_type': 'application/vnd.ibm.secrets-manager.secret.policy+json',
    'collection_total': 1,
    },
    'resources': [
        {
        'type': 'application/vnd.ibm.secrets-manager.secret.policy+json',
        'rotation': {
        'interval': 1,
        'unit': 'month',
        },
    },
    ],
};

secretsManagerApi.putPolicy(params)
    .then(res => {
        console.log('Set rotation policy:\n', JSON.stringify(result.resources, null, 2));
    })
    .catch(err => {
        console.warn(err)
    });
```
{: codeblock}
{: javascript}

```python
collection_metadata = {
    'collection_type': 'application/vnd.ibm.secrets-manager.secret+json',
    'collection_total': 1
}

rotation_policy_details = {
    'interval': 1,
    'unit': 'month'
}

secret_policy = {
    'type': 'application/vnd.ibm.secrets-manager.secret.policy+json',
    'rotation': rotation_policy_details
}

response = secretsManager.put_policy(
    secret_type='username_password',
    id=secret_id_link,
    metadata=collection_metadata,
    resources=[secret_policy]
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```go
collectionMetadata := &sm.CollectionMetadata{
    CollectionType: core.StringPtr("application/vnd.ibm.secrets-manager.secret.policy+json"),
    CollectionTotal: core.Int64Ptr(int64(1)),
}

rotationPolicyDetails := &sm.SecretPolicyRotationRotation{
    Interval: core.Int64Ptr(int64(1)),
    Unit: core.StringPtr("month"),
}

secretPolicy := &sm.SecretPolicyRotation{
    Type: core.StringPtr("application/vnd.ibm.secrets-manager.secret.policy+json"),
    Rotation: rotationPolicyDetails,
}

setPolicyOptions := secretsManagerApi.NewPutPolicyOptions(
    "username_password", secretIdLink, collectionMetadata, []sm.SecretPolicyRotation{*secretPolicy},
)

result, response, err := secretsManagerApi.PutPolicy(setPolicyOptions)
if err != nil {
    panic(err)
}

b, _ := json.MarshalIndent(result, "", "  ")
fmt.Println(string(b))
```
{: codeblock}
{: go}

A successful response returns the ID value for the policy, along with other metadata. For more information about the required and optional request parameters, see [Set secret policies](/apidocs/secrets-manager#put-policy){: external}.


### Enabling automatic rotation for certificates in the UI
{: #auto-rotate-certificate-ui}
{: ui}

When you order a public certificate (`public_cert`), you can choose to enable automatic rotation. {{site.data.keyword.secrets-manager_short}} sends a request to renew the certificate 31 days before it expires. If the domain validation completes succesfully, your certificate is renewed automatically.

You can enable automatic rotation for your public certificate at its creation, or by editing the details of an existing certificate. 

1. If you're [ordering a certificate](/docs/secrets-manager?topic=secrets-manager-user-credentials#user-credentials-ui), enable the rotation options.
   1. To rotate the certificate automatically 31 days before it expires, switch the rotation toggle to **On**.
   2. To replace the certificate's private key with a new key on each rotation, switch the rekey toggle to **On**.

      Your certificate is automatically rotated 31 days before its expiration date.
      {: note}
2. If you're editing an existing certificate, enable automatic rotation by updating its details.
   1. In the **Secrets** table, view a list of your existing Public certificates.
   2. In the row for the certificate that you want to edit, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Edit details**.
   3. Use the **Automatic rotation** option to add or remove a rotation policy for the secret.

      If you enable automatic rotation on a certificate that expires in less than 31 days, you must also manually rotate it. Only then can rotation take place in the following cycles automatically.
      {: important}
