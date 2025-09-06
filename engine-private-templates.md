---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-06"

keywords: certificate parameters, certificate templates

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

# Adding certificate templates
{: #certificate-templates}

A certificate template helps you to define the parameters to apply to the private certificates that you create in your {{site.data.keyword.secrets-manager_full}} service instance. By using a certificate template, you can include parameters to control certificate common names, alternate names, the key uses that they are valid for, and more.
{: shortdesc}

You can define up to 10 certificate templates per instance. To view a list of templates that are available for a specific certificate authority, go to the **Secrets engines > Private certificates** page in the {{site.data.keyword.secrets-manager_short}} UI.
{: note}
{: ui}

You can define up to 10 certificate templates per instance. To obtain a list of templates that are available for a specific certificate authority, you can use the [List configurations](/apidocs/secrets-manager/secrets-manager-v2#get-configuration) API.
{: note}
{: api}

## Specifying allowed domains for generated certificates
{: #allowed-domains-generated-certificates}
{: ui}

You can specify the domains that are allowed for the certificates that you generate.

- Use the **Domains settings** section to add the domains that the template allows. Then, the list is used with the **Allow bare domain** and **Allow subdomains** options to determine the type of matching between these domains and the domains for which clients can request certificates.
- Enable the **Allow bare domains** option to specify whether clients can request certificates that match the domains that the template allows.
- Enable the **Allow subdomains** option to indicate whether clients can request certificates that match subdomains of the domains that this template allows. Wildcard subdomains are included in this option.

## Before you begin
{: #before-certificate-templates}

Before you get started, be sure that you have the required level of access. To manage engine configurations for your instance, you need the [**Manager** service role or higher](/docs/secrets-manager?topic=secrets-manager-iam).


## Adding a certificate template in the UI
{: #certificate-templates-ui}
{: ui}

[After you create an intermediate certificate authority](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities) for your instance, you can create a certificate template by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your instance of {{site.data.keyword.secrets-manager_short}}.
3. In the **Secrets engines** page, click the **Private certificates** tab.
4. In the Certificate authorities table, expand the row of the intermediate certificate authority that you want to use as the issuing CA for your private certificates.
5. Click the **Templates** tab to display your existing certificate templates.
6. Click **Add template** to associate a new template.
7. Provide a name to easily identify your template.
8. Optional: Set a time-to-live (TTL) for the generated certificates.

    By setting a TTL, you determine how long the certificates that are issued by the CA remain valid. After the certificate reaches the end of its lease, it is revoked automatically.

    The TTL or validity period that you define on a certificate template can't exceed the maximum TTL value that is defined for the associated CA. For more information, see [Choosing a validity period for your certificates](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-validity-period).
    {: note}

9. Optional: [Select the key algorithm](/docs/secrets-manager?topic=secrets-manager-prepare-create-certificates#choose-key-algorithm) that you want to use to generate the keys for your generated certificates.
10. Optional: Select the [secret groups](#x9968962){: term} that you want to assign for your generated certificates.

    By selecting one or more secret groups from the list, you restrict the creation of private certificates to those groups only. For more information about secret groups, check out [Organizing your secrets](/docs/secrets-manager?topic=secrets-manager-secret-groups).

11. Optional: Enable advanced options for generated private certificates.

    1. **Domain settings**: Add specific domains, subdomains, or wildcards to apply to your generated private certificates.
    2. **Certificate roles**: Flag your generated private certificates for specific uses.
    3. **Subject name**: Apply Subject Name parameter fields to your generated private certificates.

12. To confirm your selections, click **Add**.


## Adding a certificate template with the API
{: #certificate-templates-api}
{: api}

You can create a certificate template for your service instance by calling the {{site.data.keyword.secrets-manager_short}} API.

The following example shows a query that you can use to create a certificate template and associate it with an existing intermediate certificate authority that is configured for your instance.
{: note}


```sh
curl -X POST 
  --H "Authorization: Bearer {iam_token}" \
  --H "Accept: application/json" \
  --H "Content-Type: application/json" \
  --d '{
  "config_type": "private_cert_configuration_template",
  "name": "test-certificate-template",
  "allow_any_name": true,
  "allowed_uri_sans": [
    "https://www.example.com/test"
  ],
  "certificate_authority": "test-intermediate-CA",
  "enforce_hostnames": false,
  "max_ttl": "8760h"
}' \  
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations"
```
{: codeblock}
{: curl}


A successful response adds the template configuration to your service instance. 



```json
{
  "allow_any_name": true,
  "allow_bare_domains": false,
  "allow_glob_domains": false,
  "allow_ip_sans": true,
  "allow_localhost": true,
  "allow_subdomains": false,
  "allowed_domains": [],
  "allowed_domains_template": false,
  "allowed_other_sans": [],
  "allowed_uri_sans": [
    "https://www.example.com/test"
  ],
  "basic_constraints_valid_for_non_ca": false,
  "certificate_authority": "test-intermediate-CA",
  "client_flag": true,
  "code_signing_flag": false,
  "config_type": "private_cert_configuration_template",
  "created_at": "2022-06-27T11:58:15Z",
  "created_by": "iam-ServiceId-e4a2f0a4-3c76-4bef-b1f2-fbeae11c0f21",
  "country": [],
  "email_protection_flag": false,
  "enforce_hostnames": false,
  "ext_key_usage": [],
  "ext_key_usage_oids": [],
  "key_bits": 2048,
  "key_type": "rsa",
  "key_usage": [
    "DigitalSignature",
    "KeyAgreement",
    "KeyEncipherment"
  ],
  "locality": [],
  "max_ttl_seconds": 31536000,
  "name": "test-certificate-template",
  "not_before_duration_seconds": 30,
  "organization": [],
  "ou": [],
  "policy_identifiers": [],
  "postal_code": [],
  "province": [],
  "require_cn": true,
  "secret_type": "private_cert",
  "server_flag": true,
  "street_address": [],
  "ttl_seconds": 43200,
  "updated_at": "2022-10-05T21:33:11Z",
  "use_csr_common_name": true,
  "use_csr_sans": true
}
```
{: screen}


## Adding a certificate template from the CLI
{: #certificate-templates-cli}
{: cli}

You can create a certificate template for your service instance by using the [**`ibmcloud secrets-manager configuration-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-configuration-create-command) command.

The following example shows a command that you can use to create a certificate template and associate it with an existing intermediate certificate authority that is configured for your instance.
{: note}


```sh
ibmcloud secrets-manager configuration-create 
  --configuration-prototype='{
    "config_type": "private_cert_configuration_template",
    "name": "example-certificate-template",
    "allow_any_name": true,
    "allow_bare_domains": true,
    "allow_glob_domains": true,
    "allow_ip_sans": true,
    "allow_localhost": true,
    "allow_subdomains": false,
    "allow_wildcard_certificates": true,
    "allowed_domains": ["example.com","acme.com"],
    "allowed_domains_template": true,
    "allowed_other_sans": [
      "1.2.3.5.4.3.201.10.4.3;utf8:test@example.com",
      "1.3.6.1.4.1.201.10.5.5;UTF-8:*"
      ],
    "allowed_secret_groups": "d898bb90-82f6-4d61-b5cc-b079b66cfa76",
    "allowed_uri_sans": ["example.com","acme://*"],
    "certificate_authority": "example-intermediate-CA",
    "client_flag": true,
    "code_signing_flag": false,
    "email_protection_flag": false,
    "enforce_hostnames": false,
    "key_bits": 2048,
    "key_type": "rsa",
    "key_usage": [
      "DigitalSignature",
      "KeyAgreement",
      "KeyEncipherment"
    ],
    "max_ttl": "24h",
    "server_flag": true,
    "ttl": "8h",
    "use_csr_common_name": true,
    "use_csr_sans": true
  }'
```
{: pre}


## Adding a certificate template with Terraform
{: #certificate-templates-terraform}
{: terraform}

You can create a certificate template for your service instance by using Terraform for {{site.data.keyword.secrets-manager_short}}.

The following example shows a configuration that you can use to create a certificate template and associate it with an existing intermediate certificate authority that is configured for your instance.


```terraform
    resource "ibm_sm_private_certificate_configuration_template" "test_ca_template" {
        instance_id = local.instance_id
        region = local.region
        name = "test-ca-template"
        certificate_authority = ibm_sm_private_certificate_configuration_intermediate_ca.test_int_ca.name
        allowed_domains = ["example1.com", "my.example.com"]
        allow_any_name = true
    }
```
{: codeblock}

For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager/secrets-manager-v2#create-configuration){: external}.


## Retrieving a certificate template in the UI
{: #get-certificate-template-value-ui}
{: ui}

You can retrieve the certificate template value by using the {{site.data.keyword.secrets-manager_short}} UI.

1. In the **Public certificates** secret engine, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your engine configuration.
2. To view the configuration value, click **View configuration**.
2. Click **Confirm** after you ensure that you are in a safe environment.

The secret value is displayed for 15 seconds, then the dialog closes.
{: note}


## Retrieving a certificate template using CLI
{: #get-certificate-template-value-cli}
{: cli}

You can retrieve the certificate template value by using the {{site.data.keyword.secrets-manager_short}} CLI. In the following example command, replace the engine configuration name with your configuration's name.

```sh
ibmcloud secrets-manager configuration --name EXAMPLE_CONFIG --service-url https://{instance_ID}.{region}.secrets-manager.appdomain.cloud

```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)


## Retrieving a certificate template using API
{: #get-certificate-template-value-api}
{: api}

You can retrieve the certificate template value by using the {{site.data.keyword.secrets-manager_short}} API. In the following example request, replace the engine configuration name with your configuration's name.

```sh
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
--header "Accept: application/json" \
"https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{name}"
```
{: pre}

Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)

A successful response returns the value of the engine configuration, along with other metadata. For more information about the required and optional request parameters, see [Get a secret](/apidocs/secrets-manager/secrets-manager-v2#get-configuration){: external}.


## Next steps
{: #certificate-templates-next-steps}

- [Create a private certificate](/docs/secrets-manager?topic=secrets-manager-private-certificates#create-private-certificates).
