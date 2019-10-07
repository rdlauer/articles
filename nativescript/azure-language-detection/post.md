# Language Detection with NativeScript and Azure Cognitive Services

Ever have the need to determine the *language* of text input in your mobile app? While this may seem like a very niche bit of functionality, if you think about it, there are numerous use cases for language detection:

- Translating text between languages
- Routing questions to a person with the appropriate language
- Any utility or game to help people identify common language things...

Thankfully we can look to the cloud for an absurdly easy solution to this problem. Specifically, Microsoft Azure.

Azure provides a variety of "cognitive services" that allow your apps to interact AI-powered algorithms in the cloud. You can quite literally allow your app to use some of its "human" senses by seeing, hearing, speaking, and interpreting input via traditional communication methods,

Let's take a look at how we can tap into just one of these Azure Cognitive Services APIs today: Text Analytics.

> **NOTE:** Before you continue, if you don't already have a free Azure account, create one now. You'll need your subscription key and endpoint to actually do anything!

## Create an Azure Cognitive Services Resource

We need that all-important subscription key and remote endpoint to authenticate our app with Azure. So first, you'll need to create a new *Azure Cognitive Services resource* using either the Azure Portal or the Azure CLI. This resource will enable access to the Text Analytics APIs.

> **TIP:** No need to replicate the Microsoft docs! There are some simple instructions on how to do this...

With this step complete, you should have a remote endpoint that looks something like this:

	asdf

...and a subscription key for authentication with Azure, looking something like this:

	asdf

## English, Bulgarian, or...Esperanto?

With your key and endpoint in-hand, we can get to the code. The sample app I create today is going to be awfully simple. It's going to include:

- A `TextView` UI component for, well, text;
- A `Button` UI component for the user to click (stop me of this is getting too complicated);
- A `Label` component to display Azure's best guess at a language of the inputted text.

Here is my basic UI layer:

	code

With a sprinkling of CSS:

	asdf

> **TIP:** If you're new to NativeScript, my favorite resources include:

Now we will want to wire up our UI layer to Azure. We don't need any fancy Azure SDK - though there is a JavaScript SDK should you need to use one in the future.

	code

Let's walk through this code really quickly.

talk talk walk walk here

The resulting JSON response from the above is going to look something like this:
	
	{
	   "documents": [
	      {
	         "id": "1",
	         "detectedLanguages": [
	            {
	               "name": "English",
	               "iso6391Name": "en",
	               "score": 1.0
	            }
	         ]
	      },
	      {
	         "id": "2",
	         "detectedLanguages": [
	            {
	               "name": "Spanish",
	               "iso6391Name": "es",
	               "score": 1.0
	            }
	         ]
	      },
	      {
	         "id": "3",
	         "detectedLanguages": [
	            {
	               "name": "Chinese_Simplified",
	               "iso6391Name": "zh_chs",
	               "score": 1.0
	            }
	         ]
	      }
	   ],
	   "errors": [
	
	   ]
	}

You can see in the `detectedLanguages` node that we've identified "English" as the most probable language.

At this point your app logic can take over and direct the user's experience in the app based on the language detected!

## What's Next?

We started our series on Azure Cognitive Services with the relatively basic premise of language detection. Next time we will actually analyze the sentiment of the text inputted in the app!