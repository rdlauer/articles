# What is Serverless and Why Does it Matter?

Let's not kid ourselves, until [Pied Piper's "new internet"](https://www.wired.com/2017/06/pied-pipers-new-internet-isnt-just-possible-almost/) goes online and renders datacenters irrelevant, the world will still run on the server. Whether under your desk or in an Azure cloud container, a server is still a server.

How many of us have had conversations with non-techie friends and family asking us what the cloud "is" and whether or not they should be "in" the cloud. Que eyeroll and a pithy explanation of how the cloud is really just someone else's server.

Throw "serverless" into the mix (along with acronyms like IaaS, PaaS, SaaS, and FaaS) and even the best of us end up with a spinning head.

## A Brief History of the Cloud

Today most of us realize the value of the mystical cloud and the ROI it provides for everyone from hobbyists to large enterprises. For instance, in the pre-cloud days, when deploying an app developers had to concern themselves with:

- Buying a server;
- Finding a physical place to house it;
- Connecting it to the Internet;
- Installing and patching an OS;
- Securing it (physical security, firewall, DDoS prevention, etc);
- ...and then maintaining both hardware and software.

Then came companies like Rackspace(?), who promised to offload the responsibility of server logistics. But at the end of the day, these providers were solving only a part of the problem. Creating geo-specific fail-safe clusters, maintaining server software, and mirroring environments from dev -> test -> QA -> prod was still left to us common folk.

**Enter the ☁️.**

As virtualization technologies improved, the ability for Rackspace-like companies to spin up virtual machines at will became the new norm. Instead of provisioning physical hardware, we were issuing commands and double-clicking our way to new servers and new environments.

And where there is money to be made in technology, you can be sure the tech behemoths aren't far behind. Amazon's AWS, Microsoft's Azure, and Google's Cloud ??? rolled over competitors and established themselves as the preeminent providers of cloud computing services, or better labeled, IaaS (Infrastructure-as-a-Service) providers.

These companies have dramatically shifted centralized computing resources from local datacenters (or even just server closets) to world-wide distributed data centers. Think of creating a clustered environment of fifty servers to support your apps. Today this is accomplished with little to no upfront costs and a handful of CLI commands.

## IaaS, PaaS, FaaS, SaaS, oh my!

Today's cloud is best represented as a set of services that effectively build off of each other.

IMAGE OF IAAS/PAAS/SAAS PYRAMID?

### Infrastructure-as-a-Service (IaaS)

The 2010-2012 version of the cloud was effectively just IaaS. Amazon had built up an amazing infrastructure to support their own business. Some evil genius realized that they could re-sell these services to the public.

Instead of building and managing your own datacenter, you could offload those worries to the cloud.

### Platform-as-a-Service (PaaS)

The next layer of our cloud pyramid is PaaS. What is PaaS you ask? Good question! Well...um...

The best way for me to think of PaaS is the concept of deploying your app to a pre-configured environment. As the developer, you don't worry about underlying infrastructure, storage, or network considerations. But you do have control over the apps themselves and environmental configurations.

PaaS is easily the least-defined section of the cloud, yet also holds the most opportunity. Companies like CloudFoundry and Docker are two of the more popular PaaS competitors.

### Software-as-a-Service (SaaS)

In the traditional cloud model, SaaS sits at the top. This is the end game of software development. Digital bits that are offered up for a monthly fee to a virtually limitless number of users. The most popular SaaS offerings include Microsoft's O365 and Google Docs. High performance, distributed services, that can be spun up almost immediately without any local configuration or installations necessary.

### Function-as-a-Service (FaaS)

But wait, where does FaaS sit in the cloud pyramid. And what about this "serverless" thing?

Let's stop talking about FaaS (even though it fits nicely in our acronym model) and start talking about serverless. For all intents and purposes they are the same thing.

> You can easily argue that serverless is "bigger" or "more" than FaaS, but for the scope of this article, let's not worry about that.

### Function-as-a-Service (FaaS) Serverless

Think about a mobile app today. A responsible developer will pick a framework like NativeScript (shameless plug 😄) to share code between iOS and Android. Add on a web variant that needs its own hosting environment. Likely there is a database somewhere, NoSQL or Relational. Maybe a messaging service. A crash monitoring service. You get the picture.

While spinning up environments for all of these services is easier than it's ever been, that doesn't mean it's easy. Likewise, even PaaS providers are still only incrementally adding value to our stack. This is where the concept of serverless comes into play.

**Serverless doesn't mean server-less.** Serverless the concept of bridging the last gap of app deployment and obfuscating any and all decisions completely away from the developer. **You write code and deploy it. End of story.** Everything is provisioned for you.

The serverless difference is development becomes focused on individual functions instead of services. For example, with AWS Lambda, you ???. Likewise, with Kinvey, you ???.

CODE EXAMPLE

These functions live in the cloud, and are executed in the cloud in an environment that is predictable, scalable, and reliable. Developers build systems out of these functions, piecing together apps in the most distributed and powerful way possible.

## The NativeScript Angle

So what is this article doing on the NativeScript blog?

While mobile apps are generally distributed via the app stores and of course run on mobile devices, nearly every app has a supporting backend.

When you are building your next NativeScript app, consider the serverless capabilities of Kinvey. You can build a better app with capabilities like:

- ???
- ???
- ???

Enjoy the 100% free tier of Kinvey to unlock all of these features and build a more robust, more secure, and scalable app.

https://impaddo.com/blog/serverless-architecture/
https://www.kinvey.com/platform/
https://en.wikipedia.org/wiki/Serverless_computing
https://readwrite.com/2012/10/15/why-the-future-of-software-and-apps-is-serverless/

