# Analyze User Sentiment with Azure and NativeScript

Last time we looked at NativeScript and Microsoft Azure, we drilled into a single component of one of Azure's cognitive services: the Text Analytics API. Today we are going to continue on that journey by looking at how we can leverage the cloud to perform *sentiment analysis* on a string of text.

*from microsoft:*

*The Text Analytics API is a suite of text analytics web services built with best-in-class Microsoft machine learning algorithms. The API can be used to analyze unstructured text for tasks such as sentiment analysis, key phrase and entity extraction as well as language detection. No training data is needed to use this API; just bring your text data. This API uses advanced natural language processing techniques to deliver best in class predictions.*

Why would we want this functionality? Imagine if you are considering taking support requests in your app, or any kind of interactive chatbot feature, or maybe you just want to add some gimicky features to your app 🤷.

Being able to perform sentiment analysis and effectively preview the user's mood can help to focus a response, route a support ticket, or even set a higher priority. Not that we want to reward angry customers, but...🤬

> Interested in using Azure with NativeScript? Watch a replay of our "Building Mobile Apps that Scale with Azure" webinar.

**NOTE:** Before you continue, if you don't already have a free Azure account, [create one now](https://azure.microsoft.com/en-us/free/). You'll need your subscription key and remote endpoint address to actually do anything!

## Create an Azure Cognitive Services Resource

As always with Azure, our first step is to either create or reuse an existing *Azure Cognitive Services resource*. You can do this by following Microsoft's excellent docs for either the [Azure portal](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account) or the [Azure CLI](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account-cli).

The end result of the exercise is to get the all-important **subscription key** and **remote endpoint** to authenticate our NativeScript app with Azure.

When you are done, you should have a **remote endpoint** that looks something like this:

	https://myservicename.cognitiveservices.azure.com

...and a **subscription key** for authentication with Azure, looking something like this:

	8hj3jks686l98098jhkhhu678686adfe
	
## Just How Positive/Negative Are They? 😍😭

Like the previous demo, we are going to keep the UI for our main page simple for this simple demo:

	<TextField hint="Hey! Are you 😀 or 😭" text="{{ theText }}" returnKeyType="done" id="txtText" />
	<Button text="Submit" tap="{{ onTap }}" class="-primary -rounded-sm" />
	<Label id="lblResult" class="h2 text-center" textWrap="true"/>
	
> **TIP:** Consult the previous post in this series to get a more in-depth look at the UI layer for this app.

So let's figure out a way to wire up our really wonderfully-named `theText` value to Azure and get an idea of the sentiment conveyed in the text.

Luckily for us, we don't need to overengineer this solution. Like the previous example, it's just a call to a RESTful API provided by Azure. So we can update our `path` variable to be:

	const path = '/text/analytics/v2.1/sentiment';
	
...and hit the `sentiment` endpoint.

So what is returned by this endpoint? Like with the language detection API, we are going to send an array of `documents`:

	{
	  "documents": [
	    {
	      "id": "1",
	      "text": "I love everything!"
	    }
	  ]
	}

Then the value returned in the response:

	{
	  "documents": [
	    {
	      "id": "1",
	      "score": 0.92
	    }
	  ],
	  "errors": []
	}
	
...is simply the `score` from 0 to 1. We can then process this `score` in any way we see fit. In our case, we are simply showing an emoji based on the sentiment of the text:
	
	request({
	    ...
	}).then(
	    response => {
	        let res = response.content.toJSON();
	        let score = parseFloat(res.documents[0].score);
	        if (score >= 0.66) {
	            lblResult.text = '😀 (' + score + ')';
	        } else if (score < 0.66 && score >= 0.33) {
	            lblResult.text = '😐 (' + score + ')';
	        } else {
	            lblResult.text = '😭 (' + score + ')';
	        }
	    }
	);

**Easy enough!** No complicated SDKs to learn. Everything accomplished using the familiar http calls we already make with our web apps.

## What's Next?

In our first post we tackled language detection, today was sentiment analysis. Next time we will take a look at the key phrases API that will allow us to extract key components from a long string of text.

Happy NativeScripting with Azure! ☁️
