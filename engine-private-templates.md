---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-30"

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

You can define up to 10 certificate templates per instance. To obtain a list of templates that are available for a specific certificate authority, you can use the [List configurations](/apidocs/secrets-manager#get-secret-config-element) API.
{: note}
{: api}

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
{: curl}



```sh
curl -X POST 'https://{instance_id}.{region}.secrets-manager.appdomain.cloud/api/v1/config/private_cert/certificate_templates' \
-H 'Authorization: Bearer {IAM_token}' \
-H 'Content-Type: application/json' \
-d' {
  "name": "my-exmaple-certificate-template",
  "type": "certificate_template",
  "config": {
    "certificate_authority": "my-example-intermediate-ca",
    "max_ttl": "8760h",
    "allow_any_name": true,
    "enforce_hostnames": false,
    "allowed_uri_sans": [
      "https://www.example.com/test"
    ]
  }
}'
```
{: codeblock}
{: curl}



A successful response adds the template configuration to your service instance. 



```json
{
    "metadata": {
        "collection_type": "application/vnd.ibm.secrets-manager.config+json",
        "collection_total": 1
    },
    "resources": [
        {
            "config": {
                "allow_any_name": true,
                "allow_bare_domains": false,
                "allow_glob_domains": false,
                "allow_ip_sans": true,
                "allow_localhost": true,
                "allow_subdomains": false,
                "allowed_domains": [],
                "allowed_domains_template": false,
                "allowed_other_sans": null,
                "allowed_uri_sans": [
                    "https://www.example.com/test"
                ],
                "basic_constraints_valid_for_non_ca": false,
                "certificate_authority": "my-example-intermediate-ca",
                "client_flag": true,
                "code_signing_flag": false,
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
                "max_ttl": 31536000,
                "not_before_duration": 30,
                "organization": [],
                "ou": [],
                "policy_identifiers": [],
                "postal_code": [],
                "province": [],
                "require_cn": true,
                "server_flag": true,
                "street_address": [],
                "ttl": 0,
                "use_csr_common_name": true,
                "use_csr_sans": true
            },
            "name": "my-example-certificate-template",
            "type": "certificate_template"
        }
    ]
}
```
{: screen}


<apiv2>

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


For more information about the required and optional request parameters, see [Add a configuration](/apidocs/secrets-manager#create-config-element){: external}.

## Next steps
{: #certificate-templates-next-steps}

- [Create a private certificate](/docs/secrets-manager?topic=secrets-manager-certificates#create-certificates)

