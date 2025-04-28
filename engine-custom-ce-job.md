---

copyright:
  years: 2025
lastupdated: "2025-04-28"

keywords: Secrets Manager custom credentials, Secrets Manager third-party

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Creating a {{site.data.keyword.codeengineshort}} job
{: #engine-custom-ce-job}

The custom credentials secret type allows {{site.data.keyword.secrets-manager_short}} users to manage the lifecycle of credentials for external systems (for example, Artifactory, PagerDuty) using {{site.data.keyword.secrets-manager_short}} APIs and integrations. To create custom credentials secrets, you must first set up an [ {{site.data.keyword.codeenginefull}} project](/docs/codeengine?topic=codeengine-manage-project#create-a-project) and [define a {{site.data.keyword.codeengineshort}} job](/docs/codeengine?topic=codeengine-job-plan) to interface between {{site.data.keyword.secrets-manager_short}} and the external service requiring credentials.
{: shortdesc}

Custom credentials support a wide range of third-party systems, so guidance for configuring your {{site.data.keyword.codeengineshort}} job is intentionally general, focusing on {{site.data.keyword.codeengineshort}} best practices. If your parameters are correctly formatted and you select a valid {{site.data.keyword.codeengineshort}} project and job, {{site.data.keyword.secrets-manager_short}} can run the job with various configurations. However, {{site.data.keyword.secrets-manager_short}} does not validate whether your configuration aligns with your specific use case. It only runs the provided setup.

To assist with implementing job logic for interacting with external providers, {{site.data.keyword.secrets-manager_short}} offers developer documentation, best practices, job generator and deployer tools, and code examples. [Learn how to design](https://github.com/IBM/secrets-manager-custom-credentials-providers/tree/main?tab=readme-ov-file#designing-a-new-credentials-provider-job){: external} a new credentials provider job in the Custom Credentials Providers repository.
{: note}

## Credentials provider job lifecycle
{: #credentials-provider-job-flow}

A typical job flow involves:

1. Creating the credentials, where new credentials are generated.
2. Deleting the credentials, where credentials are revoked.
3. Updating the secret task, which sends the result back to {{site.data.keyword.secrets-manager_short}}.

The following {{site.data.keyword.codeengineshort}} job flow diagram illustrates this lifecycle.

![The diagram shows the lifecycle of a {{site.data.keyword.codeengineshort}} credentials provider job.](/images/custom-credentials-ce-job-lifecycle-diagram.drawio.svg){: caption="{{site.data.keyword.codeengineshort}} job lifecycle" caption-side="bottom"}

{{site.data.keyword.secrets-manager_short}} retries daily (for up to 10 days) to delete credentials after a failed deletion attempt. Request retries are recommended when applicable but omitted in the flow diagram for clarity. A `400` or `404` status response to the **Secret task update** request indicates that the secret task cannot be updated by a retry.
{: note}


## Security considerations
{: #credentials-provider-security-considerations}

To enhance security:

* **Follow the principle of least privilege**: restrict job access to only the necessary secrets by using a dedicated secret group.
* **IAM roles**: assign the API key used by the job to authenticate with {{site.data.keyword.secrets-manager_short}} the **SecretTaskUpdater** role. If the job needs to read secrets, also assign the **SecretsReader** role. Both roles should be scoped to the dedicated secret group.
* **Avoid personally identifiable information (PII) and confidential data**: do not use personal identifiers or confidential data (such as email addresses or social security numbers) as input parameters or as credential IDs. {{site.data.keyword.secrets-manager_short}} treats the input parameters and credential ID as metadata, not as sensitive secret data.

## Error handling
{: #credentials-provider-error-handling}

If the credentials provider job is unable to create or delete the credentials, it should update {{site.data.keyword.secrets-manager_short}} with an application error code and a user facing error message. This error is displayed in the {{site.data.keyword.secrets-manager_short}} console for the failed task and allow the user to apply a corrective action.

## Next steps
{: #custom-credentials-ce-job-next-steps}

After you have created your {{site.data.keyword.codeengineshort}} project and job, you can now move on to [creating a custom credentials engine configuration](/docs/secrets-manager?topic=secrets-manager-custom-credentials-config).
