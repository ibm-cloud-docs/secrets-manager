---

copyright:
  years: 2024
lastupdated: "2024-07-10"

keywords: pricing plan, billing, cost

subcollection: secrets-manager

---

# Pricing for {{site.data.keyword.secrets-manager_short}} on {{site.data.keyword.cloud_notm}}
{: #pricing}


Pricing in {{site.data.keyword.secrets-manager_full}} is based on the number of service instances in an account, and the number of active secrets in each of the service instances. Learn more about the Trial and Standard plans offered by {{site.data.keyword.secrets-manager_short}}.
{: shortdesc}

## Plans
{: #plans}

### Trial
{: #trial-plan}

The `Trial` plan provides unlimited access to all service capabilities for a limited time of 30 days.  
{: shortdesc}

When your Trial instance expires, you lose access to your secrets and integrations. To preserve your data, and prevent any disruptions in your workflow, you must upgrade to the Standard plan before your Trial plan expires. Follow these steps to [update your pricing plan](/docs/billing-usage?topic=billing-usage-changing&interface=ui). You can use the UI, API, and CLI to complete this process.

You can create only one Trial instance of Secrets Manager per account. Before you can create a new Trial instance, you must [delete the existing Trial instance and its reclamation](/docs/secrets-manager?topic=secrets-manager-mng-data#service-delete).
{: note}

### Standard
{: #standard-plan}

The `Standard` plan provides unlimited number of Secrets Manager instances per IBM Cloud account, and unlimited access to all service capabilities.
{: shortdesc}

A service instance is billed based on the day it was provisioned in the month.
{: note}

At the end of each billing period you're charged for the number of instances in your account, plus the maximum number of secrets with an active status in those instances. If you create more instances after the start of the monthly billing period, the cost of each additional instance for the first month is prorated based on the number of days that remain in the billing period.
