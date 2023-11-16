---

copyright:
  years: 2020, 2023
lastupdated: "2023-11-16"

keywords: certificate authority, connect certificate authority, set up certificate authority, connect CA, set up CA, connect Let's Encrypt, set up Let's Encrypt, add certificate authority configuration, add CA configuration

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

# Connecting third-party certificate authorities
{: #add-certificate-authority}

With {{site.data.keyword.secrets-manager_full}}, you can connect to a third-party certificate authority by adding a configuration to your instance.
{: shortdesc}

A certificate authority (CA) is the entity that signs and issues your SSL/TLS certificates. By adding a CA configuration, you can specify the authority that you want to use when you [order public certificates](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates) through {{site.data.keyword.secrets-manager_short}}.

You can define up to 10 certificate authority configurations per instance. To view a list of configurations that are available for your instance, go to the **Secrets engines > Public certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.
{: note}
{: ui}

You can define up to 10 certificate authority configurations per instance. To obtain a list of configurations that are available for your instance, you can use the [List configurations](/apidocs/secrets-manager/secrets-manager-v2#get-secret-config-element) API.
{: note}
{: api}

## Before you begin
{: #before-add-certificate-authority}

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).

### Supported certificate authorities
{: #add-certificate-authority-supported}

You can integrate the following certificate authorities with your {{site.data.keyword.secrets-manager_short}} service instance.

| Prerequisites |
| :------------ |
| Before you connect Let's Encrypt, be sure that you:  \n  \n - Obtain the private key that's associated with your Automatic Certificate Management Environment (ACME) account.  \n The ACME protocol makes it possible to automatically obtain browser trusted certificates without human intervention. Before you can request Let's Encrypt certificates through {{site.data.keyword.secrets-manager_short}}, you must have an ACME account. If you already have a working ACME account, you need the private key that was generated when the account was initially created.  \n  \n - Optional: Create an ACME account.  \n If you don't have an existing ACME client or account, you can create one by using the [ACME account creation tool](https://github.com/ibm-cloud-security/acme-account-creation-tool){: external}. |
{: caption="Table 1. Prerequisites - Let's Encrypt" caption-side="top"}
{: #add-certificate-authority-prereqs}
{: tab-title="Let's Encrypt"}
{: tab-group="ca-prereqs"}
{: class="simple-tab-table"}

Certificate authorities might apply a charge when you order or renew a certificate. Additionally, various rate limits apply. {{site.data.keyword.secrets-manager_short}} does not control costs or rate limits that are associated with ordering certificates. For more information about rate limits to keep in mind as you order Let's Encrypt certificates, check out the [Let's Encrypt documentation](https://letsencrypt.org/docs/rate-limits/){: external}.
{: note} 

## Adding a certificate authority configuration in the UI
{: #add-certificate-authority-ui}
{: ui}

You can add certificate authority configurations to your service instance by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets engines** page, click the **Public certificates** tab.
4. In the Certificate authorities table, click **Add**.
5. Select the certificate authority that you want to use. Currently, Let's Encrypt is supported.
   1. To target the production environment, select **Let's Encrypt**.
   2. To target the staging environment, select **Let's Encrypt (Staging)**. Choose this option if you'd like to issue certificates that aren't production-ready yet.
6. Add the private key file in PEM format that's associated with your [ACME account](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#create-acme-account) or enter its value.
7. Click **Add**.

   You can now select this configuration when you order a certificate. To modify or remove an existing configuration, click **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) in the row of the configuration that you want to update.


## Adding a certificate authority configuration from the CLI
{: #add-certificate-authority-cli}
{: cli}

You can add certificate authority configurations to your service instance by using the {{site.data.keyword.secrets-manager_short}} CLI.

To add a configuration, run the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command.

```sh
ibmcloud secrets-manager configuration-create '{
  config_type": "public_cert_configuration_ca_lets_encrypt",
  "lets_encrypt_environment": "production", 
  "lets_encrypt_private_key": "-----BEGIN PRIVATE KEY-----\nMY_PRIVATE_KEY_WITH_NEWLINES_TRANSFORMED_TO_\N_CHARS-----...", "name": "my-lets-encrypt-config"
  }'
```
{: pre}


## Adding a certificate authority configuration with the API
{: #add-certificate-authority-api}
{: api}

You can add certificate authority configurations to your service instance by calling the {{site.data.keyword.secrets-manager_short}} API. 

The following example shows a query that you can use to add a configuration for Let's Encrypt. When you call the API, replace the `private_key` value with the private key that is associated with your [ACME account](/docs/secrets-manager?topic=secrets-manager-prepare-order-certificates#create-acme-account).
{: curl}

Be sure to convert your private key file to single-line format so that it can be parsed correctly by the {{site.data.keyword.secrets-manager_short}} API. You can use the following UNIX command to format the file to a single-line string: `awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' <private_key_file>`
{: tip}


```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
    "config_type": "public_cert_configuration_ca_lets_encrypt",
    "lets_encrypt_environment": "production",
    "lets_encrypt_private_key": "-----BEGIN PRIVATE KEY-----\nMIIEowIBAAKCAQEAqcRbzV1wp0nVrPtEpMtnWMO6Js1q3rhREZluKZfu0Q8SY4H3",
    "name": "lets-encrypt-config"
  }' \
    "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations"

```
{: codeblock}
{: curl}


A successful response adds the configuration to your service instance. For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-config-element){: external}.


## Adding a certificate authority configuration with Terraform
{: #add-certificate-authority-terraform}
{: terraform}

The following example shows a configuration that you can use to create a root certificate authority.

```terraform
    resource "ibm_sm_public_certificate_configuration_ca_lets_encrypt" "my_lets_encrypt_config" {
        instance_id = local.instance_id
        region = local.region
        name = "lets-encrypt-config"
        lets_encrypt_environment = "production"
        lets_encrypt_private_key = var.my_lets_encrypt_private_ley
    }
```
{: codeblock}


## Deleting a certificate authority configuration in the UI
{: #delete-certificate-authority-ui}
{: ui}

If you no longer need a configuration, you can delete it by using the {{site.data.keyword.secrets-manager_short}} UI.

After you delete a configuration, the certificates that are associated with the certificate authority can no longer be rotated automatically. Do not delete configurations that are associated with certificates in your production apps or services.
{: important}

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
   
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}. 
3. In the **Secrets engines** page, click the **Public certificates** tab.
4. Use the **Certificate authorities** table to view the configurations in your instance. 
5. In the row for the configuration that you want to delete, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) **> Delete**.  
6. Enter the name of the configuration to confirm its deletion.
7. Click **Delete**.



## Deleting a certificate authority configuration with the API
{: #delete-certificate-authority-api}
{: api}

You can delete configurations by calling the {{site.data.keyword.secrets-manager_short}} API. 

The following example shows a query that you can use to remove a certificate authority configuration from your instance. When you call the API, replace `{config_name}` with the name of the configuration that you want to delete.
{: curl}

After you delete a configuration, the certificates that are associated with the certificate authority can no longer be rotated automatically. Do not delete configurations that are associated with certificates in your production apps or services.
{: important}


```sh
curl -X DELETE 
  --H "Authorization: Bearer {iam_token}"\
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: codeblock}
{: curl}


A successful response removes the configuration from your service instance. For more information about the required and optional request parameters, see [Remove a configuration](/apidocs/secrets-manager/secrets-manager-v2#delete-config-element){: external}.


## Retrieving a certificate authority configuration in the UI
{: #get-ca-engine-value-ui}
{: ui}

You can retrieve the certificate authority value by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the **Public certificates** secret engine, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) from the Certificate authority table to open a list of options for your engine configuration.
2. To view the configuration value, click **View configurationt**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}


## Retrieving a certificate authority configuration using CLI
{: #get-ca-engine-value-cli}
{: cli}

You can retrieve the certificate authority value by using the {{site.data.keyword.secrets-manager_short}} CLI. In the following example command, replace the engine configuration name with your configuration's name.

```sh
ibmcloud secrets-manager configuration --name EXAMPLE_CONFIG --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud

```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)


## Retrieving a certificate authority configuration using API
{: #get-ca-engine-value-api}
{: api}

You can retrieve the certificate authority value by using the {{site.data.keyword.secrets-manager_short}} API. In the following example request, replace the engine configuration name with your configuration's name.

```sh
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
--header "Accept: application/json" \
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

A successful response returns the value of the engine configuration, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-configuration){: external}.


## Next steps
{: #manage-ca-config-next-steps}

- [Add a DNS provider configuration](/docs/secrets-manager?topic=secrets-manager-add-dns-provider)
- [Order a certificate](/docs/secrets-manager?topic=secrets-manager-public-certificates#order-public-certificates)



