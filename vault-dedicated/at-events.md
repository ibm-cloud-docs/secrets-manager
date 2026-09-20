---

copyright:
  years: 2020, 2026
lastupdated: "2026-09-20"

keywords: activity tracking events for Vault Dedicated, Vault Dedicated events, Secrets Manager Vault Dedicated audit events

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
{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for the Vault Dedicated plan
{: #vault-dedicated-at-events}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.secrets-manager_full}}, generate activity tracking events. 
{: shortdesc}

Vault audit devices are service managed in Vault Dedicated.
{: note}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.logs_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-about-cl).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed to an {{site.data.keyword.logs_full_notm}} instance.

## Locations where activity tracking events are generated
{: #at-locations}

The following table lists the regions where Vault Dedicated sends activity tracking events to {{site.data.keyword.logs_full_notm}}.

| Dallas (`us-south`) | Frankfurt (`eu-de`) |
|---------------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent for Vault Dedicated plan" caption-side="bottom"}

## Viewing activity tracking events for Vault Dedicated
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on starting the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI](/docs/cloud-logs?topic=cloud-logs-instance-launch).

## Analyzing events
{: #at-analyze}

Vault Dedicated plan instances generate successful events that contain various fields to help you identify the initiator, the target resource, and the outcome of each completed action.

You can create views and alerts from all your Vault Dedicated instances, or from a specific instance.  
To target a specific instance, replace `host:secrets-manager` with `app:{INSTANCE_CRN}`.

### Query for finding all Vault Dedicated actions
{: #query-all-at}

Run the following query to find all Vault Dedicated instance management actions.

```sh
host:secrets-manager action:secrets-manager.instance.read OR action:secrets-manager.admin-token.create OR action:secrets-manager.admin-tokens.delete
```
{: codeblock}

The action value can be replaced with any other applicable action.
{: note}

### Query for finding unauthorized access attempts
{: #query-specific-at}

To see unauthorized access attempts, run the following query.

```sh
host:secrets-manager reason.reasonType:Unauthorized
```
{: codeblock}

## Understanding generated events
{: #gen-events}

The following events are generated for Vault Dedicated instance management operations.

### Instance operations events
{: #at-configuration-instance-operations}

The following table lists the instance operation actions that generate an event.

| Action                                     | Description                      |
| ------------------------------------------ | -------------------------------- |
| `secrets-manager.instance.read` | Read the details of a Vault Dedicated plan for instance. |
| `secrets-manager.admin-token.create` | Generate an admin token for initial Vault access. |
| `secrets-manager.admin-tokens.delete` | Revoke active admin tokens for a Vault Dedicated plan instance. |
{: caption="List of instance operation events for Vault Dedicated plan" caption-side="bottom"}



## How events are built
{: #how-events-are-built}

Activity tracking events for the Vault Dedicated plan are dynamically constructed from underlying vault operations and request metadata. Review the following details to understand how key event properties, such as actions, severity levels, and outcomes, are mapped and filtered. 

## Instance management operations
{: #instance-management-operations}

The {{site.data.keyword.secrets-manager_short}} service layer handles instance management operations as part of the {{site.data.keyword.cloud_notm}} control plane.

The following table lists the activity tracking events for instance management operations:

| Action | Description | Severity |
|---|---|---|
| `secrets-manager.instance.read` | Read instance details | `normal` |
| `secrets-manager.admin-token.create` | Generate admin token (`auth/token/create-orphan` in `admin/` namespace) | `critical` |
| `secrets-manager.admin-tokens.delete` | Revoke admin tokens (`auth/token/revoke-accessor` in `admin/` namespace) | `critical` |
{: caption="Instance management operation events" caption-side="bottom"}



## Key value secrets operations
{: #kv-secrets-operations}

Key value secrets operations generate activity tracking events when you create, retrieve, update, or delete secrets and secret metadata. These events use dynamic action patterns that map vault secret paths to event action strings.

The following table lists the activity tracking events for key value secrets operations:

| Action pattern | Vault path | Vault operation | Severity |
|---|---|---|---|
| `secrets-manager.data.<path>` | `secrets/data/<path>` | `create` | `warning` |
| `secrets-manager.data.<path>` | `secrets/data/<path>` | `read` | `normal` |
| `secrets-manager.data.<path>` | `secrets/data/<path>` | `update` | `warning` |
| `secrets-manager.data.<path>` | `secrets/data/<path>` | `delete` | `critical` |
| `secrets-manager.metadata.<path>` | `secrets/metadata/<path>` | `list` | `normal` |
{: caption="Key-value (KV) secrets operation events" caption-side="bottom"}
 
## Access control list policy operations
{: #acl-policy-operations}

Access control list policy operations generate activity tracking events when you create, view, update, delete, or list access control list policies. These events use dynamic action patterns that are constructed by appending the policy name to the event action string.

The following table lists the activity tracking events for access control list policy operations:

| Action pattern | Vault path | Vault operation | Severity |
|---|---|---|---|
| `secrets-manager.policies.acl.<name>` | `sys/policies/acl/<name>` | `update` (create or update) | `warning` |
| `secrets-manager.policies.acl.<name>` | `sys/policies/acl/<name>` | `read` | `normal` |
| `secrets-manager.policies.acl` | `sys/policies/acl/` | `list` | `normal` |
| `secrets-manager.policies.acl.<name>` | `sys/policies/acl/<name>` | `delete` | `critical` |
{: caption="Access Control List policy operation events" caption-side="bottom"}
 
## Token operations
{: #token-operations}

Token operations generate activity tracking events when authentication tokens are created, inspected, renewed, or revoked. To help monitor these events, you can track token lifecycles and secure client access to your instance resources.

The following table lists the activity tracking events for token operations:

| Action | Vault path | Vault operation¹ | Severity |
|---|---|---|---|
| `secrets-manager.token.create` | `auth/token/create` | `update` | `warning` |
| `secrets-manager.token.lookup` | `auth/token/lookup` | `update` | `warning` |
| `secrets-manager.token.renew` | `auth/token/renew` | `update` | `warning` |
| `secrets-manager.token.revoke` | `auth/token/revoke` | `update` | `warning` |
{: caption="Token operation events" caption-side="bottom"}
 
Vault records all `POST` calls to `auth/token/*` as `update` in its audit log. Therefore, all token operations produce `warning` severity, including token creation.

## Auth method operations
{: #auth-method-operations}

Auth method operations generate activity tracking events when you enable, configure, tune, or list authentication methods. These events help you to audit changes to your authentication mechanisms and helps to ensure that only authorized authentication engines are configured in your instance.

The following table lists the activity tracking events for authentication method configuration operations:

| Action pattern | Vault path | Vault operation | Severity |
|---|---|---|---|
| `secrets-manager.auth` | `sys/auth` | `read` | `normal` |
| `secrets-manager.auth.<method>` | `sys/auth/<method>` | `update` (enable or tune) | `warning` |
{: caption="Authentication method configuration events" caption-side="bottom"}
 
## AppRole auth operations
{: #approle-auth-operations}

AppRole authentication operations generate activity tracking events when you create, retrieve, or delete roles, or when you manage role IDs and secret IDs. Monitoring these events allows you to audit machine-to-machine authentication setup and helps ensure that application credentials are securely configured.

The following table lists the activity tracking events for AppRole authentication operations:

| Action pattern | Vault path | Vault operation | Severity |
|---|---|---|---|
| `secrets-manager.approle.role.<name>` | `auth/approle/role/<name>` | `create` | `warning` |
| `secrets-manager.approle.role.<name>` | `auth/approle/role/<name>` | `read` | `normal` |
| `secrets-manager.approle.role.<name>` | `auth/approle/role/<name>` | `delete` | `critical` |
| `secrets-manager.approle.role.<name>.role-id` | `auth/approle/role/<name>/role-id` | `read` | `normal` |
| `secrets-manager.approle.role.<name>.secret-id` | `auth/approle/role/<name>/secret-id` | `update` (generate) | `warning` |
| `secrets-manager.approle.role.<name>.secret-id` | `auth/approle/role/<name>/secret-id/` | `list` | `normal` |
{: caption="AppRole authentication operation events" caption-side="bottom"}
 
## Secret engine mount operations
{: #secret-engine-mount-operations}

Secret engine mount operations generate activity tracking events when you enable, configure, tune, or disable secret engine mounts. These events use dynamic action patterns that are constructed by appending the mount path name to the event action string.

The following table lists the activity tracking events for secrets engine mount operations:

| Action pattern | Vault path | Vault operation | Severity |
|---|---|---|---|
| `secrets-manager.mounts.<name>` | `sys/mounts/<name>` | `update` (enable or tune) | `warning` |
| `secrets-manager.mounts` | `sys/mounts` | `read` | `normal` |
| `secrets-manager.mounts.<name>` | `sys/mounts/<name>` | `delete` (disable) | `critical` |
{: caption="Secrets engine mount operation events" caption-side="bottom"}

## Key value version 2.0 secrets engine events
{: #kv-v2-secrets-engine-events}

These events are generated for every operation on a Key value version 2.0 secrets engine mount.

The following table lists the activity tracking events for KV v2 secrets engine operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| Write a new secret | `secrets-manager.data.<path>` | `warning` | `<mount>/data/<path>` |
| Read a secret | `secrets-manager.data.<path>` | `normal` | `<mount>/data/<path>` |
| Update a secret | `secrets-manager.data.<path>` | `warning` | `<mount>/data/<path>` |
| Delete a secret | `secrets-manager.data.<path>` | `critical` | `<mount>/data/<path>` |
| List secrets at a path | `secrets-manager.metadata.<path>` | `normal` | `<mount>/metadata/<path>/` |
| Read secret metadata | `secrets-manager.metadata.<path>` | `normal` | `<mount>/metadata/<path>` |
| Update secret metadata | `secrets-manager.metadata.<path>` | `warning` | `<mount>/metadata/<path>` |
| Delete secret and all its versions | `secrets-manager.metadata.<path>` | `critical` | `<mount>/metadata/<path>` |
| Read secret subkeys | `secrets-manager.subkeys.<path>` | `normal` | `<mount>/subkeys/<path>` |
| Soft-delete specific versions | `secrets-manager.delete.<path>` | `warning` | `<mount>/delete/<path>` |
| Restore soft-deleted versions | `secrets-manager.undelete.<path>` | `warning` | `<mount>/undelete/<path>` |
| Permanently destroy versions | `secrets-manager.destroy.<path>` | `warning` | `<mount>/destroy/<path>` |
| Read KV engine configuration | `secrets-manager.config` | `normal` | `<mount>/config` |
| Update KV engine configuration | `secrets-manager.config` | `warning` | `<mount>/config` |
{: caption="KV v2 secrets engine events" caption-side="bottom"}

### Examples
{: #kv-v2-full-examples}

The following examples show activity tracking events that are generated for Key value version 2.0 secrets engine operations.

1. Write a new secret at path `app/database/password` on a mount named `secrets`:

   ```json
   {
     "action": "secrets-manager.data.app.database.password",
     "severity": "warning",
     "outcome": "success",
     "target": {
       "name": "secrets/data/app/database/password",
       "typeURI": "secrets-manager/secrets/data/app/database/password"
     },
     "message": "Secrets Manager: create secrets/data/app/database/password success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

2. Read a secret at path `app/database/password` on a mount named `kv`:

   ```json
   {
     "action": "secrets-manager.data.app.database.password",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "kv/data/app/database/password",
       "typeURI": "secrets-manager/kv/data/app/database/password"
     },
     "message": "Secrets Manager: read kv/data/app/database/password success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

3. List secrets at path `app/` on a mount named `kv`:

   ```json
   {
     "action": "secrets-manager.metadata.app",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "kv/metadata/app/",
       "typeURI": "secrets-manager/kv/metadata/app/"
     },
     "message": "Secrets Manager: list kv/metadata/app/ success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

4. Read secret subkeys for `secret1` on a mount named `kv`:

   ```json
   {
     "action": "secrets-manager.subkeys.secret1",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "kv/subkeys/secret1",
       "typeURI": "secrets-manager/kv/subkeys/secret1"
     },
     "message": "Secrets Manager: read kv/subkeys/secret1 success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

5. Read metadata for `secret1` on a mount named `kv`:

   ```json
   {
     "action": "secrets-manager.metadata.secret1",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "kv/metadata/secret1",
       "typeURI": "secrets-manager/kv/metadata/secret1"
     },
     "message": "Secrets Manager: read kv/metadata/secret1 success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

6. Write a new secret `secret2` on a mount named `kv`:

   ```json
   {
     "action": "secrets-manager.data.secret2",
     "severity": "warning",
     "outcome": "success",
     "target": {
       "name": "kv/data/secret2",
       "typeURI": "secrets-manager/kv/data/secret2"
     },
     "message": "Secrets Manager: create kv/data/secret2 success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

7. Delete a secret at path `ns1-secret` on a mount named `secrets`:

   ```json
   {
     "action": "secrets-manager.data.ns1-secret",
     "severity": "critical",
     "outcome": "success",
     "target": {
       "name": "secrets/data/ns1-secret",
       "typeURI": "secrets-manager/secrets/data/ns1-secret"
     },
     "message": "Secrets Manager: delete secrets/data/ns1-secret success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

## PKI secrets engine events
{: #pki-secrets-engine-events}

These events are generated for every operation on a public-key infrastructure (PKI) secrets engine mount.

The following table lists the activity tracking events for PKI secrets engine operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| List certificates | `secrets-manager.certs` | `normal` | `<mount>/certs/` |
| Read a certificate | `secrets-manager.cert.<serial>` | `normal` | `<mount>/cert/<serial>` |
| List issuers | `secrets-manager.issuers` | `normal` | `<mount>/issuers/` |
| Read an issuer | `secrets-manager.issuer.<ref>` | `normal` | `<mount>/issuer/<ref>` |
| Update an issuer | `secrets-manager.issuer.<ref>` | `warning` | `<mount>/issuer/<ref>` |
| List roles | `secrets-manager.roles` | `normal` | `<mount>/roles/` |
| Create or update a role | `secrets-manager.roles.<name>` | `warning` | `<mount>/roles/<name>` |
| Read a role | `secrets-manager.roles.<name>` | `normal` | `<mount>/roles/<name>` |
| Delete a role | `secrets-manager.roles.<name>` | `critical` | `<mount>/roles/<name>` |
| Issue a certificate | `secrets-manager.issue.<role>` | `warning` | `<mount>/issue/<role>` |
| Sign a CSR | `secrets-manager.sign.<role>` | `warning` | `<mount>/sign/<role>` |
| Revoke a certificate | `secrets-manager.revoke` | `warning` | `<mount>/revoke` |
| Tidy expired certificates | `secrets-manager.tidy` | `warning` | `<mount>/tidy` |
| Read CA certificate | `secrets-manager.ca` | `normal` | `<mount>/ca` |
| Read CA chain | `secrets-manager.ca_chain` | `normal` | `<mount>/ca_chain` |
| Generate root CA | `secrets-manager.root.generate.<type>` | `warning` | `<mount>/root/generate/<type>` |
| Generate intermediate CA | `secrets-manager.intermediate.generate.<type>` | `warning` | `<mount>/intermediate/generate/<type>` |
| Set signed intermediate | `secrets-manager.intermediate.set-signed` | `warning` | `<mount>/intermediate/set-signed` |
| Read CRL configuration | `secrets-manager.config.crl` | `normal` | `<mount>/config/crl` |
| Update CRL configuration | `secrets-manager.config.crl` | `warning` | `<mount>/config/crl` |
| Update URL configuration | `secrets-manager.config.urls` | `warning` | `<mount>/config/urls` |
{: caption="PKI secrets engine events" caption-side="bottom"}

### Examples
{: #pki-full-examples}

The following examples show activity tracking events that are generated for PKI secrets engine operations.

1. List certificates on a mount named `pki`:

   ```json
   {
     "action": "secrets-manager.certs",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "pki/certs/",
       "typeURI": "secrets-manager/pki/certs/"
     },
     "message": "Secrets Manager: list pki/certs/ success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

2. List issuers on a mount named `pki`:

   ```json
   {
     "action": "secrets-manager.issuers",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "pki/issuers/",
       "typeURI": "secrets-manager/pki/issuers/"
     },
     "message": "Secrets Manager: list pki/issuers/ success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

3. List roles on a mount named `pki`:

   ```json
   {
     "action": "secrets-manager.roles",
     "severity": "normal",
     "outcome": "success",
     "target": {
       "name": "pki/roles/",
       "typeURI": "secrets-manager/pki/roles/"
     },
     "message": "Secrets Manager: list pki/roles/ success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

4. Issue a certificate by using role `web-server` on a mount named `pki`:

   ```json
   {
     "action": "secrets-manager.issue.web-server",
     "severity": "warning",
     "outcome": "success",
     "target": {
       "name": "pki/issue/web-server",
       "typeURI": "secrets-manager/pki/issue/web-server"
     },
     "message": "Secrets Manager: update pki/issue/web-server success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

## Token operations events
{: #token-operations-events}

These events are generated for operations on Vault tokens.

The following table lists the activity tracking events for token operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| Create a token | `secrets-manager.token.create` | `warning` | `auth/token/create` |
| Look up a token | `secrets-manager.token.lookup` | `warning` | `auth/token/lookup` |
| Renew a token | `secrets-manager.token.renew` | `warning` | `auth/token/renew` |
| Revoke a token | `secrets-manager.token.revoke` | `warning` | `auth/token/revoke` |
| Revoke a token by accessor | `secrets-manager.token.revoke-accessor` | `warning` | `auth/token/revoke-accessor` |
{: caption="Token operations events" caption-side="bottom"}

All token operations record as `update` in the Vault audit log, so all produce `warning` severity — including token creation.

### Example
{: #token-full-examples}

The following example shows the activity tracking events that are generated for token operations.

Create a token:

```json
{
  "action": "secrets-manager.token.create",
  "severity": "warning",
  "outcome": "success",
  "target": {
    "name": "auth/token/create",
    "typeURI": "secrets-manager/auth/token/create"
  },
  "message": "Secrets Manager: update auth/token/create success",
  "requestData": { "vaultNamespace": "my_namespace/" }
}
```
{: codeblock}

## AppRole auth method events
{: #approle-auth-method-events}

These events are generated for AppRole role and credential operations.

The following table lists the activity tracking events for AppRole auth method operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| List AppRole roles | `secrets-manager.approle.role` | `normal` | `auth/approle/role/` |
| Create an AppRole role | `secrets-manager.approle.role.<name>` | `warning` | `auth/approle/role/<name>` |
| Read an AppRole role | `secrets-manager.approle.role.<name>` | `normal` | `auth/approle/role/<name>` |
| Delete an AppRole role | `secrets-manager.approle.role.<name>` | `critical` | `auth/approle/role/<name>` |
| Read a Role ID | `secrets-manager.approle.role.<name>.role-id` | `normal` | `auth/approle/role/<name>/role-id` |
| Generate a Secret ID | `secrets-manager.approle.role.<name>.secret-id` | `warning` | `auth/approle/role/<name>/secret-id` |
| List Secret IDs | `secrets-manager.approle.role.<name>.secret-id` | `normal` | `auth/approle/role/<name>/secret-id/` |
{: caption="AppRole auth method events" caption-side="bottom"}

### Examples
{: #approle-full-examples}

The following examples show activity tracking events that are generated for AppRole auth method operations.

1. Create an AppRole role named `my-app`:

   ```json
   {
     "action": "secrets-manager.approle.role.my-app",
     "severity": "warning",
     "outcome": "success",
     "target": {
       "name": "auth/approle/role/my-app",
       "typeURI": "secrets-manager/auth/approle/role/my-app"
     },
     "message": "Secrets Manager: create auth/approle/role/my-app success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

2. Generate a Secret ID for role `my-app`:

   ```json
   {
     "action": "secrets-manager.approle.role.my-app.secret-id",
     "severity": "warning",
     "outcome": "success",
     "target": {
       "name": "auth/approle/role/my-app/secret-id",
       "typeURI": "secrets-manager/auth/approle/role/my-app/secret-id"
     },
     "message": "Secrets Manager: update auth/approle/role/my-app/secret-id success",
     "requestData": { "vaultNamespace": "my_namespace/" }
   }
   ```
   {: codeblock}

## Access control list policy events
{: #acl-policy-events}

These events are generated for operations on Vault access control list policies.

The following table lists the activity tracking events for access control list policy operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| List policies | `secrets-manager.policies.acl` | `normal` | `sys/policies/acl/` |
| Create or update a policy | `secrets-manager.policies.acl.<name>` | `warning` | `sys/policies/acl/<name>` |
| Read a policy | `secrets-manager.policies.acl.<name>` | `normal` | `sys/policies/acl/<name>` |
| Delete a policy | `secrets-manager.policies.acl.<name>` | `critical` | `sys/policies/acl/<name>` |
{: caption="ACL policy events" caption-side="bottom"}

### Example
{: #acl-full-examples}

The following example shows an activity tracking event that is generated for an access control list policy operation.

Create a policy named `app-policy`:

```json
{
  "action": "secrets-manager.policies.acl.app-policy",
  "severity": "warning",
  "outcome": "success",
  "target": {
    "name": "sys/policies/acl/app-policy",
    "typeURI": "secrets-manager/sys/policies/acl/app-policy"
  },
  "message": "Secrets Manager: update sys/policies/acl/app-policy success",
  "requestData": { "vaultNamespace": "my_namespace/" }
}
```
{: codeblock}

## Auth method management events
{: #auth-method-management-events}

These events are generated when you enable or disable authentication methods.

The following table lists the activity tracking events for auth method management operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| List enabled auth methods | `secrets-manager.auth` | `normal` | `sys/auth` |
| Enable an auth method | `secrets-manager.auth.<method>` | `warning` | `sys/auth/<method>` |
| Disable an auth method | `secrets-manager.auth.<method>` | `critical` | `sys/auth/<method>` |
{: caption="Auth method management events" caption-side="bottom"}

## Secret engine mount events
{: #secret-engine-mount-events}

These events are generated when you enable, tune, or disable secrets engines.

The following table lists the activity tracking events for secret engine mount operations:

| Operation | Action | Severity | target.name |
|---|---|---|---|
| List mounted engines | `secrets-manager.mounts` | `normal` | `sys/mounts` |
| Enable a secrets engine | `secrets-manager.mounts.<name>` | `warning` | `sys/mounts/<name>` |
| Tune a secrets engine | `secrets-manager.mounts.<name>` | `warning` | `sys/mounts/<name>` |
| Disable a secrets engine | `secrets-manager.mounts.<name>` | `critical` | `sys/mounts/<name>` |
{: caption="Secret engine mount events" caption-side="bottom"}

### Example
{: #mount-full-examples}

The following example shows an activity tracking event that is generated for a secret engine mount operation.

Enable a key value version 2.0 engine at mount path `my-kv`:

```json
{
  "action": "secrets-manager.mounts.my-kv",
  "severity": "warning",
  "outcome": "success",
  "target": {
    "name": "sys/mounts/my-kv",
    "typeURI": "secrets-manager/sys/mounts/my-kv"
  },
  "message": "Secrets Manager: update sys/mounts/my-kv success",
  "requestData": { "vaultNamespace": "my_namespace/" }
}
```
{: codeblock}

## System-generated events
{: #system-generated-events}

These events appear automatically in your logs as a side effect of Vault CLI and UI operations. They are not triggered directly by you.

The following table lists the system-generated events:

| Event | Action | Severity | Triggered by |
|---|---|---|---|
| Vault CLI mount resolution | `secrets-manager.internal.ui.mounts.secrets` | `normal` | Generated automatically on every KV CLI or UI operation |
{: caption="System-generated events" caption-side="bottom"}

You will see this event paired with every KV operation. It can safely be filtered out by using `-action:secrets-manager.internal.ui.mounts.secrets` in your IBM Cloud Logs queries.

## Instance management events
{: #instance-management-events}

These events are generated by IBM Cloud control-plane operations on your instance. The action values are fixed strings, not path-derived.

The following table lists the activity tracking events for instance management operations:

| Operation | Action | Severity |
|---|---|---|
| Read instance details | `secrets-manager.instance.read` | `normal` |
| Create a log destination | `secrets-manager.destination.create` | `warning` |
| List log destinations | `secrets-manager.destinations.list` | `normal` |
| Read a log destination | `secrets-manager.destination.read` | `normal` |
| Update a log destination | `secrets-manager.destination.update` | `warning` |
| Delete a log destination | `secrets-manager.destination.delete` | `critical` |
| Test a log destination | `secrets-manager.destination.test` | `warning` |
{: caption="Instance management events" caption-side="bottom"}

## Failure events
{: #failure-events}

Any operation that fails produces an event with `outcome: failure` and a populated `reason` block. The same action and severity rules apply.

Example — unauthorized read attempt:

```json
{
  "action": "secrets-manager.data.my-secret",
  "severity": "normal",
  "outcome": "failure",
  "reason": {
    "reasonCode": 401,
    "reasonType": "unauthorized",
    "reasonForFailure": "1 error occurred: * permission denied"
  },
  "target": {
    "name": "secrets/data/my-secret",
    "typeURI": "secrets-manager/secrets/data/my-secret"
  },
  "message": "Secrets Manager: read secrets/data/my-secret failure",
  "requestData": { "vaultNamespace": "my_namespace/" }
}
```
{: codeblock}

## Generic rules
{: #generic-rules}

All events you receive in your IBM Cloud Logs instance are governed by the following rules. Understanding these rules lets you predict the exact action and severity for any Vault operation — including operations on engines not listed in this page.

### Rule 1 — How the action field is constructed
{: #rule-1-action-field}

The action field is derived from the Vault request path, not a fixed string.

```
action = "secrets-manager." + <Vault request path with the mount name removed and "/" replaced by ".">
```
{: codeblock}

The mount name is always the first segment of the Vault path and is always dropped. Everything after it becomes the action suffix.

The following table shows examples of how the action field is constructed:

| Vault path called | Mount name | Action produced |
|---|---|---|
| `secrets/data/my-key` | `secrets` | `secrets-manager.data.my-key` |
| `my-kv/data/my-key` | `my-kv` | `secrets-manager.data.my-key` |
| `kv/metadata/` | `kv` | `secrets-manager.metadata` |
| `pki/certs/` | `pki` | `secrets-manager.certs` |
| `pki/issue/my-role` | `pki` | `secrets-manager.issue.my-role` |
| `sys/policies/acl/my-policy` | `sys` | `secrets-manager.policies.acl.my-policy` |
| `auth/approle/role/my-role` | `auth` | `secrets-manager.approle.role.my-role` |
{: caption="Action field construction examples" caption-side="bottom"}

Because the mount name is always dropped, two operations on different mounts with the same sub-path produce the same action value. Always use `target.name` to identify the exact mount and resource that was accessed.

For any secrets engine not listed in this page, apply this rule to the HashiCorp Vault API paths for that engine to determine the action values you will see.

### Rule 2 — How the severity field is determined
{: #rule-2-severity-field}

Severity is set by the Vault operation type, not the path or engine.

The following table lists the severity levels for each Vault operation type:

| Vault operation | Severity | Typical cases |
|---|---|---|
| `read` | `normal` | Reading a secret, reading a policy, reading a role |
| `list` | `normal` | Listing secrets, listing certificates, listing roles |
| `create` | `warning` | Creating a new secret, creating a role, creating an AppRole |
| `update` | `warning` | Updating a secret, renewing a token, issuing a certificate, generating a Secret ID |
| `delete` | `critical` | Deleting a secret, deleting a policy, disabling a mount |
{: caption="Severity field determination" caption-side="bottom"}

Vault records most `POST` requests as `update`. This means token creation, certificate issuance, and Secret ID generation all produce `warning` severity even though they create new resources.
{: note}
