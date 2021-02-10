# Creating Your First Native Mobile App with Visual Studio (Part 2)

[**&lt;-- Read Part 1**](#)

In the first post of this series, we started our "Bill Murray" mobile app with [NativeScript's](https://www.nativescript.org/) tab strip TypeScript template in Visual Studio, using [Telerik AppBuilder](http://www.telerik.com/platform/appbuilder). Now it's time to write some code to create a truly native, cross-platform app, with 100% code re-use between our iOS and Android apps. By the end of this article we hope to:

- Learn about NativeScript layouts;
- Build our app structure using native UI elements;
- Mock in some data to show off a fully-functional app;
- Debug our app with Visual Studio debugging tools.

Let's get started!

## NativeScript Layouts

The XML syntax behind NativeScript layouts will be very familiar to those of you who have used XAML-based layouts in the past. For example:

- XAML has `<Grid>` and NativeScript has `<GridLayout>`
- XAML has `<StackPanel>` and NativeScript has `<StackLayout>`
- XAML has `<DockPanel>` and NativeScript has `<DockLayout>`
- XAML has `<WrapPanel>` and NativeScript has `<WrapLayout>`
- XAML has `<Canvas>` and NativeScript has `<AbsoluteLayout>`

Of course both XAML and NativeScript [have more layout options than just these](https://docs.nativescript.org/ui/layouts#layouts), but you get the idea.

> Tip: Read more about layouts in this article on ["Demystifying NativeScript Layouts"](http://developer.telerik.com/featured/demystifying-nativescript-layouts/)

If you recall from last time, our simple Bill Murray app is going to have one page with a `TabView` containing two views of data. One tab is going to show quotes and images from some of his movies and the second will show a pie chart breaking down the quotes by movie.

The first view will need the ability to show a random quote (we need a `<Label>` element to display a string) and an image of Bill Murray (we need an `<Image>` element to display an image). We will want the UI elements stacked on each other, so we will use a `<StackLayout>` element.

Open up `main-page.xml` and replace the current contents with this:

	<Page xmlns="http://schemas.nativescript.org/tns.xsd" loaded="pageLoaded">
	  <TabView>
	    <TabView.items>
	      <TabViewItem title="Quotes">
	        <TabViewItem.view>
	          <StackLayout class="tab-content" id="layout">
	            <Label text="{{ quote }}" class="title" textWrap="true" />
	            <Image src="{{ image }}" />
	          </StackLayout>
	        </TabViewItem.view>
	      </TabViewItem>
	      <TabViewItem title="Chart">
	        <TabViewItem.view>
	          <Label text="Chart Placeholder" class="title" />
	        </TabViewItem.view>
	      </TabViewItem>
	    </TabView.items>
	  </TabView>
	</Page>

> Tip: Tabs can also be [dynamically generated](https://docs.nativescript.org/cookbook/ui/tab-view) at runtime, but for simplicity sake we are going to do everything with XML.

Remember, to see your changes at any time you can use the **Build {app name} in Cloud** function to build for the Telerik Platform Companion App. If you've already deployed an app build to the Companion App, you can use the **Synchronize {app name} with Cloud** function and do a **three-finger tap** (iOS) or **tap the "sync" function in the notification menu** (Android) to load your changes.

You may be asking yourself, "what are those {{ }} curly braces all about?". This is commonly referred to as "mustache syntax" and is a way for us to implement [data binding](https://docs.nativescript.org/core-concepts/data-binding) in our app. Which makes now a great time to start building out our code behind!

> You can find the complete source code for this NativeScript app in [this Github repository](https://github.com/rdlauer/tns-bill-murray).

## Code Behind and Mock Data

The brains of our app will be in the `main-page.ts` code behind file (we know it's the code behind file for `main-page.xml` simply because of the file name match). Let's start working on our app's business logic and mock in some sample data so we can get a real-world idea of how our app will function.

Go ahead and replace everything in your `main-page.ts` file with this:

	import observable = require("data/observable");
	import pages = require("ui/page");
	
	export function pageLoaded(args: observable.EventData) {
	
	    let page = <pages.Page>args.object;
	    let myLayout = page.getViewById("layout");
	
	    myLayout.bindingContext = {
	        quote: "Cinderella story. Outta nowhere. A former greenskeeper, now, about to become the Masters champion.",
	        image: "https://www.fillmurray.com/200/302"
	    };
	
	}

You can see we've made some small changes to our `pageLoaded` function, which is called when this view is first loaded (see `loaded="pageLoaded"` in `main-page.xml`). We are also binding mock data to our `StackLayout` element (you can see how we get the reference to the `StackLayout` as we select the element by its id with `page.getViewById("layout")`).

> We will be using [fillmurray.com](https://www.fillmurray.com/) to load remote images of Bill!

You'll notice the corresponding `text="{{ quote }}"` and `src="{{ image }}"` properties in our UI elements match up to the `quote` and `image` properties we define when we bind our data.

Once these changes are in place, use the **Synchronize {app name} with Cloud** menu option and do a **three-finger press** (iOS) or **tap the "sync" function** (Android) to load your changes. You should see something like this on both iOS and Android:

![first look at bill murray app ios](part-2-first-look.png) ![first look at bill murray app android](part-2-first-look-android.png)

Yay! We have a very simple, cross-platform native app, running on both iOS and Android platforms with 100% code re-use.

**But we can do better!**

## Additional UI Elements

We will spend some time prettying it up later, but for now, let's make two small UI changes:

- Add an `ActionBar` to display the title;
- Add a reload button to display a new quote/image.

It's very easy to add an `ActionBar` to our app. Between the opening `<Page>` element and the opening `<TabView>` element, paste this:

	<ActionBar title="Bill Murray Quotes" />

This will give you a nice little [ActionBar](http://docs.nativescript.org/ui/action-bar) that looks like this on iOS and Android:

![action bar ios](part-2-actionbar.png) ![action bar android](part-2-actionbar-android.png)

Next, let's add a reload button to our `ActionBar`. We will do this by adding an [ActionItem](http://docs.nativescript.org/ui/action-bar#action-items) to our `ActionBar` element:

	<ActionItem tap="onReload" ios.systemIcon="13" ios.position="right" android.systemIcon="ic_menu_rotate" android.position="actionBar" />

In this code snippet you'll notice some iOS- and Android-specific properties. This is the first example of how you can make changes to your app, in code, that are only visible on the appropriate platforms.

> Tip: Take a look at this article on [platform-specific development with NativeScript](http://developer.telerik.com/products/nativescript/platform-specific-development-nativescript/).

**What properties did we add to our `ActionItem` element?**

- An `onReload` function that is fired when the icon is tapped;
- Icons from the `systemIcon` sets on both iOS and Android (see iOS and Android [system icon options](https://docs.nativescript.org/ui/action-bar#setting-icons));
- Positioning the iOS icon to the *right* of the action bar title, and Android positioning *on* the action bar (just some platform-specific quirks!).

Of course, we don't have a corresponding `onReload` function set up yet, but just for the sake of the UI, your `<ActionBar>` element should now look like this in `main-page.xml`:

	<Page xmlns="http://schemas.nativescript.org/tns.xsd" loaded="pageLoaded">
	  <ActionBar title="Bill Murray Quotes">
	    <ActionItem tap="onReload" ios.systemIcon="13" ios.position="right" android.systemIcon="ic_menu_rotate" android.position="actionBar" />
	  </ActionBar>
	  <TabView>
	  ...

Which results in the following updated `ActionBar` UI in iOS and Android:

![reload in action bar ios](part-2-reload.png) ![reload in action bar android](part-2-reload-android.png)

## Handing Errors in the Companion App

It's inevitable. Even using TypeScript, every once in a while we will make a mistake and that mistake might not be visible until we actually deploy the app to our device.

Error handling for NativeScript apps in the Telerik Platform Companion App is not quite perfect. Sometimes you do a LiveSync and your app just straight up crashes! If this is the case, chances are very good that you just introduced an error. The easiest way to discover the error and resolve the problem is to:

1. Kill and re-open the NativeScript Developer App on your device;
2. Discover the error with the console log output provided (see iOS example below);
3. Fix the problem, tap **Reset** to completely reset your NativeScript Developer App, and start over with a new build.

![error in companion app](part-2-error.png)

> One way to help avoid errors is by [writing unit tests](http://developer.telerik.com/products/nativescript/adding-unit-tests-to-your-nativescript-app/). NativeScript has built-in support for Jasmine, Mocha, and QUnit.

If you are experiencing issues with your app, instead of trying to debug in the Telerik Platform Companion App, you should really take advantage of Visual Studio's powerful debugging features.

## Debugging NativeScript  in Visual Studio

Visual Studio isn't just a text editor. It includes expansive debugging tooling that we can tap into when developing for iOS or Android with NativeScript.

Before we start debugging on a physical device, take a quick look at the [requirements for debugging on device](http://docs.telerik.com/platform/appbuilder/nativescript/debugging-your-code/debugging-on-device/prerequisites-for-debugging). Nothing too crazy in here as the default TypeScript settings in our app already conform to the requirements.

We could write a whole article on debugging an app, but for right now, I'm going to debug on a physical Android device (thereby avoiding the provisioning hassle of iOS).

> Tip: Check out the specific docs for debugging on [Android](http://docs.telerik.com/platform/appbuilder/nativescript/debugging-your-code/debugging-on-device/debug-on-android-device) and [iOS](http://docs.telerik.com/platform/appbuilder/nativescript/debugging-your-code/debugging-on-device/debug-on-ios-device) as there are some gotchas for both platforms.

Since I'm debugging on Android, I'll want to make sure my [developer options system setting is available](https://www.howtogeek.com/129728/how-to-access-the-developer-options-menu-and-enable-usb-debugging-on-android-4.2/) and that USB debugging is also enabled. Next, with my device connected to my PC, I'll do the following to debug my app:

1. From the **APPBUILDER** menu, choose **Build {app name} and Deploy** (this will build the app in the cloud and deploy it to a connected device).
2. Next, choose the device, make sure the **Debug** configuration is selected, and click **Run**.
3. Once the app is running on your device, open up a TypeScript (.ts) file and in the margin of the code editor, click next to a line number to set a breakpoint.
4. In the Visual Studio toolbar, choose your device and click the magical green arrow to start debugging!
 
![visual studio debug toolbar](part-2-debug-menu.png)

You can now set more breakpoints, use step-through debugging, and fully debug your app with the built-in Visual Studio debugger!

![debugging nativescript with visual studio](part-2-debug.png)

## What's Next?

While our app UI isn't *quite* complete yet, now is a good time for us to move on to work on our backend! In the next article in this series, we will put NativeScript aside and focus on creating a fully functional backend with SQL Server and a RESTful service built with ASP.NET Web API. Once that is done, we will put the finishing touches on our app with some CSS styling, native animations, and a quick look at [Telerik UI for NativeScript](http://www.telerik.com/nativescript-ui)!

[**Read Part 3 --&gt;**](#)