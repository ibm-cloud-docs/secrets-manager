---

copyright:
  years: 2025
lastupdated: "2025-09-06"

keywords: migration, integrations, vault jenkins plug-in, ansible, tool integrations

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

# Integrating with Vault Jenkins plug-in
{: #integration-jenkins}

Are you already using the Vault Jenkins plug-in? You can integrate this tool with {{site.data.keyword.secrets-manager_full}} to minimize the changes in your setup. 
{: shortdesc}

## Before you begin 
{: #before-jenkins-integration}

1. Be sure that you have the required level of access. You need the [SecretsReader](/docs/secrets-manager?topic=secrets-manager-iam#iam-roles-actions) service role to view secrets.
2. Create an IAM API key by using a service ID that has `SecretsReader` permissions.

## Setting up the Vault Jenkins plug-in integration
{: #integration-jenkins-setup}

By following these steps, you can integrate {{site.data.keyword.secrets-manager_short}} with the plug-in that you use in your Jenkins job or project. For more information about the Jenkins plug-in, see [Hashicorp Vault](https://plugins.jenkins.io/hashicorp-vault-plugin/){: external}.

1. In the Jenkins dashboard, select your job or project where you're using the plug-in.
2. Click **Build Environment** in the configuration section.
3. Select **Vault Plugin**.
4. Select **Vault Token Credentials** from the **Credential Authentication Kind** field.
5. Create a placeholder value for the **Token** field (for example, `temp`). 
6. Set an ID (for example, `sm-vault-token`).
7. Click **Add**.
8. In the **Vault URL** field, point to the URL of your {{site.data.keyword.secrets-manager_short}} instance.
9. Click **Advanced** to display the **Prefix Path**. 
10. Set the **Prefix Path** to `ibmcloud/kv`. 
11. Make sure that the **K/V Engine Version** is `2`.

## Updating the plug-in's Vault token
{: #integration-jenkins-token}

You can configure a `vault-refresh` job to run periodically to refresh the Vault token before it expires. 

1. Create a periodically repeated job to refresh the token. The token uses the Jenkins CLI command `update-credentials-by-xml` to update the value of the token credential that you defined as a placeholder value in step 2 of the [Setting up](/docs/secrets-manager?topic=secrets-manager-integration-jenkins#integration-jenkins-setup) section.
2. Create a Jenkins job with the name `vault-refresh`, for example. 
3. In the **Build Environment** section, add the name of the secret as the **Secret Text** variable. It is recommended for you to generate an API key from a service ID. Assign to the service ID the minimal access that is needed for reading the secrets that you handle by using the Jenkins plug-in.
4. In the **Build Steps** section, add **Executive Shell** and enter the following script.

    ```sh
    wget YOUR-TAAS-JENKINS-INSTANCE-NAME.swg-devops.com/jnlpJars/jenkins-cli.jar
    export IAM_TOKEN=$(curl -X POST 'https://iam.cloud.ibm.com/identity/token' -H 'Content-Type: application/x-www-form-urlencoded' -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=${IAM_API_KEY}" | jq --raw-output '.access_token')
    export NEW_VAULT_TOKEN=$(curl -X PUT -H "X-Vault-Request: true" -d "{\"token\":\"${IAM_TOKEN}\"}" ${VAULT_ADDR}/v1/auth/ibmcloud/login | jq --raw-output '.auth.client_token')

    printf "<com.datapipe.jenkins.vault.credentials.VaultTokenCredential plugin=\"hashicorp-vault-plugin@3.8.0\"><scope>GLOBAL</scope><id>sm-vault-token</id><description>Vault token for Secrets Manager</description><token>$NEW_VAULT_TOKEN</token></com.datapipe.jenkins.vault.credentials.VaultTokenCredential>" > creds.xml
    java -jar jenkins-cli.jar -s https://YOUR-TAAS-JENKINS-INSTANCE-NAME.swg-devops.com/ -auth <Jenkins user>:<user token> update-credentials-by-xml system::system::jenkins '(global)' sm-vault-token < creds.xml
    ```
    {: codeblock}


    `Jenkins user` represents the user or FID that you use to run your Jenkins automation. `user token` is a token that belongs to the user. To obtain a Jenkins API key or token, click the username and select **Configure > API token > New Token***.
    {: note}


    Provide the values that are associated with your environment. The ID of the Vault token in the XML is the same ID that you assigned in step 2. After you run the job, the Vault token is updated with the real value.


For more information about the {{site.data.keyword.secrets-manager_short}} authentication and Vault tokens, see the [Vault API reference](/docs/secrets-manager?topic=secrets-manager-vault-api#vault-login).


## Next steps
{: #integration-jenkins-next}

Now that you integrated your Jenkins job by using the Vault plug-in, you can fetch secrets from {{site.data.keyword.secrets-manager_short}} and add the environment variables as usual, without additional code changes.
