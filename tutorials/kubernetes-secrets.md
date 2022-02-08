---


copyright:
  years: 2022
lastupdated: "2022-01-04"

keywords: tutorial, Secrets Manager

subcollection: secrets-manager
content-type: tutorial
services: secrets-manager,containers,Registry
account-plan: lite
completion-time: 45m

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


# Secure secrets for apps that run in your Kubernetes cluster
{: #tutorial-kubernetes-secrets}
{: toc-content-type="tutorial"}
{: toc-services="secrets-manager,containers,Registry"}
{: toc-completion-time="45m"}


In this tutorial, you learn how to use {{site.data.keyword.secrets-manager_full}} to manage secrets for applications that run your {{site.data.keyword.containerfull_notm}} cluster.
{: shortdesc}

You're a developer for a large organization, and your team is using {{site.data.keyword.containershort}} to deploy containerized apps and services on {{site.data.keyword.cloud_notm}}. In your current flow, you use [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/){: external} to store the sensitive data, such as passwords and API keys, that are used by the apps and services that run in your cluster. To have more control over your application secrets, you want the ability to store your cluster secrets in an external secrets management service, where you can [encrypt them at rest](/docs/secrets-manager?topic=secrets-manager-mng-data), [monitor their activity](/docs/secrets-manager?topic=secrets-manager-at-events), and easily manage them.

With {{site.data.keyword.secrets-manager_short}}, you can centralize and secure the secrets that are used by the apps that run in your Kubernetes clusters. Rather than injecting your secrets at deployment time, you can configure your apps to securely retrieve secrets from {{site.data.keyword.secrets-manager_short}} at run time. When it's time to rotate the secret, you can do so from {{site.data.keyword.secrets-manager_short}}. For example, consider the following scenario:


![The diagram shows the basic flow between Secrets Manager and your Kubernetes cluster.](../images/iks-external-secrets-flow.svg){: caption="Figure 1. Kubernetes External Secrets flow" caption-side="bottom"}


1. As a developer, you use {{site.data.keyword.secrets-manager_short}} to store a secret for an application that you want to deploy in a Kubernetes cluster.
2. {{site.data.keyword.secrets-manager_short}} provides an ID for the secret. You include the ID in the `ExternalSecrets` configuration file for your app and you apply the configuration to the cluster.
3. The External Secrets controller fetches the `ExternalSecrets` objects in the configuration file that you defined by using the Kubernetes API. 
4. At application run time, the controller retrieves the secret data from {{site.data.keyword.secrets-manager_short}}, and converts the `ExternalSecrets` objects to Kubernetes secrets for your cluster.

This scenario features a third-party tool that can impact the compliance readiness of workloads that run in your Kubernetes cluster. If you add a community or third-party tool, keep in mind that you are responsible for maintaining the compliance of your apps and working with the appropriate provider to troubleshoot any issues. For more information, see [Your responsibilities with using {{site.data.keyword.containerfull_notm}}](/docs/containers?topic=containers-responsibilities_iks).
{: note}

## Before you begin
{: #tutorial-kubernetes-secrets-prereqs}

Before you get started, be sure that you have [**Administrator** platform access](/docs/account?topic=account-assign-access-resources#assign-new-access) so that you can create account credentials and provision resources. You also need the following prerequisites:

- [Download and install the IBM Cloud CLI](https://cloud.ibm.com/docs/cli).
- [Install the {{site.data.keyword.secrets-manager_short}} CLI plug-in](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli).
- [Install the Kubernetes CLI (`kubectl`)](https://kubernetes.io/docs/tasks/tools/){: external}.
- [Download and install jq](https://stedolan.github.io/jq/){: external}.

    `jq` helps you slice and filter JSON data. You use `jq` in this tutorial to grab and use stored environment variables.



## Set up your environment
{: #tutorial-kubernetes-secrets-set-up-env}
{: step}

To work with {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.containershort}}, you need to create a cluster and a {{site.data.keyword.secrets-manager_short}} instance in your {{site.data.keyword.cloud_notm}} account. You also need to configure permissions so that you can run operations against both services.

In this step, you set up an access environment by creating a service ID and an {{site.data.keyword.cloud_notm}} API key. At the end of the tutorial, you can easily remove your resources if you no longer need them.


### Create a service ID and API key
{: #tutorial-external-kubernetes-secrets-access}

Start by creating the account credentials that you need to be able to run operations against {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.containershort}}.

1. From the command line, log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
    {: note}

2. Create a service ID and set it as an environment variable.

    ```sh
    export SERVICE_ID=`ibmcloud iam service-id-create kubernetes-secrets-tutorial --description "A service ID for testing Secrets Manager and Kubernetes Service." --output json | jq -r ".id"`; echo $SERVICE_ID
    ```
    {: pre}

3. Assign the service ID permissions to read secrets from {{site.data.keyword.secrets-manager_short}}.

    ```sh
    ibmcloud iam service-policy-create $SERVICE_ID --roles "SecretsReader" --service-name secrets-manager
    ```
    {: pre}

    By assigning **SecretsReader** service access, the External Secrets controller has the correct level of access to read secrets from {{site.data.keyword.secrets-manager_short}} and populate them in a Kubernetes cluster.


4. Create an {{site.data.keyword.cloud_notm}} API key for your service ID.

    ```sh
    export IBM_CLOUD_API_KEY=`ibmcloud iam service-api-key-create kubernetes-secrets-tutorial $SERVICE_ID --description "An API key for testing Secrets Manager." --output json | jq -r ".apikey"`
    ```
    {: pre}

    You use this API key later to configure {{site.data.keyword.secrets-manager_short}} for your cluster deployment.


### Create a Kubernetes cluster and {{site.data.keyword.secrets-manager_short}} instance
{: #tutorial-kubernetes-secrets-prepare-cluster}

Next, create a test Kubernetes cluster and an instance of {{site.data.keyword.secrets-manager_short}} in your {{site.data.keyword.cloud_notm}} account.

You can create one free Kubernetes cluster and {{site.data.keyword.secrets-manager_short}} service instance per {{site.data.keyword.cloud_notm}} account. If you already have both resources in your account, you can use your existing free cluster and {{site.data.keyword.secrets-manager_short}} instance to complete the tutorial.
{: note}

1. From the command line, select the account, region, and resource group where you want to create a {{site.data.keyword.secrets-manager_short}} service instance.

    In this tutorial, you interact with the Dallas region. If you're logged in to a different region, be sure to set Dallas as your target region by running the following command.

    ```sh
    ibmcloud target -r us-south -g default
    ```
    {: pre}

2. Create a Kubernetes cluster.

    ```sh
    ibmcloud ks cluster create classic --zone dal10 --flavor free --name my-test-cluster
    ```
    {: pre}

3. Create a {{site.data.keyword.secrets-manager_short}} instance.

    ```sh
    ibmcloud resource service-instance-create my-secrets-manager secrets-manager lite us-south
    ```
    {: pre}

    Provisioning for both {{site.data.keyword.secrets-manager_short}} and your Kubernetes cluster takes 5 - 8 minutes to complete.

4. Set up a private image repository in IBM Cloud Container Registry.

    A private image repository in IBM Cloud is identified by a namespace. The namespace is used to create a unique URL to your image repository that developers can use to access private Docker images.

    ```sh
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

    Replace `<namespace>` with a namespace of your choice that is unrelated to this tutorial. For example, `_test_namespace_`.

5. Before you continue to the next step, verify that your cluster and {{site.data.keyword.secrets-manager_short}} instance provisioned successfully.

    1. Verify that the deployment of your worker node is complete.

        ```sh
        ibmcloud ks worker ls --cluster my-test-cluster
        ```
        {: pre}

        When your worker node is finished provisioning, the status changes to **Ready**.

        ```sh
        ID                                                       Public IP       Private IP      Flavor   State          Status                Zone    Version
        kube-c39pf4ld0m87o3fv1utg-mytestclust-default-000000dd   169.xx.xx.xxx   10.xxx.xx.xxx   free     normal   Ready   mex01   1.20.7_1543
        ```
        {: screen}

    2. Next, verify that your {{site.data.keyword.secrets-manager_short}} instance provisioned successfully.

        ```sh
        ibmcloud resource service-instance my-secrets-manager
        ```
        {: pre}

        When the instance is finished provisioning, the state changes to **Active**.

        ```plaintext
        Name:                  my-secrets-manager
        ID:                    crn:v1:bluemix:public:secrets-manager:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:fe06948b-0c6b-4183-8d4b-e6c1d38ff65f::
        GUID:                  fe06948b-0c6b-4183-8d4b-e6c1d38ff65f
        Location:              us-south
        Service Name:          secrets-manager
        Service Plan Name:     lite
        Resource Group Name:   default
        State:                 active
        Type:                  service_instance
        Sub Type:
        Created at:            2021-01-06T17:11:32Z
        Created by:            zara@example.com
        Updated at:            2021-03-31T02:33:26Z
        ```
        {: screen}

6. Set the context for your Kubernetes cluster in the CLI.

    ```sh
    ibmcloud ks cluster config --cluster my-test-cluster
    ```
    {: pre}

7. Verify that `kubectl` commands run properly and that the Kubernetes context is set to your cluster.

    ```sh
    kubectl config current-context
    ```
    {: pre}

    Example output:

    ```sh
    my-test-cluster/<your_cluster_ID>
    ```
    {: screen}


### Prepare your {{site.data.keyword.secrets-manager_short}} instance
{: #tutorial-kubernetes-secrets-prepare-sm}

Finally, configure your {{site.data.keyword.secrets-manager_short}} instance to start working with secrets.

1. From the command line, verify that you can access the {{site.data.keyword.secrets-manager_short}} CLI plug-in.

    ```sh
    ibmcloud secrets-manager --help
    ```
    {: pre}

    Don't have the plug-in installed? To install the {{site.data.keyword.secrets-manager_short}} CLI plug-in, run `ibmcloud plugin install secrets-manager`.
    {: tip}

2. Export an environment variable with your unique {{site.data.keyword.secrets-manager_short}} API endpoint URL.

    ```sh
    export SECRETS_MANAGER_URL=`ibmcloud resource service-instance my-secrets-manager --output json | jq -r '.[].dashboard_url | .[0:-3]'`; echo $SECRETS_MANAGER_URL
    ```
    {: pre}

3. Create a secret group for your instance.

    [Secret groups](/docs/secrets-manager?topic=secrets-manager-secret-groups) are a way to organize and control who on your team has access to specific secrets in your instance. To create a secret group from the {{site.data.keyword.cloud_notm}} CLI, you use the [**`ibmcloud secrets-manager secret-group-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-group-create-command) command. Run the following command to create a secret group and store its ID as an environment variable.

    ```sh
    export SECRET_GROUP_ID=`ibmcloud secrets-manager secret-group-create --resources '[{"name":"my-test-secret-group","description":"Read and write to my test app."}]' --output json | jq -r ".resources[].id"`; echo $SECRET_GROUP_ID
    ```
    {: pre}

    Using a Windows™ command prompt (`cmd.exe`) or PowerShell? If you encounter errors with passing JSON content on the command line, you might need to adjust the strings for quotation-escaping requirements that are specific to your operating system. For more information, see [Using quotation marks with strings in the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-quote-strings).
    {: tip}

    Success! Now you can store the secret in {{site.data.keyword.secrets-manager_short}} that you want to populate in your Kubernetes cluster. Continue to the next step.


## Create a secret in {{site.data.keyword.secrets-manager_short}}
{: #tutorial-kubernetes-secrets-create-secret}
{: step}

Secrets are application-specific and can vary based on the individual app or service that requires them. A secret might consist of a username, password, API key, or any other type of credential.

{{site.data.keyword.secrets-manager_short}} supports various [types of secrets](/docs/secrets-manager?topic=secrets-manager-what-is-secret#secret-types) that you can create and manage in the service. For example, if you need to manage an API key for an app that is protected by {{site.data.keyword.cloud_notm}} IAM authentication, you can create an [IAM credential](/docs/secrets-manager?topic=secrets-manager-iam-credentials). Or, if you need to manage a secret that can hold any type of structured or unstructured data, you can create an [arbitrary secret](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets).

In this tutorial, you create a username and password as an example. To create a secret from the {{site.data.keyword.cloud_notm}} CLI, you use the [**`ibmcloud secrets-manager secret-create`**](/docs/secrets-manager?topic=secrets-manager-cli-plugin-secrets-manager-cli#secrets-manager-cli-secret-create-command) command. Run the following command to create the secret and store its ID as an environment variable.

```sh
export SECRET_ID=`ibmcloud secrets-manager secret-create --secret-type username_password  --resources '[{"name":"example_username_password","description":"Extended description for my secret.","secret_group_id":"'"$SECRET_GROUP_ID"'","username":"user123","password":"cloudy-rainy-coffee-book","labels":["my-test-cluster","tutorial"]}]' --output json | jq -r ".resources[].id"`; echo $SECRET_ID
```
{: pre}

The output shows the ID of your newly created secret. For example:

```plaintext
e0246cea-d668-aba7-eef2-58ca11ad3707
```
{: screen}


## Set up Kubernetes External Secrets
{: #tutorial-kubernetes-secrets-configure-external}
{: step}

Now that you have a secret for your application, you can set up [Kubernetes External Secrets](https://github.com/external-secrets/kubernetes-external-secrets){: external} for your cluster. This package configures the connection between {{site.data.keyword.secrets-manager_short}} and your cluster by creating `ExternalSecrets` objects that are converted to Kubernetes secrets for your application.

Kubernetes External Secrets is an open source tool that is not maintained by IBM. For more information about this tool or to troubleshoot any issues, refer to the project in [GitHub](https://github.com/external-secrets/kubernetes-external-secrets){: external}.
{: note}

### Configure Kubernetes External Secrets for your cluster
{: #tutorial-kubernetes-secrets-configure-app}

First, add `kubernetes-external-secrets` resources to your cluster by installing the official Helm chart. For more installation options, check out the [`README.md`](https://github.com/external-secrets/kubernetes-external-secrets){: external}.

1. From your command line, use the service ID API key that you created in step 1 to define `secret-api-key`.

    ```sh
    kubectl -n default create secret generic secret-api-key --from-literal=apikey=$IBM_CLOUD_API_KEY
    ```
    {: pre}

2. Run the following commands to install Kubernetes External Secrets.

    ```sh
    helm repo add external-secrets https://external-secrets.github.io/kubernetes-external-secrets/
    ```
    {: pre}

    ```sh
    helm install secrets-manager-tutorial external-secrets/kubernetes-external-secrets
    ```
    {: pre}

3. In the console, click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Kubernetes > Clusters**.
4. Click _my-test-cluster_ **> Kubernetes dashboard > Deployments**.
5. In the table row for _secrets-manager-tutorial-kubernetes-external-secrets_, click the **Actions** menu ![Actions icon](../../icons/actions-icon-vertical.svg) **> Edit**.
6. In the JSON editor, scroll to find the `spec.containers` object that contains information about `kubernetes-external-secrets`.
7. Set the following environment variables. 

    ```yaml
    env:
      - name: IBM_CLOUD_SECRETS_MANAGER_API_APIKEY
        valueFrom:
          secretKeyRef:
            name: secret-api-key
            key: apikey
      - name: IBM_CLOUD_SECRETS_MANAGER_API_AUTH_TYPE
         value: iam
      - name: IBM_CLOUD_SECRETS_MANAGER_API_ENDPOINT
         value: <endpoint_url>
    ```
    {: codeblock}

    Replace `<endpoint_url>` with the {{site.data.keyword.secrets-manager_short}} endpoint URL that you retrieved in [step 1](#tutorial-kubernetes-secrets-prepare-sm).
8. Click **Update** to save your changes.


### Update your app configuration 
{: #tutorial-kubernetes-secrets-update-deployment}

After you install External Kubernetes Secrets in your cluster, you can define {{site.data.keyword.secrets-manager_short}} as the secrets backend for your application. Start by creating a configuration file that targets the secret in {{site.data.keyword.secrets-manager_short}} that you want to use.

1. In the root directory of your application, create an `external-secrets-example.yml` file.

    ```sh
    touch external-secrets-example.yml
    ```
    {: pre}

2. Modify the file to include information about the secret that you want to fetch from your {{site.data.keyword.secrets-manager_short}} instance.

    ```yaml
    apiVersion: kubernetes-client.io/v1
    kind: ExternalSecret
    metadata:
      name: ibmcloud-secrets-manager-example
    spec:
      backendType: ibmcloudSecretsManager
      data:
        - key: <SECRET_ID>
          name: username
          property: username 
          secretType: username_password
        - key: <SECRET_ID>
          name: password
          property: password
          secretType: username_password
    ```
    {: codeblock}

    Replace `<SECRET_ID>` with the unique ID of the secret that you created in the previous step.

3. Apply the configuration to your cluster.

    ```sh
    kubectl apply -f external-secrets-example.yml
    ```
    {: pre}

4. Verify that Kubernetes External Secrets controller is able to fetch the secret that is stored in your {{site.data.keyword.secrets-manager_short}} instance.

    ```sh
    kubectl get secret ibmcloud-secrets-manager-example -o json | jq '.data | map_values(@base64d)'
    ```
    {: pre}

    Example output:

    ```json
    {
        "password": "cloudy-rainy-coffee-book",
        "username": "user123"
    }
    ```
    {: screen}

    Success! You're now able to fetch the secret data that is stored in your {{site.data.keyword.secrets-manager_short}} instance. Continue to the next step.

## Deploy an app to the cluster
{: #tutorial-kubernetes-secrets-deploy-app}
{: step}


Finally, you can deploy an application in your cluster that uses the {{site.data.keyword.secrets-manager_short}} secret that you defined in the `external-secret-example.yml` file. At application run time, the secret data that is fetched from {{site.data.keyword.secrets-manager_short}} is converted to a Kubernetes secret that can be used by your cluster.

Looking for examples on how to deploy an app? Check out the [Kubernetes Service tutorial](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial_lesson3) to find out more about deploying a single instance of an app.



## (Optional) Clean up resources
{: #tutorial-kubernetes-secrets-clean-up}
{: step}

If you no longer need the resources that you created in this tutorial, you can complete the following steps to remove them from your account.

1. Delete your test Kubernetes cluster.

    ```sh
    ibmcloud ks cluster rm --cluster my-test-cluster
    ```
    {: pre}

2. Delete your test {{site.data.keyword.secrets-manager_short}} instance.

    ```sh
    ibmcloud resource service-instance-delete my-secrets-manager
    ```
    {: pre}

3. Delete your test service ID.

    ```sh
    ibmcloud resource service-id-delete $SERVICE_ID
    ```
    {: pre}

## Best Practices
You should note that the Secret Manager sets a limit on the rate in which a client can send API requests to it. The limit is 20 calls per second for all API methods. See: [api-rate-limits](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits#api-rate-limits). 
When constructing the [yaml document](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-tutorial-kubernetes-secrets#tutorial-kubernetes-secrets-update-deployment), each key in the data section is will be polled periodically via REST from the SM instance.  Be aware that:
1. Polling interval is default for 10 seconds, and should be configured with an enviornment parameter `POLLER_INTERVAL_MILLISECONDS` to be greater than 1000 * number of kuberenets secrets.  
2. While multiple kuberenets secrets (represented by multiple yaml docs) poll is spread evenly over the interval time, multiple data entries (represented by keys inside the yaml data section) are fetched consequently without delays from SM. Having many such data entries aggregated inside a kube secret (in the above example we have two: user and password), could make your instance reach the rate limit and return 429 erros back to the tool. Please make sure that you do not create more data entries than needed in each kubernetes secret.  
3. When setting yaml to fetch a SM secret by name rather than id (`keyByName: true`), each data entry generates 2 API calls rather tben one, so be extra careful with the number of data entries in the yaml if you select this option. see: [IBM Cloud Secret Manage Backend documentation](https://github.com/external-secrets/kubernetes-external-secrets#ibm-cloud-secrets-manager). 

## Next steps
{: #kubernetes-secrets-next-steps}

Great job! In this tutorial, you learned how to set up {{site.data.keyword.secrets-manager_short}} to securely populate application secrets to your cluster. Check out more resources to help you get started with {{site.data.keyword.secrets-manager_short}}.

- Design an access strategy with [secret groups](/docs/secrets-manager?topic=secrets-manager-secret-groups).
- Learn more about the [{{site.data.keyword.secrets-manager_short}} API](/apidocs/secrets-manager).




