# Analyze User Sentiment with Azure and NativeScript

Last time we looked at NativeScript and Microsoft Azure, we drilled into one component of one of Azure's cognitive services: the Text Analytics API. Today we are going to continue on that journey by looking at how we can leverage the cloud to perform sentiment analysis on text.

Why would we want this functionality? Imagine if you are taking support requests in your app - or if you have any kind of chatbot feature. Being able to perform sentiment analysis and effectively preview the user's mood can help to focus a response, route a support ticket, or even set a higher priority. Not that we want to reward angry customers, but...🤬

> Interested in using Azure with NativeScript? Learn more at our upcoming [webinar on Thursday, November 7th](https://attendee.gotowebinar.com/register/3325827363192779779?source=blog)!

**NOTE:** Before you continue, if you don't already have a free Azure account, [create one now](https://azure.microsoft.com/en-us/free/). You'll need your subscription key and remote endpoint address to actually do anything!

## Create an Azure Cognitive Services Resource

Our first step, as always with Azure, is to either create or reuse an existing *Azure Cognitive Services resource*. You can do this by following Microsoft's excellent docs for either the [Azure portal](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account) or the [Azure CLI](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account-cli).

The end result of the exercise is to get the all-important **subscription key** and **remote endpoint** to authenticate our NativeScript app with Azure.

When you are done, you should have a **remote endpoint** that looks something like this:

	https://myservicename.cognitiveservices.azure.com

...and a **subscription key** for authentication with Azure, looking something like this:

	8hj3jks686l98098jhkhhu678686adfe
	
## Just How Positive/Negative Are They? 😍😭

