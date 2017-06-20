Progress's NativeScript and Appcelerator's Titanium are both powerful platforms for building **native mobile applications** using Javascript. Being well versed in multiple platforms is not only smart, but prudent. As a developer, being able to make the connections across multiple platforms and tool sets makes you a more valuable developer as a whole.

In this post we are going to walk through the similarities and differences between NativeScript and Titanium Alloy and how one would go about writing a NativeScript application if you were used to writing Titanium Alloy applications.

Trying new things
----
NativeScript and Titanium Alloy have very similar methodologies for structure and design concepts. This article is for those that have been using Titanium Alloy and would like to try NativeScript for the first time or moving those concepts to larger projects.

Moving the cheese
----
If you have never read the book ["Who Moved My Cheese"](https://www.amazon.com/Who-Moved-My-Cheese-Amazing/dp/0399144463), the author talks about dealing with change in your life and work. I will save you the time of reading the book, the whole point is that change is good and can ultimately lead to something better. So often when we try something new but similar to something we are used to it can be even harder than trying something completely new. Our preconceptions can blind us to new opportunities and ways of doing things.

For this article let's take a step past *"This is how (insert pet platform/method) does it and that is the only way"*, to a mindset of curiosity and openness.

Getting Started
----
Nativescript has an advantage over many other platforms because its core libraries are written in Javascript/Typescript. This makes it incredibly flexible and makes it easy to write your own libraries on top of the existing ones. 

For this article we are going to use the NativeScript Core methodology. There are three major schools of thought when building a NativeScript app: NativeScript Core, NativeScript Core with Typescript, and NativeScript with Angular. 

To some this might seem fractured but it makes a lot of sense. We all go to Five Guys and order a burger but some of us like Jalapenos on our burger and some of us don't. So instead of thinking of them as different meals, think of them as the same burger with different toppings.

I would be remiss if I did not at least mention Angular and the structure it can provide your applications. When trying NativeScript with a background in Titanium Alloy it makes the most sense to use Core, but if you are willing to step outside your comfort zone a bit and try Angular you won't be disappointed.

That brings us to getting started. This is not a getting started guide so head on over to the getting started guide and come back once you are ready to dive in.

If you haven't gone through the [NativeScript Getting Started Guide](http://docs.nativescript.org/tutorial/chapter-0), please do that now.

Assumptions
----
1. We are going to assume you have gone through the getting started guide for NativeScript, or {N} as we will refer to it from now on.
2. You have a basic understanding of concepts and methodologies used in Titanium Alloy, or Alloy as we will be referring to it for the remainder of this article.
3. This article and the comments below are not a comparison or a place to bash either community or platform. Both have merit and strengths and weaknesses.

**Let's dive in.**

Getting an Alloy dev to "Hello World"
-----
This was kind of a trick header because if you are an Alloy developer and you went through the [NativeScript Getting Started Guide](http://docs.nativescript.org/start/getting-started) you have already accomplished a Hello World app in NativeScript and much more.

Folder structure
----

I am comparing the folder structures for an open source project I wrote called [MobileJS](http://mobilejs.io/). The project is a code comparison between NativeScript, React Native, and Titanium Alloy.

[**Alloy**](https://github.com/joshjensen/mobilejs/tree/master/titanium/alloy/app)

	- assets
	- controllers
	- lib
	- styles
	- views
	alloy.js
	config.json
	README
	widgets

[**{N}**](https://github.com/joshjensen/mobilejs/tree/master/ns/app)

	- App_Resources
	- fonts
	- todo
	- utils
	app.css
	app.js
	config.js
	LICENSE
	package.json

Immediately you will begin to see some similarities and differences. When you are working with {N} (NativeScript) the corresponding folder to assets in Alloy is "App_Resources". This folder will contain your platform specific resources like launch images, icons and what not.

On Alloy your app has 2 main places where you bootstrap the application from.
1. alloy.js and/or config.json
2. controllers/index.js

I personally, am not a huge fan of the `alloy.js` file in Alloy as it gives the all too tempting option for global scope pollution and memory leaks. I know why it was put in there, but like Stan Lee said, "With great power comes great responsibility".

To be clear `alloy.js` is just a commonJS module, but the idea that you can set something in one place and it's accessible automatically almost anywhere in the app, can be too tempting.

Here is my suggestion for Moving your alloy.js/config.json logic to a new home in {N}. Obviously there is no "Alloy" namespace so this is not a one to one conversion. However, in practice I would set up your shared logic like this.

1. Create a new folder called: `lib`
2. In the folder create a new file called: `shared.js` or `globals.js` or `config.js`
3. Now place your "globals", config object or objects in the `config.js` file for example:
```
module.exports = {
    colors: {
        white: '#fff',
        black: '#000',
        companyDefaultColor: '#333'
    }
}
```
4. Then later in your application simply require the config module for access to all of your configuration options.
```
var config = require('config.js');
var colorINeed = config.colors.companyDefaultColor;
```

What's the difference? *"Couldn't I just include my global file at the top of every controller and have the same issue?"* you ask. Very true, but creating a well defined structure for your data flow and application logic makes your app more resilient, easier to maintain, and can help manage memory leaks.

Controllers
----
Like Alloy, {N} uses a controller structure or what many {N} devs call "code behind" files. "Code behind" is a Microsoft term and will be unfamiliar to most Alloy devs or Javascript devs. As a crossover term we are going to use "controller" for the term that refers to the code that is used to interact with the view.

Start your engines
---
When you first start an Alloy app Alloy automatically looks for a file called `app/views/index.xml` and a file called `app/controllers/index.js`.

Those 2 files are where your application are bootstrapped from. You could, don't, but you could write your entire application in those 2 files. It would be ugly without your styles but you could do it. As I said, don't.

In my Alloy projects I almost always leave `index.xml` in its default state as `<Alloy></Alloy>`. Unless the project is so simple it only requires a couple of pages.

Then I use `app/controllers/index.js` as a launching off point for my application logic. For example:

	// ALLOY
	var application = require('application');
	application.start();

In Alloy that would have required a file called `application.js` in the `lib` folder and called the function "start".

{N} uses much the same logic. The application looks to `app.js` in the `app` folder.

	// {N}
	var application = require('application');
	
	application.mainModule = 'todo/list';
	application.cssFile = './app.css';
	
	application.start();

In {N} `application` is a core module unlike my Alloy example earlier. In this [example](https://github.com/joshjensen/mobilejs/blob/master/ns/app/app.js) we are setting the "mainModule" as `todo/list` and the application wide css file as `./app.css`.

[Check out the folder structure](https://github.com/joshjensen/mobilejs/tree/master/ns/app) you will see the `todo` folder as well as the `app.css` file.

`app.css` has the same functionality as `app.tss` in the `styles` folder of your Alloy project. `app.css` applies global styles across your entire application. Unlike Alloy you can name your `app.css` whatever you want. If you want to call it `legoMovie/wildStyle.css` just make sure you reference it properly in the `.cssFile` property before calling `application.start();`.

Views
----
In Alloy/Titanium Views are king, if you come from a web background some people look at it as nearly a one to one with html divs.

One of your Alloy XML files might look like this.

	<Alloy>
		<Window id="listWindow">
			<View id="headerWrapper">
	            <Label id="backButton" class="fa"></Label>
				<Label id="header">Todos</Label>
			</View>
			<View id="wrapper">
				<View id="formWrapper">
					<Label id="selectAllIcon" class="fa"></Label>
					<TextField id="textInput" />
				</View>
	      <View class="hborder" />
	      <TableView id="todoTable" />
			</View>
		</Window>
	</Alloy>

A `Window` with a bunch of `View`s in it. Then contained in those views would be UI elements. Sometimes views are used just for UI. Like a colored 1px divider or something like that.

In {N} this is a major departure from what an Alloy dev would be used to.

Here is an example of a page using {N}.

	<Page xmlns="http://www.nativescript.org/tns.xsd" navigatedTo="navigatedTo">
	  <DockLayout stretchLastChild="true">
	    <StackLayout dock="top" text="fill" id="wrapper">
	      <StackLayout>
	        <Label text="Todo" id="header" horizontalAlignment="stretch" />
	        <Label text="" id="backButton" cssClass="fa" horizontalAlignment="left" visibility="{{ showBack ? 'visible' : 'collapsed' }}" tap="onBackTap" />
	      </StackLayout>
	      <StackLayout orientation="horizontal" id="formWrapper" cssClass="fa">
	        <Label text="" id="selectAllIcon" tap="markAllAsDoneOnTap" />
	        <TextField text="{{ textInput }}" id="textInput" hint="What needs to be done?" />
	      </StackLayout>
	    </StackLayout>
	    <ListView items="{{ todoItems }}" id="listView" itemTap="rowOnPress" separatorColor="#fff">
	        <ListView.itemTemplate>
	            <StackLayout orientation="horizontal" cssClass="todo-row">
	                <Label text="{{ isChecked ? '' : '' }}" color="{{ iconColor }}" tap="onPressCheckbox" cssClass="fa checkbox"  />
	                <Label text="{{ text }}" />
	            </StackLayout>
	        </ListView.itemTemplate>
	    </ListView>
	  </DockLayout>
	</Page>

Notice the layouts, these are like views but with very specific purposes for layout. [Demystifying NativeScript Layouts](http://developer.telerik.com/featured/demystifying-nativescript-layouts/) is a great place to start to understand the paradigm shift when using layouts instead of views. Instead of `Window` you will notice the `Page` element. This page element wraps all of the other elements and is the foundation. Then, as we will talk about a little later, the page element has some lifecycle management callbacks that are very useful.

Once you are inside the layouts they will look very familiar with Labels, TextFields and other UI elements contained and laid out inside.

Classes and ID's
----
Alloy and {N} use classes very similarly. They are mostly, if not exclusively, for styling purposes. For example, referencing the classes in the associated TSS or CSS files.

However, Alloy relies heavily on ID's for referencing an element in the XML doc. There is no DOM so let's not get terms confused. In Alloy you would have a `Label` in your XML it would looks something like this: `<Label id="myLabel">Alloy</Label>`. To reference that label in your controller you could call `$.myLabel`. We could then, as an example, set the text to something else `$.myLabel.setText('Alloy!');`.

{N} definitely leans towards using a binding context for changing content or params. However, I will show you how to target an element using it's ID in {N} since that is such an integral part of the Alloy methodology. Please note: In {N} setting a parameter for an element is best done using a binding context but if you wanted to call focus() on a textInput, for example, then it would be appropriate to use the following method.

Let's use a text field in this example:

	<TextField text="{{ textInput }}" id="textInput" hint="What needs to be done?" />

In your controller you would have something like this:

    exports.onLoad = function(args) {
        var page = args.object;

        page.getViewById("textInput").focus();
    };

Note in the controller there is an exported function called onLoad. That function is related to the function name specified in the Page attributes.

	<Page xmlns="http://www.nativescript.org/tns.xsd" onLoad="onLoad">

There are a few such functions that {N} is looking for. The two I have used the most are `onLoad` and `navigatedTo`. What I have noticed is that `onLoad` is called anytime the page is loaded. `navigatedTo`, however, is called when the page has been (can you guess?) navigated to. So if the page is part of a navigation structure, this can be very useful because you can pass data down to the navigated to pages using this function. The data passed down from the parent is available in the `page.navigationContext` object. [You can see it's use here](https://github.com/joshjensen/mobilejs/blob/master/ns/app/todo/list.js#L39).

Data Binding
----

I am admittedly not a huge fan of data binding in Alloy. Some people love it. That is just not me. In Alloy I like to have very specific control over my content and application states. That said, it would be unfair to mention data binding in both Alloy and {N} because they are both very similar and very powerful.

To find out more about data binding:
1. [Alloy data binding](http://docs.appcelerator.com/platform/latest/#!/guide/Alloy_Data_Binding)
2. [{N} data binding](https://docs.nativescript.org/core-concepts/data-binding)

Data binding examples are outside the scope of this article but I will say this. When working with NativeScript it is tempting to look up your elements using ID's like in Alloy. Perhaps you are someone like me who does not use the data bindings in Alloy and therefore you are not comfortable trying them in {N}. I encourage you to give data binding in {N} a fresh try. Remember I talked earlier about moving the cheese and trying something new? This is one of those moments where it's especially good to try something new.

You will find that not only is {N}'s data binding performant, but also rather intuitive once you get used to it. If there is a substantial amount of demand, perhaps follow up article on data binding for Alloy devs.

Native Libraries
----
If you are an Alloy developer you know how to add a native library to your project. You either find the library has been turned into a TiModule for you, you do it yourself, or you pay someone to do it.

With the advent of HyperLoop you can have direct access to every iOS and Android API using JavaScript. In a very similar fashion {N} allows you to reference iOS and Android Native libraries directly from Javascript.

Just include the library and reference it's exposed APIs from your application logic.

Check out this blog post [Using Native Libraries in Your NativeScript Apps](http://nuvious.com/Blog/2015/4/5/using-native-libraries-in-your-nativescript-apps) for more information.

Or this core concept doc [Accessing Native APIs with Javascript](https://docs.nativescript.org/core-concepts/accessing-native-apis-with-javascript) for more information.


Last but not least
----
As I said at the beginning, "NativeScript and Titanium Alloy have very similar methodologies for structure and design concepts" hopefully this article will give you a launching off point for exploring a new side of Native Mobile Development using Javascript in NativeScript.

Check out [mobileJS.io](http://mobilejs.io/) and feel free to download the source and play around with the {N} and Alloy apps that are there. If you have any questions, please comment below.