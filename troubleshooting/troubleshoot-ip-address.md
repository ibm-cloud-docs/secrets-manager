<staging>---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-03"

keywords: can't create IAM credentials, can't regenerate IAM credentials, IAM credentials not working

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

# Why am I blocked from regenerating IAM credentials and ordering certificates?
{: #ip-address-not-allowed}
{: troubleshoot} 
{: support}

You try to use {{site.data.keyword.secrets-manager_full}} to generate IAM credentials or order public certificates, but the service is unable to complete the action.
{: shortdesc}

When you try to complete an action in {{site.data.keyword.secrets-manager_short}} that uses an API key to access another service, for example when you generate IAM credentials or order a public certificate for domains that are managed in Cloud Internet Services (CIS), you encounter the following error:
{: tsSymptoms}

```json
TBD - IAM credentials couldn't be regenerated because IP address restrictions are enabled for the account. 
```
{: screen}

```json
TBD - Cloud Internet Services (CIS) couldn't be reached because IP address restrictions are enabled for the account. 
```
{: screen}

This error can occur when {{site.data.keyword.secrets-manager_short}} attempts to log in to the target account with the configured API key in order to complete the requested action, but the service is unable to do so because the account allows access to specific IP addresses only. To allow the account to accept requests from {{site.data.keyword.secrets-manager_short}}, you must specify a required list of IP addresses, along with your own IP address.
{: tsCauses}

To resolve the issue, ensure that the IP address restriction settings in the account are updated to allow the IP addresses that correspond with the region where your {{site.data.keyword.secrets-manager_short}} is provisioned.
{: tsResolve}

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage > Access (IAM)**, and select **Settings**.
2. From the Account restrictions section, edit the **IP address access** setting.
3. In the Allowed IP addresses field, include the IP addresses for {{site.data.keyword.secrets-manager_short}}.

   The following table lists the IP addresses in CIDR format that are required to allow access to the service by region. If your {{site.data.keyword.secrets-manager_short}} instance is accessible only through [private service endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints#service-endpoints), include the list of private IP addresses.

    | Region        | IP addresses                                                  |
    | ------------- | ---------------------------------------------------------------- |
    | Dallas        | `50.22.138.0/27`, `52.116.190.80/28`, `52.116.210.0/26`, `52.117.32.240/28`, `169.46.88.224/27`, `169.59.238.112/28`, `169.61.33.64/26`, `169.61.239.64/26`, `169.62.221.96/27` |
    | Frankfurt     | `149.81.102.16/28`, `149.81.115.160/27`, `158.177.85.192/28`, `158.177.105.160/27`, `158.177.142.128/26`, `161.156.26.224/27`, `161.156.95.240/28`    |
    | London        | `141.125.99.192/28`, `141.125.137.0/27`, `158.175.70.224/28`, `158.175.88.224/27`, `158.176.66.32/27`, `158.176.121.0/28`    |
    | Osaka         | `10.8.11.192/26`, `10.9.29.128/26`, `10.10.21.0/26`   |
    | Sao Paulo     | `163.107.67.144/28`, `163.109.69.32/28`, `169.57.207.96/27`, `169.57.229.112/28`   |
    | Sydney        | `130.198.71.240/28`, `130.198.78.32/27`, `135.90.90.80/28`, `135.90.91.0/27`, `159.23.76.240/28`, `159.23.77.224/27`   |
    | Tokyo         | `128.168.96.160/28`, `128.168.151.192/27`, `161.202.97.48/28`, `165.192.69.160/27`, `165.192.86.192/28`, `169.56.26.96/27`   |
    | Toronto       | `163.74.70.208/28`, `163.75.66.192/28`, `169.48.17.192/27`, `169.59.70.192/28`   |
    | Washington DC | `52.117.74.160/28`, `169.47.43.160/28`, `169.47.63.0/27`, `169.60.79.96/27`, `169.61.98.32/27`, `169.63.169.208/28`  |
    {: caption="Table 1. IP addresses in CIDR format that correspond with {{site.data.keyword.secrets-manager_short}} regions - Public" caption-side="top"}
    {: #public-instances}
    {: tab-title="Public"}
    {: tab-group="secrets-manager-ips"}
    {: class="simple-tab-table"}

    | Region        | IP addresses                                                         |
    | ------------- | ------------------------------------------------------------------------ |
    | Dallas        | `10.74.45.0/26`, `10.209.252.0/26`, `10.221.200.128/26` |
    | Frankfurt     | `10.13.1.0/26`, `10.75.78.128/26`, `10.194.143.0/26`    |
    | London        | `10.45.251.0/26`, `10.72.214.64/26`, `10.196.87.0/26`    |
    | Osaka         | `163.68.76.0/28`, `163.68.97.64/27`, `163.69.71.32/28`, `163.73.69.224/28`   |
    | Sao Paulo     | `10.14.24.192/26`, `10.15.21.0/26`, `10.151.30.128/26`   |
    | Sydney        | `10.63.39.0/26`, `10.195.87.192/26`, `10.210.40.192/26`   |
    | Tokyo         | `10.132.179.64/26`, `10.192.247.0/26`, `10.193.133.192/26`   |
    | Toronto       | `10.11.57.0/26`, `10.166.179.192/26`, `10.243.75.64/26`   |
    | Washington DC | `10.183.18.128/26`, `10.188.107.0/26`, `10.191.218.64/26`  |
    {: caption="Table 1. IP addresses in CIDR format that correspond with {{site.data.keyword.secrets-manager_short}} regions - Private" caption-side="top"}
    {: #private-instances}
    {: tab-title="Private"}
    {: tab-group="secrets-manager-ips"}
    {: class="simple-tab-table"}

    