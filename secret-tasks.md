---

copyright:
  years: 2025
lastupdated: "2025-09-08"

keywords: Secrets Manager custom credentials, secret tasks

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Secret tasks
{: #secret-tasks}

{{site.data.keyword.secrets-manager_full}} uses secret tasks to trigger and manage the life-cycle of asynchronous secret types. An asynchronous secret type is created in an initial `pre-activation` state, and a new execution task is automatically created to track the external creation of the secret credentials. Once the external process is completed it updates the secret task with the new credentials and {{site.data.keyword.secrets-manager_short}} updates this secret state to become `active`. You can list secret tasks, get their details and delete them.
{: shortdesc}

The Custom credentials secret type uses secret tasks to trigger the asynchronous execution of credentials providers implemented as  {{site.data.keyword.codeenginefull}} jobs. Each task is mapped to a job run with the task ID set as the job run name.

Tasks have different **statuses**, **types**, and record of **values** associated with them.

| Status | Description |
| ----- | :----- |
| `queued` |	The task has been created and is waiting in the queue to start processing. Queued tasks may be cancelled by deleting them. |
| `processing` |	The task is now running. In case of a custom credentials secret type a {{site.data.keyword.codeengineshort}} job run has started and the task is waiting for the job to return or for the timeout to elapse. Processing tasks may be cancelled by deleting them. |
| `credentials_created`	| The creation task has been successfully completed. In case of a custom credentials secret type the {{site.data.keyword.codeengineshort}} job reporting success to {{site.data.keyword.secrets-manager_short}}. Tasks with this status do not accept further updates. |
| `credentials_deleted`	| The deletion task has been successfully completed. In case of a custom credentials secret type the {{site.data.keyword.codeengineshort}} job. Tasks with this status do not accept further updates. |
| `failed` | The task timeout elapsed or the credentials provider updated the task with an error. In case of a custom credentials secret type the {{site.data.keyword.codeengineshort}} job has returned to Secrets Manager with an error. Tasks with this status do not accept further updates. |
{: caption="Available task statuses" caption-side="top"}
{: #task-statuses-table1}
{: tab-title="Task statuses"}
{: tab-group="secrets-manager"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the platform role name and the column headers identify the specific information available about each role."}
{: class="simple-tab-table"}
{: row-headers}

| Status | Description |
| ----- | :----- |
| `create_credentials` | This type represents a task that creates credentials. Can be triggered because of secret creation or a rotation. |
| `delete_credentials` | This type represents a task that deletes previously created credentials. It can be triggered because of deleting the secret, the expiration or deletion a specific version, or by rotation. | 
{: caption="Available task types" caption-side="top"}
{: #task-types-table1}
{: tab-title="Task types"}
{: tab-group="secrets-manager"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the platform role name and the column headers identify the specific information available about each role."}
{: class="simple-tab-table"}
{: row-headers}

| Status | Description |
| ----- | :----- |
| `secret_creation`	| This task was triggered because of the creation of the secret. |
| `manual_secret_rotation` |	This task was triggered because of a manual rotation. |
| `automatic_secret_rotation` |	This task was triggered because of an automatic (periodic) rotation. |
| `secret_version_expiration`	| This task was triggered because of an expiration of a secret version. |
| `secret_version_data_deletion` |	This task was triggered because of deleting a secret. |
{: caption="Available task record values" caption-side="top"}
{: #task-record-values-table1}
{: tab-title="Task record values"}
{: tab-group="secrets-manager"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the platform role name and the column headers identify the specific information available about each role."}
{: class="simple-tab-table"}
{: row-headers}

## Working with secret tasks from the UI
{: #custom-credentials-tasks-ui}
{: ui}

For a look at the tasks associated with your custom credentials secret, you can access the **Tasks** menu inside the **Action** drop down. Here you can see a record of the actions that are associated with your credentials. For example, when a credential was created or deleted, or whether a timeout was associated with a task.

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
2. To view the secret tasks, click **Tasks**.

To delete a secret task:

1. In the **Secrets** table, click the **Actions** menu ![Actions icon](../icons/actions-icon-vertical.svg) to open a list of options for your secret.
2. Click **Tasks** to view all tasks.
3. Tick the checkbox for a task to delete and click  **Delete selected**.

## Working with secret tasks from CLI
{: #custom-credentials-tasks-cli}
{: cli}

Before you begin, [follow the CLI docs](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli) to set your API endpoint.

To look at the tasks associated with your custom credentials secret by using the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-secrets-manager-cli#secrets-manager-cli-secret-create-command) command:

```sh 
ibmcloud secrets-manager secret-tasks --id SECRET_ID
```
{: pre}

To get the details of a specific task, use the task ID:

```sh
ibmcloud secrets-manager secret-task --secret-id SECRET-ID --id TASK_ID
```
{: pre}

To delete a task:

```sh
ibmcloud secrets-manager secret-task-delete --secret-id SECRET-ID --id TASK_ID
```
{: pre}


## Working secret tasks using the API
{: #custom-credentials-tasks-api}
{: api}

You can look at your custom credentials secret tasks programmatically by calling the {{site.data.keyword.secrets-manager_short}} API. When you call the API, replace the SECRET_ID variables and IAM token with the values that are specific to your {{site.data.keyword.secrets-manager_short}} instance:

```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/SECRET_ID/tasks" 
```
{: codeblock}
{: curl}

To get the details of a specific task, use the task ID:

```sh
curl -X POST 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/SECRET_ID/tasks/TASK_ID" 
```
{: codeblock}
{: curl}

To delete a task:

```sh
curl -X PUT 
    -H "Authorization: Bearer {IAM_token}" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
  "https://{instance_ID}.{region}.secrets-manager.appdomain.cloud/api/v2/secrets/SECRET_ID/tasks/TASK_ID" 
```
{: codeblock}
{: curl}
