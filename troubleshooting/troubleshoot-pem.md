---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-14"

keywords: troubleshoot secrets manager, import certificates doesn't work, can't import certificate, convert crt to pem, convert cer to pem, convert der to pem, convert certificate file to pem

subcollection: secrets-manager

content-type: troubleshoot

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


# Why can't I import my certificate file?
{: #troubleshoot-pem}
{: troubleshoot}

You try to use {{site.data.keyword.secrets-manager_full}} to import an SSL/TLS certificate, but you're unable to complete the action.
{: shortdesc}


You have an unexpired TLS certificate that you want to store in {{site.data.keyword.secrets-manager_short}}. When you try to import the file by using the {{site.data.keyword.secrets-manager_short}} UI, you get the following error:
{: tsSymptoms}

```
Add secret failed
An error occurred and the secret couldn't be added.
```
{: screen}

You also try to import the file by using the {{site.data.keyword.secrets-manager_short}} API, but you get the following error:

```
Unable to parse the certificate
```
{: screen}

{{site.data.keyword.secrets-manager_short}} supports X.509 certificate files in the `.pem` format only. However, you might be working with a certificate that is in a different file format. For example, X.509 certificates can have a variety of file extension types, including:
{: tsCauses}

- Certificate (`.crt`) or (`.cer`)
- Distinguished encoding rules (`.der`)
- Privacy-enhanced electronic mail (`.pem`)

To resolve the issue, ensure that your certificate file is in the supported format before you import it to {{site.data.keyword.secrets-manager_short}}.
{: tsResolve}

1. Use the `openssl` utility to convert an X.509 certificate to the `.pem` format.

    To convert a `.crt` file to `.pem`, run the following command:

    ```sh
    openssl x509 -in cert.crt -out cert.pem
    ```
    {: pre}

    To convert a `.cer` file to `.pem`, run the following command:

    ```sh
    openssl x509 -in cert.cer -out cert.pem
    ```
    {: pre}

    To convert a `.der` file to `.pem`, run the following command:

    ```sh
    openssl x509 -in cert.der -out cert.pem
    ```
    {: pre}

2. Optional: If you're using the {{site.data.keyword.secrets-manager_short}} API to import your certificate, ensure that the data is formatted correctly.

    You can use the following UNIX command to format your `.pem` file to a single-line string can be passed to the {{site.data.keyword.secrets-manager_short}} API:

    ```sh
    awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' cert.pem
    ```
    {: pre}






