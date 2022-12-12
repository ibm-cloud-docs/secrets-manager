---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-08"

keywords: IP addresses for Secrets Manager, IP ranges for Secrets Manager, allowlist Secrets Manager

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

# IP addresses for {{site.data.keyword.secrets-manager_short}}
{: #ip-addresses}

When you work in an {{site.data.keyword.cloud}} account that allows access to only specific IP address, you might need to specify a required list of IP addresses to allow access to {{site.data.keyword.secrets-manager_short}}. Review the list of IP addresses for {{site.data.keyword.secrets-manager_short}}, grouped by network connection and region, that you can use to update the IP address settings for your account.
{: shortdesc}

To learn more about updating your IP address settings, see [Allowing specific IP addresses for an account](/docs/account?topic=account-ips#ips_account).

## List of IP addresses for {{site.data.keyword.secrets-manager_short}}
{: #ip-addresses-list}

Depending on how you connect to your {{site.data.keyword.secrets-manager_short}} service instance, you might need to specify a list of either public IP addresses, private IP addresses, or both.

### Public IP addresses
{: #ip-addresses-public}

If you connect to your {{site.data.keyword.secrets-manager_short}} instance through [public service endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints#service-endpoints), include the following list of public IP addresses in CIDR format.

| Region        | IP addresses                                                  |
| ------------- | ---------------------------------------------------------------- |
| Dallas        | `50.22.138.0/27`, `52.116.190.80/28`, `52.116.210.0/26`, `52.117.32.240/28`, `169.46.88.224/27`, `169.59.238.112/28`, `169.61.33.64/26`, `169.61.239.64/26`, `169.62.221.96/27` |
| Frankfurt     | `149.81.102.16/28`, `149.81.115.160/27`, `158.177.85.192/28`, `158.177.105.160/27`, `158.177.142.128/26`, `161.156.26.224/27`, `161.156.95.240/28`    |
| London        | `141.125.99.192/28`, `141.125.137.0/27`, `158.175.70.224/28`, `158.175.88.224/27`, `158.176.66.32/27`, `158.176.121.0/28`    |
| Osaka         | `163.68.76.0/28`, `163.68.97.64/27`, `163.69.71.32/28`, `163.73.69.224/28`  |
| Sao Paulo     | `163.107.67.144/28`, `163.109.69.32/28`, `169.57.207.96/27`, `169.57.229.112/28`   |
| Sydney        | `130.198.71.240/28`, `130.198.78.32/27`, `135.90.90.80/28`, `135.90.91.0/27`, `159.23.76.240/28`, `159.23.77.224/27`   |
| Tokyo         | `128.168.96.160/28`, `128.168.151.192/27`, `161.202.97.48/28`, `165.192.69.160/27`, `165.192.86.192/28`, `169.56.26.96/27`   |
| Toronto       | `163.74.70.208/28`, `163.75.66.192/28`, `169.48.17.192/27`, `169.59.70.192/28`   |
| Washington DC | `52.117.74.160/28`, `169.47.43.160/28`, `169.47.63.0/27`, `169.60.79.96/27`, `169.61.98.32/27`, `169.63.169.208/28`  |
{: caption="Table 1. Public IP addresses in CIDR format that correspond with {{site.data.keyword.secrets-manager_short}} regions" caption-side="top"}

### Private IP addresses
{: #ip-addresses-private}

If you connect to your {{site.data.keyword.secrets-manager_short}} instance through [private service endpoints](/docs/secrets-manager?topic=secrets-manager-endpoints#service-endpoints), include the following list of private IP addresses in CIDR format.

| Region        | IP addresses                                                         |
| ------------- | ------------------------------------------------------------------------ |
| Dallas        | `10.74.45.0/26`, `10.209.252.0/26`, `10.221.200.128/26` |
| Frankfurt     | `10.13.1.0/26`, `10.75.78.128/26`, `10.194.143.0/26`    |
| London        | `10.45.251.0/26`, `10.72.214.64/26`, `10.196.87.0/26`    |
| Osaka         | `10.8.11.192/26`, `10.9.29.128/26`, `10.10.21.0/26`   |
| Sao Paulo     | `10.14.24.192/26`, `10.15.21.0/26`, `10.151.30.128/26`   |
| Sydney        | `10.63.39.0/26`, `10.195.87.192/26`, `10.210.40.192/26`   |
| Tokyo         | `10.132.179.64/26`, `10.192.247.0/26`, `10.193.133.192/26`   |
| Toronto       | `10.11.57.0/26`, `10.166.179.192/26`, `10.243.75.64/26`   |
| Washington DC | `10.183.18.128/26`, `10.188.107.0/26`, `10.191.218.64/26`  |
{: caption="Table 2. Private IP addresses in CIDR format that correspond with {{site.data.keyword.secrets-manager_short}} regions" caption-side="top"}


