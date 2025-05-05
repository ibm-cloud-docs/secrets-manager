---

copyright:
  years: 2025
lastupdated: "2025-04-28"

keywords: Secrets Manager custom credentials, Secrets Manager third-party

subcollection: secrets-manager

---

{{site.data.keyword.attribute-definition-list}}

# Preparing to create custom credentials
{: #custom-credentials-prepare}

You can enable your {{site.data.keyword.secrets-manager_short}} service instance to create custom credentials secrets by adding a custom credentials engine configuration.

In {{site.data.keyword.secrets-manager_short}}, the custom credentials engine configuration acts as the back end for the `custom_credentials` secret type. Custom credentials can represent various types of secrets, implemented by using a credentials provider.

A credentials provider is an  {{site.data.keyword.codeenginefull}} job that implements a {{site.data.keyword.secrets-manager_short}} interface. It is typically designed to create and delete credentials for a third-party system.

This model enables cloud developers to extend {{site.data.keyword.secrets-manager_short}} by supporting additional secret types. Developers can then create and manage these secrets alongside the built-in secret types, can ensure a uniform lifecycle management experience.

## Before you begin
{: #custom-credentials-prepare-before-begin}

It is recommended to first explore the [{{site.data.keyword.codeengineshort}} documentation](/docs/codeengine?topic=codeengine-getting-started) and decide on aspects such as: implementation language, and serverless runtime environment configuration possibilities.

Make sure to review [security-related pre-requisites](/docs/secrets-manager?topic=secrets-manager-custom-credentials-config&interface=ui#custom-credentials-config-before-begin) and [other security considerations](/docs/secrets-manager?topic=secrets-manager-engine-custom-ce-job&interface=ui#credentials-provider-security-considerations).

## Next steps
{: #custom-credentials-prepare-next-steps}

After you have completed your prerequisites, you can start [creating a {{site.data.keyword.codeengineshort}} project and job](/docs/secrets-manager?topic=secrets-manager-engine-custom-ce-job)
