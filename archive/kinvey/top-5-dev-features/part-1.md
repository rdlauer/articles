# Top 5 Kinvey Features Developers Adore - Part One: FlexServices

On the Developer Relations team here at Progress, we regularly interface with the users of the tools we provide. Developers. Everything Progress offers, whether it be OpenEdge, Sitefinity, Telerik UI, Kendo UI, or NativeScript - each product line and product has a singular goal of making developers more productive doing what they do best.

Progress Kinvey is no different. Kinvey adds significant value to the backend, just as our UI suites add value to the front end. Kinvey provides a secure, robust, performant serverless platform on which developers can create more engaging systems. This leads to more engaging apps, with increased customer satisfaction and retention.

In this five-part blog series, we will look at some specific features of Kinvey that our marketing materials may normally overlook. That's because these features appeal directly to the developer, the user of Kinvey (which, as we know, is not always the buyer, right enterprise developers!).

- Part One: FlexServices
- Part Two: Kinvey Studio
- Part Three: Mobile Identity Connect
- Part Four: Data Connectors + SDKs
- Part Five: Kinvey Chat

Let's start with the business logic capabilities of FlexServices.

## What are FlexServices?

To start at the very beginning, FlexServices are JavaScript apps that run on the Node.js platform provided by Kinvey. Their primary purpose is to allow you to add secure and performant server-side business logic to your web or mobile app.

> Before we go any further, you should know that Kinvey supports a myriad of SDKs out of the box. We'll dive into this in a later part of this series!

Why run your business logic in the cloud, versus on the client?

- Performance: FlexServices in Kinvey run on a low latency, high performance serverless platform.
- Security: Business logic is stored in the cloud, not on the device. Keep prying eyes away from your intellectual property.
- Reusability: Kinvey enables you to create loosely-coupled components that are easy to reuse throughout your app.

Now Kinvey does offer multiple means of solving this same problem, but FlexServices is clearly the most powerful and the wave of the future on the platform.

FlexServices are more than just writing some JavaScript code and deploying it to the server. These functions can also contain third party API integrations, integrate with existing enterprise-grade authentication systems, or facilitate data integrations across disparate platforms. Is your customer data stored in Salesforce but your product data in SQL Server? FlexServices can help with that!

Not only can these FlexServices be custom RESTful endpoints, like so:

	EXAMPLE

...but they can also be hooks that are called when data is inserted, updated, or deleted from a Kinvey collection (or table).

	EXAMPLE?

A FlexService could also be the system of record for data in a Kinvey collection - or even used for authenticating end users.

IMAGE?

## FlexServices, FlexAuth, FlexData, 💪

*Internal FlexServices* are deployed on the FlexService Runtime (basically what we've been talking about up until now - the cloud-based infrastructure manged by Kinvey). While this type of deployment is the one recommended for most users, there is another option.

External FlexServices are deployed on the server of your choice. These deployements are most suitable for customers who require the service to run on a custom infrastructure (such as an on-premises deployment).

> Note that when you deploy FlexServices outside of the Kinvey cloud, you lose the out-of-the-box high availability, disaster recovery, load balancing, and scaling provided by Kiney!

But let's be clear: FlexServices can be more than just business logic functions that run in the cloud.

You can even use FlexServices to create custom user authentication handlers using FlexAuth (allowing you to interact with auth providers outside the scope of those covered by Mobile Identity Connect). Also, FlexData exists to enable developers to build completely custom data connections for your Kinvey collections (think along the lines of merging disparate APIs and external or on-prem data sources). This is useful for when you're trying to utilize a data source not otherwise supported by RapidData.

## FlexServices Examples

Let's take a look at what makes up a basic FlexService:

Your first FlexService can be as simple as a JavaScript function that returns a string:

	function HelloWorld(context, complete, modules) {
		complete().setBody({message: 'Hello FlexServices!'}).done();
	}

With the Flex SDK installed, you register the above service:

	flex.functions.register('HelloWorld', HelloWorld);

...and call it locally:

	http://localhost:10001/_flexFunctions/HelloWorld

> For a far more detailed getting started experience, check out [Getting Started with Kinvey FlexServices](https://www.progress.com/blogs/getting-started-with-kinvey-flexservices)

For more real world examples, we can think along the lines of:

- Creating a FlexService that acts as an aforementioned [collection hook](https://devcenter.kinvey.com/rest/guides/business-logic#collection-hooks). You can manage data pre/post fetch, pre/post save, and pre/post delete activities from a Kinvey collection. Maybe you want to fire off a notification or hit some other endpoint when a record is deleted or when a certain type of data is inserted.
- Connect completely distinct data sources, APIs, and Kinvey collections. Make them available as a single custom endpoint that can be called by your app.
- Using FlexData, a FlexService can connect to external data sources that can serve as the system of record for a Kinvey collection.

To deploy your FlexService remotely you'll want to use the FlexService runtime. This is a serverless runtime environment that allows you to deploy your services to the cloud, on Kinvey-managed infrastructure. (Such a deployment is an example of an internal FlexService as we previously discussed.)

This is just scratching the surface. For a complete tutorial on how to build FlexServices with Kinvey, check out the article on [Getting Started with Kinvey FlexServices](https://www.progress.com/blogs/getting-started-with-kinvey-flexservices).

> Likewise, check out this public repository on GitHub with a set of pre-built FlexServices examples!

## Next Steps

Kinvey FlexServices brings the power and security of server-side business logic home to the Node.js developer. By layering FlexServices on top of existing data, adding hooks to common data management tasks, using it to combine disparate data sources, or even managing third party APIs, JavaScript developers can take their apps to the next level.

But that's just the beginning! The next part of our series on the top Kinvey features for developers focuses on the developer-approved low code tool, Kinvey Studio.