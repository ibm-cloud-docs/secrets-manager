---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-20"

keywords: intermediate certificate authority, intermediate CA, rotate

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

# Rotating certificate authorities certificates
{: #rotating-ca-certificates}

Managing your public key infrastructure (PKI) with {{site.data.keyword.secrets-manager_full}} should take into consideration the life-cycle of the CA chain certificates. Those certificates should be periodically rotated and distributed before their expiration to maintain uninterrupted TLS workflows for their consumers. 

## Rotating a root CA
{: #rotating-root-ca}

### Before you begin
{: #before-rotating-root-ca}

Rotating the root certificate authority is a major change that can impact your entire public-key infrastructure and should be carefully planned ahead of time.

You should plan for a time when both the existing and new root CA certificates overlap to allow the new root CA certificate to be distributed to all consumers.  
It is therefore recommended that you set a long validity period for your root CA certificate. In {{site.data.keyword.secrets-manager_short}}, the default TTL for root certificates is 10 years.

### Rotating your root CA
{: #rotating-your-root-ca}

1. Notify your PKI users about the upcoming rotation plan. The time when the new root CA will be available for download, the time when new leaf certificates will be issued using the new CA chain, and the time when the existing root CA certificate will expire.
2. Follow the [Creating root certificate authorities process](/docs/secrets-manager?topic=secrets-manager-root-certificate-authorities) to create a new root CA.
3. Follow the [Creating intermediate certificate authorities process](/docs/secrets-manager?topic=secrets-manager-intermediate-certificate-authorities) to create your CA chain signed with your new root CA.
4. Distribute the new root CA to allow all consumers to install it in their truststore alongside the existing root CA.
5. Monitor your rotation plan and notify your PKI users about each upcoming milestone event.

## Rotating an intermediate CA
{: #rotating-an-intermediate-ca}

### Before you begin
{: #before-rotating-intermediate-ca}

Notify your PKI users about the upcoming intermediate CA rotation ahead of time.  

An intermediate CA can be rotated inline in case it is not in use to sign other intermediate Certificate Authorities.  
In case you have a multitiered intermediate CA chain, you should create a new intermediate CA chain alongside the existing one ahead of time and migrate your PKI consumers to use the new chain.

### Rotating an intermediate CA inline
{: #rotating-intermdiate-ca-inline}

1. An intermediate CA can be rotated inline in case it is an internally signed CA in active state, and it is only used to sign leaf certificates.
2. Inline intermediate CA rotation will not affect existing leaf certificates that were signed using the previous CA certificate. New leaf certificates will be signed using the new CA certificate.

### Rotating using service UI
{: #rotating-intermediate-ui}
{: ui}

You can rotate the intermediate CA certificate using the {{site.data.keyword.secrets-manager_short}} service UI.

1. In the console, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Resource List**.
2. From the list of services, select your {{site.data.keyword.secrets-manager_short}} instance.
3. In the **Secrets engines** page, click the **Private certificates** tab.
4. In the row for the intermediate CA certificate that you want to rotate, click the **Details** menu ![Actions icon](../icons/actions-icon-vertical.svg).
5. Click the **Actions** button.
6. Select the **Rotate** action and confirm.

### Rotating using CLI
{: #rotating-intermediate-cli}
{: cli}

You can rotate an intermediate CA certificate using the {{site.data.keyword.secrets-manager_short}} CLI.

```sh
ibmcloud secrets-manager configuration-action-create \                                                               
    --name "your-intermediate-ca-name" \
    --config-action-prototype '{"action_type": "private_cert_configuration_action_rotate_intermediate"}' \
    --config-type private_cert_configuration_intermediate_ca
```
{: pre}

### Rotating using API
{: #rotating-intermediate-api}
{: api}

You can rotate an intermediate CA certificate using the {{site.data.keyword.secrets-manager_short}} API:

```sh
curl -X 'POST' \
  --header "accept: application/json" \
  --header "Content-Type: application/json" \
  --header 'X-Sm-Accept-Configuration-Type: private_cert_configuration_intermediate_ca' \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/configurations/{intermediate-configuration-name}/actions" \
  -d '{
      "action_type": "private_cert_configuration_action_rotate_intermediate"
  }'
```
{: pre}

- Replace `{instance_ID}` and `{region}` with the values that apply to your {{site.data.keyword.secrets-manager_short}} service instance. To find the endpoint URL that is specific to your instance, you can copy it from the **Endpoints** page in the {{site.data.keyword.secrets-manager_short}} UI. For more information, see [Viewing your endpoint URLs](/docs/secrets-manager?topic=secrets-manager-endpoints#view-endpoint-urls)
- Replace `{intermediate-configuration-name}` with your intermediate configuration name.
