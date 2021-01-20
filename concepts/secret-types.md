---

copyright:
  years: 2021
lastupdated: "2021-01-20"

keywords: secrets, secret types, supported secrets, static secrets, dynamic secrets, 

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
{:curl: .ph data-hd-programlang='curl'}
{:go: .ph data-hd-programlang='go'} 
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}

# What is a secret?
{: #secret-basics}

A secret is a piece of sensitive information. For example, an API key, password, or any type of credential that you might use to access a confidential system.
{: shortdesc} 

By using secrets, you're able to authenticate to protected resources as you build your applications. For example, when you try to access an external service API, you're asked to provide a unique credential. After you supply your credential, the external service can understand who you are and whether you're authorized to interact with it. 



To learn more about the general characteristics of a secret, check out the following video. 

![What is Secrets Management?](https://www.youtube.com/embed/iETENR5MEB8){: video output="iframe" data-script="#secrets-mgmt-video-transcript" id="youtubeplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Video transcript
{: #secrets-mgmt-video-transcript}
{: notoc}

How are you making sure that your secrets are securely stored so that you can avoid data breaches as well as chaos in your DevOps workflows?

Hi, I'm Alex Greer with the IBM Cloud team, and before I get started make sure to like and subscribe now. 

What is a secret? 

A secret is a digital credential that is going to allow entities to communicate and perform actions on a service. This discrete piece of information keeps that access point secure. So let's take a look at the way in which this paradigm exists. 

Let's start with an entity here that needs to access some sort of service. We'll leave it little ambiguous for right now, but some sort of service. To properly communicate with the service and be able to take the action that it needs to get its job done, this entity is going to need to communicate to the service two things: One, who it is, so that service can understand what or who is interacting with it. Two, it's going to have to know the set of permissions that it should grant in the context of its service. With these, the service can now properly allow that entity to interact with them. How we enable this is with something we call a "secret."

Now, with this dynamic established, let's move into an example with users. For users, we'll say this user here is our entity, and we'll say our service here – let's just say that it's a developer,  and they happen to need read or write access. They're interacting with the development repo in order to do that. To gain that access again, again coming back to the need, we have authorization and permission. How it's going to communicate, specifically in this circumstance, is by giving it user credentials. Now, that user can interact with the dev repo in the way that they need to get their job done.

Now, looking at a cloud native application story, we have a lot of microservices that have to talk to each other. So, let's look at that. Let's just call it service "A," that needs to interact with a database called "DB" and grab a piece of specific information that it needs to get its job done. What it needs, in the form of the secret here, is what we call "DB configs." Again, this DB config is going to allow it to have the right communication with the service saying, this is who I am and this is what I came here to accomplish.

But now that we have our user credentials in our DB configs as examples of secrets, we realize the vulnerability that can be created here if these were to fall into the wrong hands. And this is why it's so important to establish a centralized place to manage all these things as we build out more and more applications and microservices. As we have more of these, the problem becomes more complex. If it falls into the wrong hands, how is protected? How do we block it from getting to that point? How was that data isolated? 

When we look at the damage that this can cause, we're looking in the millions of dollars, for example, for a data breach. In terms of developer operations, if you're not properly managing these, forget even the case in which a bad actor hasn't been involved, but it can be confusing for teams to use this. So what we need to do is make sure that we have it, again, centralized improperly stored so that we can leverage these secrets in the right way and they can properly communicate with services.

OK, now, let's take a look at the next layer of the onion in a more complicated example. 

Now let's go back to Jane, the enterprise developer. Jane – our "E dev" here – needs again to have access to that development repo that she's referring to earlier. Let's just go ahead and call this, maybe it's GitHub. So what she needs to be able to do is have write access and so she's going to need to request the information that's going to give her access to that right role. So that's where a secrets manager service comes in in a perfect, complementary fashion. 

A secrets manager service can securely store these credentials, along with other types of secrets, in a centralized way for her to be able to access or maybe other services to access, like we'll see in a second. But when it's done, it's given her the peace of mind that her user credentials are securely stored and now she can worry about authenticating with the service and getting her job done because secrets manager is going to take care of that for her. 

However, what's really important for secrets manager service to do is interact with the cloud service provider's IAM, or identity and access management service. IAM is going to be the source of truth allowing secrets manager to one, authenticate who she is so that it can pass it down to GitHub, and secondarily also allow for her to get the right set of roles based on the paradigm that they have within their IAM service. With this, we now understand what it's like for a user to get the right permission and be able to access the tool of their choice of their service, and be able to do this in the context of using a secrets manager service. 

But now, let's look what it's like for a service to interact with another service and potentially where data breaches could be harmful. 

Let's look at our service to service example. What we have to start with is a lending application. This lending app is going to want to request permissions to be able to access – again, we were talking about a database earlier. Let's be a little bit more specific: this database that it needs to access is a given table within the database has necessary information to give to its model in order to be able to make a judgment on whether or not they want to provide a consumer alone. So we're going to call this a profile database. So within here, we have profile DB, and we know that the set of permissions that we want to grant are going to be read permissions for table A. So again, where are we going to store the secret that's ultimately going to give us access to give us access to authentication and ultimately to the set of permissions that we need here? So that's again going to be the secrets manager service provided, and that service needs to talk to Cloud IAM again. Now that we've got this established, what type of credential do we have here? The credential that we have is called DB config. So let's think about the scenario we just walked through: this DB config allows this lending application a service to be able to have read access or be able to take a specific set of information from a given table within a database that has some IP data and some other highly sensitive information in it. So, in a scenario in which these DB configs are not stored properly or the service that they stored in is compromised, what we ultimately get is a pretty catastrophic scenario for the provider, because the provider of this service of this lending application then has to go and tell its customer that a bad actor was able to take advantage of access that it got wrongfully to its DB config. And, that DB config ultimately allowed that bad actor to steal their data and do whatever they wanted with it against that customer. What we can do to mitigate it again is stored in a safe location or bad actors do not have access and you have the level of data isolation that you're comfortable with as an enterprise. That's where we have secrets manager services.

Again, secrets manager services help to ensure the secure storage of secrets so that you don't have to worry about data breaches from last credentials or from other types of secrets. And, ultimately, it makes it a little more efficient for the management of your secrets while you're going through your DevOps operations.

Thank you. If you have questions, please drop us a line below. If you want to see more videos like this in the future, please like and subscribe, and don't forget you can grow your skills and earn a badge with IBM Cloud Labs which, are free browser-based interactive Kubernetes labs. 


## Types of secrets
{: #secret-types}

As an enterprise developer, you might encounter various secret types. {{site.data.keyword.secrets-manager_short}} helps you to secure user credentials, IAM credentials, and arbitrary secrets.

Generally, you can further classify these secrets based on how they are created and stored. Some secrets are created ahead of time, while others are generated only when they are accessed. 

<dl>
  <dt>Static secrets</dt>
    <dd>Static secrets are secrets that you create and store before you need to supply them. For example, you might decide to create and store a set of user credentials for an external service that you don't need to access right away. You delete static secrets manually when you no longer need them.</dd>
  <dt>Dynamic secrets</dt>
    <dd>Unlike static secrets, dynamic secrets are generated only when they are accessed. Dynamic secrets are configured with a time-to-live (TTL) that determines how long they can exist. When you use a dynamic secret, a unique credential is generated on your behalf. After the credential reaches the end of its TTL, access to the protected resource is revoked and the secret is deleted automatically.</dd>
</dl>

### User credentials
{: #user-credentials}

User credentials consist of a username and password that you can use to log in to or access an external service or application. In {{site.data.keyword.secrets-manager_short}}, user credentials are static secrets that you can create and store in your instance ahead of time. You can enable automatic rotation for user credentials so that you can create new versions of them automatically based on the rotation frequency that you specify.

### IAM credentials
{: #iam-credentials}

IAM credentials are dynamic secrets that are used to access an {{site.data.keyword.cloud_notm}} resource on-demand. A set of IAM credentials consists of a service ID and an API key that is generated each time that you access a protected resource. By defining a time-to-live (TTL) or a lease duration for your IAM credential at its creation, you mitigate against secret compromise and shorten the amount of time that that secret can exist. Because an IAM credential is revoked automatically when its lease expires, you don't need to manage its rotation manually.

### Arbitrary secrets
{: #arbitrary-text}

Arbitrary secrets can hold random data that you can use for authentication and authorization to any protected system, whether the system is inside or outside of {{site.data.keyword.cloud_notm}}. By using the {{site.data.keyword.secrets-manager_short}} UI, you can select a file from your computer to use as an arbitrary secret, or you can enter a custom value. In {{site.data.keyword.secrets-manager_short}}, arbitrary secrets are static secrets that you can create and store in your instance ahead of time.
