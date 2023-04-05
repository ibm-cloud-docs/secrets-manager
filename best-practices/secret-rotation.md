---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-05"

keywords: secrets management best practice, rotating secrets, secret rotation, locking secrets, automatic rotation

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

# Best practices for rotating and locking secrets
{: #best-practices-rotate-secrets}

With {{site.data.keyword.secrets-manager_full}}, you can design a strategy for rotating your secrets and sensitive data. Review the following suggested guidelines for implementing best practices around your secrets management.
{: shortdesc}


## Define your rotation strategy
{: #best-practices-rotation-strategy}

As you use {{site.data.keyword.secrets-manager_short}} to design your secrets management strategy, consider how often you want to rotate your secrets based on the internal guidelines for your organization. Determine ahead of time which users or service IDs require access to rotate secrets, and how those secrets can be rotated manually to avoid interruptions to your applications.

1. Determine a frequency of rotation for your secrets.

    After you store a secret in {{site.data.keyword.secrets-manager_short}}, you decide the frequency of its rotation. You might want to rotate secrets due to personnel turnover, process malfunction, or according to your organization's internal guidelines. Rotate your secrets regularly, for example every 90 days, to meet best practices around secrets management.

2. Test out rotation workflows for each type of secret that you manage in {{site.data.keyword.secrets-manager_short}}.

    The way in which {{site.data.keyword.secrets-manager_short}} evaluates a request to rotate a secret differs depending on the type of secret. For example, some secrets are replaced immediately with the data that you provide on rotation, whereas other secrets, such as public certificates, move into an extra validation step. For more information about how {{site.data.keyword.secrets-manager_short}} handles rotation requests, see [Manually rotating secrets](/docs/secrets-manager?topic=secrets-manager-manual-rotation#manual-rotate-by-type).

3. Establish a process for deploying the newest secret versions to your applications.

    Use an automated flow to obtain and deploy the latest version of your secret after it is rotated. For more information, see [Avoid application outages by locking your secrets](#best-practices-lock-secrets).

## Set up alerts for expiring secrets
{: #best-practices-expiry-notifications}

Connect to the {{site.data.keyword.en_short}} service so that {{site.data.keyword.secrets-manager_short}} can notify you in advance when your secrets or certificates are about to expire.

1. Set up alerts for your instance by enabling event notifications. To connect your instance to the {{site.data.keyword.en_short}} service, go to the **{{site.data.keyword.secrets-manager_short}} UI > Settings > Event Notifications**.
2. Create topics and subscriptions in {{site.data.keyword.en_short}} so that alerts can be forwarded and delivered to your [selected destinations](/docs/secrets-manager?topic=secrets-manager-event-notifications#event-notifications-destinations), for example Slack or email. 

    Looking for examples? Check out our [tutorial series](/docs/secrets-manager?topic=secrets-manager-tutorial-expiring-secrets-part-1) that guides you through sending notifications to GitHub and Slack.
    {: tip}

## Enable automatic rotation for secrets
{: #best-practices-automati-rotation}

Simplify the process of rotating secrets in your instance by enabling automatic rotation.

1. Use automatic rotation to limit how long your secrets remain active. 

    By scheduling automatic rotation of secrets at regular intervals, you reduce the likelihood of compromised credentials. When it's time to rotate the secret based on the rotation interval that you specify, {{site.data.keyword.secrets-manager_short}} automatically creates a new version of your secret. For more information, see [Automatically rotating your secrets](/docs/secrets-manager?topic=secrets-manager-automatic-rotation).

2. Schedule automatic rotation to take place before your secrets are set to expire.

   To avoid interruptions to your applications, it's recommended that you set the minimum interval between automatic rotation and expiration date to 30 days.

    

## Avoid application outages by locking your secrets
{: #best-practices-lock-secrets}

Use locks to plan for periodic rotation of secrets. When you create and remove locks on a secret as part of an automated workflow, you can safely delete secrets only after their newest versions are fully deployed to your applications.

1. Create locks on a secret to represent the applications or services that use it. To create a lock from the {{site.data.keyword.secrets-manager_short}} UI, go to **Secrets** page and click the **Actions** menu ![Actions icon](../../icons/actions-icon-vertical.svg) **> Locks**. 

    You can think of a lock as a kind of mapping that you can create between a secret and the client or application that consumes it. If a secret has a lock attached to it, it means that shouldn't be modified because it's currently in use by your application.
    
    A secret can be unlocked and able to be deleted or modified only after all of its associated locks are removed.
    {: note}

2. Build an automated flow that can help you to safely delete old or expired secrets from your instance only after your applications have picked up the latest secret versions. For example, consider the following scenario:

    You create a secret that expires in 89 days and is scheduled to be automatically rotated in 90 days.

    | | Description of action |
    | --- | --- |
    | 1 | Run an pipeline that obtains the secret from {{site.data.keyword.secrets-manager_short}} and deploys it to a Kubernetes cluster so that it can be used by your applications. |
    | 2 | Create locks on the current version of the secret (one lock to for each application or service that uses the secret). |
    | 3 | After 90 days, receive a notification from {{site.data.keyword.en_short}} that your secret was rotated. In response to the notification, run a pipeline to get the latest secret version and deploy it to your cluster. |
    | 4 | Run another pipeline that 1) verifies that the new secret version was picked up by the service pods in your cluster, and 2) removes both locks on the previous version of the secret in {{site.data.keyword.secrets-manager_short}}.
    | 5 | After two days, the previous version of the secret expires and is safely deleted from your instance. The next periodic rotation cycle repeats steps 2 - 4. |
    {: caption="Table 1. Example automated workflow for locking secrets." caption-side="bottom"}




