---

copyright:
  years: 2020
lastupdated: "2020-12-15"

keywords: Data security for Secrets Manager, byok, kyok, data storage, data encryption in Secrets Manager, customer managed keys

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# Securing your data in {{site.data.keyword.secrets-manager_short}}
{: #mng-data}

To ensure that you can securely manage your data when you use {{site.data.keyword.secrets-manager_full}}, it is important to know exactly what data is stored and encrypted and how you can delete it. Data encryption by using customer-managed keys is supported by using {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} with {{site.data.keyword.secrets-manager_short}}.
{: shortdesc}

## How your data is stored and encrypted in {{site.data.keyword.secrets-manager_short}}
{: #data-storage}

When you work with the {{site.data.keyword.secrets-manager_short}} service, you store secrets that allow users or services to access your protected resources. Your secrets are encrypted at rest by using [envelope encryption](#x9860393){: term}. At no time are your credentials available in clear text while they are stored by the service.

{{site.data.keyword.secrets-manager_short}} also uses the following security mechanisms to protect your data in transit.

- TLS 1.2+ for end to end communications
- mTLS for internal communications
- Web App Firewall and DDoS protection  
- Ingress and Egress network rules to isolate your dedicated instance


## Protecting your sensitive data in {{site.data.keyword.secrets-manager_short}}
{: #data-encryption}

You can add a higher level of encryption control to your data at rest (when it is stored) by enabling integration with {{site.data.keyword.keymanagementservicelong_notm}}.

The data that you store in {{site.data.keyword.cloud_notm}} is encrypted at rest by using [envelope encryption](#x9860393){: term}. If you need to control the encryption keys, you can integrate {{site.data.keyword.keymanagementserviceshort}}. This process is commonly referred to as Bring your own keys (BYOK). With {{site.data.keyword.keymanagementserviceshort}} you can create, import, and manage encryption keys. You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific service. 

### About customer-managed keys
{: #about-encryption}

{{site.data.keyword.secrets-manager_short}} uses [envelope encryption](#x9860393){: term} to implement both provider-managed or customer-managed keys. Envelope encryption describes encrypting one encryption key with another encryption key. The key used to encrypt the actual data is known as a [data encryption key (DEK)](#x4791827){: term}. The DEK itself is never stored but is wrapped by a second key that is known as the key encryption key (KEK) to create a wrapped DEK. To decrypt data, the wrapped DEK is unwrapped to get the DEK. This process is possible only by accessing the KEK, which in this case is your root key that is stored in {{site.data.keyword.keymanagementserviceshort}}.

{{site.data.keyword.keymanagementserviceshort}} keys are secured by FIPS 140-2 Level 3 certified cloud-based [hardware security modules (HSMs)](#x6704988){: term}.


### Enabling customer-managed keys for {{site.data.keyword.secrets-manager_short}}
{: #using-byok}

If you choose to work with a key that you manage, you must ensure that valid IAM authorization is assigned to the instance of {{site.data.keyword.secrets-manager_short}} that you're working with. To create that authorization, you can use the following steps.

As a beta service, {{site.data.keyword.secrets-manager_short}} does not yet support state changes of the {{site.data.keyword.keymanagementserviceshort}} root key that you enable for the service.
{: note}

1. [Create an instance of {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision#provision-gui).
2. [Generate or import your own root key](/docs/key-protect?topic=key-protect-create-root-keys) to your instance of {{site.data.keyword.keymanagementserviceshort}}.

    When you use {{site.data.keyword.keymanagementserviceshort}} to create a root key, the service generates cryptographic key material that is rooted in cloud-based HSMs. Be sure that the name of your key does not contain any personal information such as your name or location.
3. Grant service access to {{site.data.keyword.keymanagementserviceshort}}.

    You must be the account owner or an administrator for the instance of {{site.data.keyword.keymanagementserviceshort}} that you're working with. You must also have at least Viewer access for the {{site.data.keyword.secrets-manager_short}} service.

    1. Go to **Manage > Access IAM > Authorizations**.
    2. Select the {{site.data.keyword.secrets-manager_short}} service as the source service.
    3. Select the instance of the {{site.data.keyword.keymanagementserviceshort}} as the target service.
    4. Select the key that you created in the previous steps.
    5. Assign the Reader role.
    6. Click **Authorize** to confirm the authorization.

4. Create an instance of the {{site.data.keyword.secrets-manager_short}} service. 

    1. Select the region that corresponds to the region for the instance of {{site.data.keyword.keymanagementserviceshort}} that you created previously.
    2. Select your **{{site.data.keyword.keymanagementserviceshort}} instance**.
    3. Select the **Root key** that you previously authorized.
    4. Click **Create**.

## Deleting your data in {{site.data.keyword.secrets-manager_short}}
{: #data-delete}


When you delete a secret, secret group, or an instance of {{site.data.keyword.secrets-manager_short}}, your data is removed from {{site.data.keyword.secrets-manager_short}}. For more information about the {{site.data.keyword.secrets-manager_short}} data retention policy, see the [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).
