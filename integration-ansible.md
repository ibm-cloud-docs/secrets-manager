---

copyright:
  years: "2023"
lastupdated: "2023-06-15"

keywords: integration, tool integrations, ansible, integrations

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

# Integrating with Ansible
{: #integration-ansible}

Do you store secrets by using Ansible? Migrate to the {{site.data.keyword.secrets-manager_full}} Standard plan to maintain any required secrets before you can migrate your operations to use {{site.data.keyword.secrets-manager_short}}. 
{: shortdesc}

## Before you begin 
{: #before-ansible-integration}

1. Create a Standard plan {{site.data.keyword.secrets-manager_short}} instance.
2. Create an IAM API key by using a service ID that has `SecretReader` permissions.

## Retrieving credentials 
{: #integration-ansible-setup}

By following these steps, you can retrieve {{site.data.keyword.secrets-manager_short}} secrets by using Ansible. 

1. Create your secrets. The following code samples use key-value and user credentials secrets as examples to demonstrate the retrieval process. 
2. Get the ID of the secrets from the secret details panel. 
3. Run the following Ansible playbook and set the values of your `SM_INSTANCE` and `IBMCLOUD_API_KEY` as `extra-vars`.
4. Set your user credentials secret ID as the value in the `lookup` and `parsing` tasks. 

```sh
- name: IBM Secrets Manager Standard Example
  gather_facts: false
  hosts: localhost
  connection: local
  vars:
    api_key: "{{ IBMCLOUD_API_KEY }}"
    secret_manager_instance_id: "{{ SM_INSTANCE }}"
    region: "us-south"
    hostname_vault: "https://{{ secret_manager_instance_id }}.{{ region }}.secrets-manager.appdomain.cloud"
  tasks:
    - name: Create IAM Token
      uri:
        url: https://iam.cloud.ibm.com/identity/token
        headers:
          Content-Type: application/x-www-form-urlencoded
          Accept: application/json
        body_format: form-urlencoded
        method: POST
        body:
          grant_type: "urn:ibm:params:oauth:grant-type:apikey"
          apikey: "{{ api_key }}"
      register: login

    - block:
        - name: Setting IAMtoken
          set_fact:
            iam_token: "{{ login.json.access_token }}"

        - name: Create Vault Token
          uri:
            url: "{{ hostname_vault }}/v1/auth/ibmcloud/login"
            headers:
              Content-Type: application/json
              Accept: application/json
            body_format: json
            method: PUT
            body: '{"token": "{{ iam_token }}" }'
          register: token_vault_rest_call

        - name: Set vault token
          set_fact:
            vault_token: "{{ token_vault_rest_call.json.auth.client_token }}"


        - name: Lookup KV secret with token
          ansible.builtin.debug:
            msg: "{{ lookup('community.hashi_vault.hashi_vault', 'secret=ibmcloud/kv/data/mykvsecret:key1 token={{ vault_token }} url={{ hostname_vault }}') }}"

        - name: Lookup User Credentials secret with token - full
          vars:
            secret_id: "dc1d3b5a-176f-aea4-8124-7073f53dcf82"
          ansible.builtin.debug:
            msg: "{{ lookup('community.hashi_vault.hashi_vault', 'secret=ibmcloud/username_password/secrets/{{ secret_id }} token={{ vault_token }} url={{ hostname_vault }}') }}"

        - name: Parsing username_password
          vars:
            secret_id: "dc1d3b5a-176f-aea4-8124-7073f53dcf82"
            secret_data: "{{ lookup('community.hashi_vault.hashi_vault', 'secret=ibmcloud/username_password/secrets/{{ secret_id }}:secret_data token={{ vault_token }} url={{ hostname_vault }}') | to_json }} "
          ansible.builtin.debug:
            msg: "user is {{ secret_data.username }} and password is {{ secret_data.password }}"

      when: login.status == 200
```
{: codeblock}


A successful request returns the following response.


```sh
TASK [Lookup KV secret with token] *****************************************************************************************************
ok: [localhost] => {
    "msg": "secret1"
}

TASK [Lookup User Credentials secret with token - full] ********************************************************************************
ok: [localhost] => {
    "msg": {
        "created_by": "xxxxxxxxxxxxx",
        "creation_date": "2023-01-19T10:15:54Z",

        xxxxxx REDACTED xxxxxxxx

        "secret_data": {
            "password": "pass1",
            "username": "user1"
        },
        "secret_type": "username_password",
        "state": 1,
        "state_description": "Active",
        "versions": [
            {
                "auto_rotated": false,
                "created_by": "xxxxxxxxxx",
                "creation_date": "2023-01-19T10:15:54Z",
                "downloaded": true,
                "id": "373ff4a1-64a7-d6b0-993c-605ba564540d",
                "payload_available": true,
                "version_custom_metadata": {}
            }
        ],
        "versions_total": 1
    }
}

TASK [Parsing username_password] *******************************************************************************************************
ok: [localhost] => {
    "msg": "user is user1 and password is pass1"
```
{: codeblock}



## Next steps
{: integration-ansible-next}

You can run this playbook to look up other secrets from {{site.data.keyword.secrets-manager_short}}.

