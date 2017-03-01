# Creating Your First Native Mobile App with Visual Studio Part 4

[**<-- Read Part 3**](#)

Over the past few blog posts, we have been working on creating a NativeScript app with Visual Studio and SQL Server - covering each and every part of the development process. We've been using the capabilities of Telerik AppBuilder to allow us to develop an iOS app from within Windows (all without having a Mac).

In the previous post, we knocked out our SQL Server/ASP.NET Web API backend. Now we are going to wire up our app and make it look gorgeous. By the end of this article we hope to:

- Wire up our NativeScript app with our backend service;
- Learn about NativeScript animations and style our app with CSS;
- Implement a chart with Telerik UI for NativeScript.

Let's get started!

## NativeScript + ASP.NET Web API

In the last post we created a very simple Web API service. Let's take a moment to wire that service up to our app!

We are going to kill two birds with one stone by creating a new method that will handle both the initial load of a quote from our Web API, but also to handle what happens when we want to load a *new* quote.

In our `main-page.xml` file, change the `tap` method for our `ActionItem` to a new JavaScript function called `loadQuote`:

	<ActionItem tap="loadQuote" ...

Next, in our `main-page.ts` file, we are going to change a few things around to accommodate a new `loadQuote` method. We need the `loadQuote` method to fetch data from our remote endpoint, format the JSON response, and bind it to our UI elements.

Here is the complete `main-page.ts`:

	import observable = require("data/observable");
	import pages = require("ui/page");
	
	let page;
	
	export function pageLoaded(args: observable.EventData) {
	    page = <pages.Page>args.object;
	    loadQuote();
	}
	
	function getRandomNumber(min: number, max: number) {
	    return Math.random() * (max - min) + min;
	}
	
	export function loadQuote() {
	    let myLayout = page.getViewById("layout");
	
	    fetch("http://your-web-server.com/billmurray/api/quote/GetQuote").then(response => { return response.json(); }).then(function (r) {
	        myLayout.bindingContext = {
	            quote: "\"" + r.Data.QuoteText + "\" - " + r.Data.QuoteSource,
	            image: "https://www.fillmurray.com/200/" + getRandomNumber(295, 305)
	        };
	    });
	}

**What all changed?**

- The value of the `page` variable is still set on `pageLoaded`, but now available to other methods;
- We added a simple `getRandomNumber` function to return a random number between two values (why? see below!);
- We added a `loadQuote` function that uses the fetch API to access our remote Web API.

> You may notice `r.Data` prepended to the `QuoteText` and `QuoteSource` properties from the JSON response. If you recall in part 3, the data we want from our Web API is contained in the `Data` property.

*So what's with the random number?* We append a random number to our request from fillmurray.com. This is a quick-and-dirty way to get a random image with every request (since each request with a different pixel size provides a different image).

Once these changes are in place, use the **Synchronize {app name} with Cloud** menu option and do a **three-finger press** to load your changes (or use the **Build {app name} in Cloud** to load a new copy of the app). The app should work like it used to, but also provide the ability to load a new quote.

But we can do better!

## Animations and Styling

NativeScript's power comes from the ability to use JavaScript/TypeScript to create truly native mobile apps. But I contend that any great app needs to be easy to style and have engaging features that keep the end user coming back for more. And NativeScript also enables you to create these more engaging user experiences by:

1. Providing an easy to understand [animation API](https://docs.nativescript.org/ui/animation);
2. Letting you re-use your web-based [CSS styling](https://docs.nativescript.org/ui/styling) knowledge.


### Quick Look at Animations

**Let's start by adding some animations to our app.** I'd like to animate the `Label` element when a quote is loaded and when it is replaced with a new quote.

Back in the `main-page.xml` file, we need to give our `Label` element an id, so it can be easily selected and animated in our code-behind:

	<Label text="{{ quote }}" class="title" textWrap="true" id="lblQuote" />

At the top of `main-page.ts`, we need to import the `AnimationCurve` enumeration like so:

	import {AnimationCurve} from "ui/enums";

All of the other changes happen in the `loadQuote` method. Let's look at the new method and then go through the changes we made:

	export function loadQuote() {
	
	    let myLayout = page.getViewById("layout");
	    let lblQuote = page.getViewById("lblQuote");
	
	    lblQuote.animate({
	        translate: { x: -300, y: 0 },
	        duration: 250,
	        curve: AnimationCurve.linear
	    }).then(() => {
	
	        fetch("http://your-web-server.com/billmurray/api/quote/GetQuote").then(response => { return response.json(); }).then(function (r) {
	
	            myLayout.bindingContext = {
	                quote: "\"" + r.Data.QuoteText + "\" - " + r.Data.QuoteSource,
	                image: "https://www.fillmurray.com/200/" + getRandomNumber(295, 305)
	            };
	
	            lblQuote.animate({
	                translate: { x: 0, y: 0 },
	                duration: 1000,
	                curve: AnimationCurve.spring
	            });
	
	        });
	    });
	}

**What all changed?**

- We get a reference to our `lblQuote` element;
- We `animate` the quote out of view (negative x-axis) using a [linear animation](https://docs.nativescript.org/ui/animation#animation-curves);
- Since `animate()` method returns an [ES6 promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), it's easy to chain operations together with `.then` syntax;
- We fetch the data and again animate the label back into view with a [spring animation](https://docs.nativescript.org/ui/animation#animation-curves);

### Quick Look at Styling with CSS

Before we see the result of our labors, let's put the final styling touches on our app with some CSS.

There are a few UI tweaks I'd like to make to this app. Namely:

- The font size could be smaller;
- Bill Murray's image needs a little margin to add white space;
- The action bar and tab strip could use some color;
- Add some font icons to our tab strip.

**Let's take a look at how we can do all of this with some simple CSS!**

First up, font size. You'll see how our `Label` element already has `class="title"`. Yes, this is in fact the same class selector you use on the web! Go ahead and open up the `app.css` file.

> Remember that styles added to `app.css` are available globally. If you want page-specific classes, use the `page-name.css` syntax.

Find the `.title` selector and decrease the `font-size` property from 36 to 24. Remember we use device-agnostic units in NativeScript, so don't add a "px" or "em" to your sizes.

Next, let's add some margin to our image. You can probably figure this out on your own, but we are simply going to add a `class` to our existing `Image` element, like so:

	<Image src="{{ image }}" class="image" />

...and in our CSS file, add:

	.image {
		margin: 20;
	}

Now let's add some color! Following a similar pattern, we are going to add classes to our `ActionBar` and `TabView` elements in `main-page.xml`:

	<ActionBar title="Bill Murray Quotes" class="action-bar">

and

	<TabView class="tab-view">

...and in our CSS file, add:

	.action-bar {
	    background-color: #e0e0e0;
	}
	
	.tab-view {
	    tab-background-color: #1084ff;
	    tab-text-color: #ffffff;
	    selected-tab-text-color: #ffffff;
	}

**Last, but not least, let's add a new font to our app.** NativeScript allows you to use TTF fonts in your app, just like you would on the web.

Start by downloading the latest [Font Awesome font set](http://fontawesome.io/). Unzip it and find the `fontawesome-webfont.ttf` file (in my case found at "font-awesome-4.7.0\fonts"). Create a new directory called **fonts** inside your **app** directory in your Visual Studio project. Copy and paste the `fontawesome-webfont.ttf` file into that directory.

Our `.tab-view` CSS can now have the `FontAwesome` font applied to it, like so:

	.tab-view {
	    ...
	    font-family: FontAwesome;
	}

Now we can use the Font Awesome icon set! Let's add icons to our "Quotes" and "Chart" tabs. Open up `main-page.xml` and replace the two `TabViewItem` titles with these:

	<TabViewItem title="&#xf10d; Quotes &#xf10e;">

and

	<TabViewItem title="&#xf200; Chart">

