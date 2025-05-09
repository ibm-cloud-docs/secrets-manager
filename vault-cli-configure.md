---

copyright:
  years: 2020, 2025
lastupdated: "2025-05-09"

keywords: Vault CLI, configure the Vault CLI, use Secrets Manager with Vault CLI, CLI commands, log in to Vault

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

# Setting up the Vault CLI
{: #configure-vault-cli}

If you're already using the HashiCorp Vault command-line interface (CLI), you can use its CLI format and guidelines to interact with {{site.data.keyword.secrets-manager_full}}.
{: shortdesc}

All operations follow the guidelines that are available for the Vault CLI. To learn more about using the Vault CLI, check out the [Vault documentation](https://developer.hashicorp.com/vault/docs/commands){: external}.

## Prerequisites
{: #configure-vault-cli-prereqs}

- [Download and install the Vault CLI](https://developer.hashicorp.com/vault/docs/get-vault){: external}.
- [Create an {{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-manapikey) or generate an {{site.data.keyword.cloud_notm}} IAM access token.

    By providing your account credentials, Vault can understand who you are and whether you have the correct level of access to run specific Vault commands against your {{site.data.keyword.secrets-manager_short}} instance.
- Optional: [Download and install jq](https://stedolan.github.io/jq/){: external}.

    `jq` helps you slice up JSON data. You use `jq` in this tutorial to grab and use an access token that's returned when you call the IAM Identity Service API.

## Setting up your environment
{: #configure-vault-cli-env}

First, set up your environment to access a {{site.data.keyword.secrets-manager_short}} service instance by using Vault. Start by creating a shell script that sets the credentials that are needed to authenticate to Vault.

1. In a project directory, create a `login-vault.sh` file.

    ```sh
    touch login-vault.sh
    ```
    {: codeblock}

2. Copy and paste the following script into `login-vault.sh` and update the placeholder values.

    ```sh
    #!/bin/sh

    IBM_CLOUD_API_KEY="xxxxx"

    export VAULT_ADDR="https://<instance_ID>.<region>.secrets-manager.appdomain.cloud"

    export IAM_TOKEN=`curl -s -X POST \
    "https://iam.cloud.ibm.com/identity/token" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -H "Accept: application/json" \
    -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=$IBM_CLOUD_API_KEY" | jq -j ".access_token"`
    ```
    {: codeblock}

    Replace the placeholder values according to the following table.

    | Variable | Description |
    | -------- | ----------- |
    | `IBM_CLOUD_API_KEY` | An {{site.data.keyword.cloud_notm}} API key that has at least [**Viewer** platform access](/docs/secrets-manager?topic=secrets-manager-iam) and [**Reader** service access](/docs/secrets-manager?topic=secrets-manager-iam) to your {{site.data.keyword.secrets-manager_short}} instance.|
    | `VAULT_ADDR` | The Vault API endpoint that's unique to your {{site.data.keyword.secrets-manager_short}} instance.  \n  \n You can find your unique endpoint URL in the **Endpoints** page of the {{site.data.keyword.secrets-manager_short}} UI, or by [retrieving it by HTTP request](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls). |
    {: caption="Required variables that are needed to extract a token" caption-side="top"}


3. Mark the file as executable by running the `chmod` command in your command line.

    ```sh
    chmod +x login-vault.sh
    ```
    {: pre}

4. Run the script to set your environment variables.

    ```sh
    source ./login-vault.sh
    ```
    {: pre}

5. Optional. Verify that the environment variables are set correctly by printing them to your command line window.

    ```sh
    echo $VAULT_ADDR && echo $IAM_TOKEN
    ```
    {: codeblock}

    The output might look similar to the following example.

    ```sh
    https://e415e570-f073-423a-abdc-55de9b58f54e.us-south.secrets-manager.appdomain.cloud
    eyJraWQiOiIyMDIwMTAxODE3MDEiLCJhbGciOiJSUzI1NiJ9.eyJpYW1faWQi...(truncated)
    ```
    {: screen}

## Logging in to Vault
{: #configure-vault-cli-login}

After you configure your environment, log in to Vault to start interacting with your {{site.data.keyword.secrets-manager_short}} instance.

1. Authenticate to Vault by using your {{site.data.keyword.cloud_notm}} IAM token.

    ```sh
    vault write auth/ibmcloud/login token=$IAM_TOKEN
    ```
    {: pre}

    The following screen shows the example output.

    ```plaintext
    Key                      Value
    ---                      -----
    token                    s.5DQYF57xU1qOAIj2PhnMC39H
    token_accessor           C14JDJ6KtwQKQR5UNR5NIC7J
    token_duration           1h
    token_renewable          true
    token_policies           ["default" "instance-manager"]
    identity_policies        []
    policies                 ["default" "instance-manager"]
    token_meta_grant_type    urn:ibm:params:oauth:grant-type:apikey
    token_meta_name          test-ibm-cloud-api-key
    token_meta_resource      crn:v1:bluemix:public:secrets-manager:us-south:a/791f5fb10986423e97aa8512f18b7e65:e415e570-f073-423a-abdc-55de9b58f54e::
    token_meta_user          iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8
    token_meta_bss_acc       791f5fb10986423e97aa8512f18b7e65
    ```
    {: screen}

2. Log in to Vault by using the `token` value that was returned in the previous step.

    ```sh
    vault login <token>
    ```
    {: pre}

    The following screen shows the example output.

    ```plaintext
    Success! You are now authenticated. The token information displayed is
    already stored in the token helper. You do NOT need to run "vault login"
    again. Future Vault requests will automatically use this token.

    Key                      Value
    ---                      -----
    token                    s.6yFk0Z1IRi0Yc5DtwIKuENDJ
    token_accessor           BnEHQuAxTiHJGhP1x0hNqagV
    token_duration           57m58s
    token_renewable          true
    token_policies           ["default" "instance-manager"]
    identity_policies        []
    policies                 ["default" "instance-manager"]
    token_meta_grant_type    urn:ibm:params:oauth:grant-type:apikey
    token_meta_name          test-ibm-cloud-api-key
    token_meta_resource      crn:v1:bluemix:public:secrets-manager:us-south:a/791f5fb10986423e97aa8512f18b7e65:e415e570-f073-423a-abdc-55de9b58f54e::
    token_meta_user          iam-ServiceId-222b47ab-b08e-4619-b68f-8014a2c3acb8
    token_meta_bss_acc       791f5fb10986423e97aa8512f18b7e65
    ```
    {: screen}

    Now you can use Vault CLI commands to interact with your {{site.data.keyword.secrets-manager_short}} instance. To find out more, check out the [CLI reference](/docs/secrets-manager?topic=secrets-manager-vault-cli).
