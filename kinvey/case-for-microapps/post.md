# The Case for Microapps

Well good morning, good afternoon, and good evening to everyone out there. Welcome to our webinar today which is all about microapps.

Microapps is a super hot topic right now. But frankly a lot of us, myself included before we started planning this webinar, don’t really know what microapps are or what benefits they bring.

So today we are going to dive into microapps and do a very pragmatic overview of:

What exactly is a microapp
What is the business case for a microapp?
Meaning what efficiencies are we bringing to the table when we talk about moving from traditional app development to a microapps style architecture.
A full demonstration of the Kinvey Microapps platform
How NativeScript fits into the equation
Spoiler alert: it’s a big part

But first let’s tackle some webinar logistics.

If you can’t hear me, which of course you wouldn’t be able to hear me say. But we just ask that you try connecting to your computer audio, not your phone, to listen to the audio of this webinar.

And per usual everything you see here today will be made available on YouTube in the coming days. This means if you run into any audio or visual problems, rest assured you’ll be able to watch this webinar, in its entirety, later on. We will send out follow up emails with this information included..

If and when you have questions during the webinar, what are you supposed to do? [CLICK] Well you can use the Q&A panel in your GoToWebinar interface. We will be here to answer your questions over the next hour or so.

[insert {N} intro section here]

Intro speakers

So let’s start by answering that very first, and very key question, what is a microapp?

Well a microapp is just like any other app on your mobile device, except it’s far more focused on performing one task, and performing it very efficiently. In software terms, we often think of the single responsibility principle, which basically boils down to doing one thing, only one thing, and doing it well. So microapps takes this principle and applies it more broadly to this new paradigm of mobile experiences.

But let’s be crystal clear, microapps are not really a new thing. You’ve likely been using microapps yourself, maybe without even realizing it. Google is famous for including microapps in their search results.

Say you want to look up the standings of your favorite sports team. This experience occurs via tiny, focused microapps. I get all of the information I need without having to wade through a larger resource. In this case, I can avoid clicking around espn.com for instance to get that same information. Now whether or not this is a good thing is a battle to be waged between Google and ESPN. But the value for the consumer is evident. We can be hyper-focused on providing exactly what the consumer needs, without all the extra cruft and overhead if you will. Another good example is looking up flights on Google. I’m provided a simple, focused UI, that then leads to a more full-blown experience when I’m ready to buy.

Today we are going to look at microapps in terms of mobility. Because today we have a problem. Modern enterprises often have 20 or more web and mobile apps that you, as an employee, have to use just to get your job done. And how much functionality do you really use within these apps? It can be hard to find what we want, when we want. We are also suffering from app fatigue, both as app consumers in our personal lives and in our work lives as we deal with tens to hundreds of apps on our devices.

As app developers we are also suffering, because these 20+ apps have to be maintained. We are also fielding requests for more and more apps, just adding to the maintenance pile. And each one of these new apps comes with some baseline work for us to get it up and running:

Authentication
Push
Offline
Metadata mgmt
Monitoring
Device logging
Crash analytics
App approval
App distribution
App updates
App branding/theming
UI
Biz logic
auth/provisioning

As app developers, instead of marching through these steps for each new mobile experience we create, microapps allow us to do the hard and boring stuff once, and focus on the practical features and engaging experiences that we ultimately want to deliver.

And consumers of these apps, even internal apps, are getting more picky and require:

X-plat
Native ui, native perf, 60fps
Chatbots (example of new UI paradigm)
AR/VR experience
Full API access
Access via consumer devices (BYOD)

As app consumers we are provided with one app that provides focused functionality that we need, with modern and engaging features.

So microapps dramatically improves the experience for both app consumers and app developers.

You can also think of all this in terms of a web portal. Back in the day we had this concept of portlets that would encapsulate a specific set of functionality that could be provisioned to web users via a web portal. You can think of microapps as an evolution of this model for the mobile world.

So we’ve identified some problems and how microapps in general help solve these problems. Let’s now talk about how Progress is solving this problem. This is where Kinvey Microapps comes into play. Kinvey Microapps is part of the established Progress Kinvey offering, which is a high productivity app development platform.

From a technical perspective, Kinvey Microapps enables you, as an app developer, to be dramatically more productive delivering mobile experiences to your users.

To deliver a microapps experience, you build a single container app. This container app is cross-platform, providing a native experience for both iOS and Android users that is branded for your company.

Your container app then contains any number of microapps. Each microapp can be created from existing microapp templates we provide. From scratch as a brand-new NativeScript app (but again, without the ceremony of scaffolding what a new app normally requires). These from-scratch microapps can use existing cocoapods, android jar files, and JavaScript libraries, allowing you to re-use existing resources when you create new microapps. Or a microapp can even be an existing legacy web app. Yes, that means you can simply plug in a URL and it will automagically work as a microapp.

Created for you is also a mediation layer, effectively allowing you to create and host backend services or microservices to connect to remote APIs and manage data coming from those APIs. Building on top of Progress Kinvey also allows you to connect to other backend systems with your microapps like SAP, Salesforce, Oracle, SQL Server, and so on. You can also tie into integrations with existing enterprise authentication providers like Active Directory, LDAP, SAML, oAuth2, you name it.

Finally, microapps created by the Kinvey Microapps platform are business-agnostic. It doesn’t matter if you are a massive corporation, a city or county providing public resources, or a smaller organization trying to make your employees as efficient as possible. Every part of your company can benefit from microapps, whether it’s field service, sales, product development, or human resources.

Now hopefully that helps kind of gel what the concept of microapps is and how they can benefit you, your business, and your app consumers. So let’s switch over to a practical technical demo of the microapp development experience. And today you might see some references to Eloha. Just to clarify, Eloha is a graduate of Progress Labs. Progress Labs is our internal incubator of new product ideas. So Eloha has become Kinvey Microapps. If you see Eloha, just think Kinvey Microapps (and of course this branding is being updated, I just wanted to point this out to avoid any confusion).

Today we are going to look at how microapps can provide benefits from the HR perspective. I think for a general audience we can all relate to this problem of needing to find company resources to ask a question, fill out a leave request, or any of the myriad HR-related tasks we face. To accomplish this, I’d like to switch over to Tara Manicsic, who is going to provide a demo of how this all comes together with the Kinvey Microapps platform. Tara, the stage is yours.



Thank you Tara. So hopefully at this point you have a good grasp of the business case for microapps along with a sense of how you can implement this from a technical perspective.

Today we’ve seen how the Kinvey Microapps platform dramatically simplifies:

App creation (project is simple, easier to initiate, lower cost, lower development time, fewer bugs)
Speed up delivery (with role-based provisioning the right people get the right apps at the right time, it’s also far easier to update existing apps)
Connecting to disparate and legacy data, systems, and services is also a core feature of Progress Kinvey and Kinvey Microapps
Building on NativeScript...providing native UI, AR/VR, chatbots

Now before we end today, I’d be remiss if I didn’t tell you about jsmc…

And finally where do you go from here? Well you can learn more about Kinvey Microapps at…. If you’re interested in speaking to one of our microapp specialists, you can set up a completely free no-obligation meeting to dive into microapps at ….

Thank you all for attending and have a great rest of your day.
