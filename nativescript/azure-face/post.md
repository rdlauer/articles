# Using Azure Cognitive Services in NativeScript

Let's face facts: the computers have won! The future now effectively lies in the hands of our supreme robot leaders.

image - "i for one..." from simpsons

While the present day isn't *quite* a Terminator-style reality, the truth is that "cognitive services" are exploding. From machine learning to artificial intelligence to other cognitive APIs, these "thinking" services are offloading incredible capabilities to the cloud.

> It's no wonder that the future of Progress is Cognitive Apps! Take a look at our future strategy that includes NativeScript and Kinvey.

Out of the myriad of cognitive services out there, Azure stands out as a reliable option for engaging APIs such as:

- Face
- ???

## Azure Face API

One of the more fun APIs to play with is Azure's Face API. Using the Face API, you can send Azure an image with a person's face, and it will return numerous attributes:

json result

Let's take a look at how we can get started with the Face API.

## Setting Up an Azure Account

Azure provides a completely free 7 day trial to use its cognitive services (and more). After that, you'll have to use a credit card to continue access.

image?

No worries though, as Azure also provides a $200 credit for the first 30 days AND will warn you before you start accruing any fees.

With your Azure account created, navigate to the ??? section:

image

Search for "face" and simply enable the Face API:

image

At this point, take a note of two things (you'll need both of these in your NativeScript app):

1. Your API key.
2. The URL of your Azure endpoint.

Done? Great! That's all we need from the Azure console.

??? Intro to {N} section ???

## Building a NativeScript App

> **TIP:** This app is inspired by a meetup session given by Ignacio Fuentes at ???. Click here to view the complete session.

We can build a simple app that has two views. One to list people and another to show an individual's picture along with facial attributes returned from Azure.

Since the Meetup API is so gracious to return photos, we can easily use that as our data source for our first view.

url

...returns the following JSON for the selected NYC ??? meetup:

json

Consuming this in NativeScript is extremely easy, with a `fetch` function:

code

### List of People

We can then display this data in a NativeScript ListView (which is a cross-platform abstraction of native ??? and ??? for iOS and Android, respectively).

code

Let's sprinkle in a little CSS to clean up the look of our app:

code

This should magically return the following screen:

image

> Now is a good time to note that the full source code for this app (built with TypeScript) is available in this GitHub repository. If you're looking for a similar version for Angular, take a look here.

### Photo Details with Facial Attributes

When I tap on one of the individuals in the `ListView`, I can send a link to the image to Azure and the Face API:

code

...it's at this point the Azure magic happens and returns the following JSON:

json

You'll see we can retrieve Azure's best guess at an age, gender, and mood (mood being the highest number of all possible moods).

code

Provided we haven't missed anything (hey, I'm an awful coder sometimes, you never know!) we should see a screen that looks something like this:

image

And we are done!

## Conclusion

While this was an extremely simple example, it does give you glimpse of the power of cognitive services. Whether you are using Azure, AWS, or Progress, rest assured cognitive-first is the future of engaging app development across all devices and experiences.