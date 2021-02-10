# Top 5 Kinvey Features Developers Adore - Part Two: Kinvey Studio

Helping developers to be successful with our products is a core component of the Developer Relations life here at Progress. Every tool we offer - from OpenEdge, to Sitefinity, to Telerik UI, to Kendo UI, to NativeScript - each is focused on developer productivity.

Let's add Progress Kinvey to that mix! Kinvey adds significant value to the backend, just as our UI suites add value to the front end. Kinvey provides a secure, robust, performant serverless platform on which developers can create more engaging systems. This leads to more engaging apps, with increased customer satisfaction and retention.

In this five-part blog series, we will look at some specific features of Kinvey that our marketing materials may normally overlook. That's because these features appeal directly to the developer, the user of Kinvey (which, as we know, is not always the buyer, right enterprise developers!).

- Part One: FlexServices
- Part Two: Kinvey Studio
- Part Three: Mobile Identity Connect
- Part Four: Data Connectors + SDKs
- Part Five: Kinvey Chat

Today we are going to focus on the low code, but high control features provided by Kinvey Studio. Low code? High control? Let's first dive into those terms.

## Low Code vs High Control

Historically, systems that classified themselves as "low code" generally meant one thing: inflexible. This is because they often relied on proprietary systems of incomprehensible meta data that generated code that, while machine-readible, was barely legible by developers (and rarely editable). This meant a tool you were attached to for life. There was no "export" feature, and the thought of round-tripping from tool to code to tool again was incomprehensible.

This put the concept of low code at odds with "high control". When we say "high control", we are focusing on the concept of developers having full control over the apps they write. They don't want to *just* click through some screens to create an app, they want to *see* the code generated (maybe for their own peace of mind, or maybe to edit that same code to add some unique customizations).

This is where Kinvey Studio comes into play. Kinvey Studio is a low code/high productivity tool, that *also* allows for high control. If it sounds like the best of both worlds, well, it certainly is!

GIF

## Complexity of Modern Apps

Today though, app development needs are becoming more complex, with more screens needed across more distinct platforms. And today's definition of an "app" oftentimes means:

- a responsive website
- a Progresive Web App
- a native iOS app
- a native Android app
- a cross-platform chatbot
- a wearable on Android Wear OS and/or Apple Watch

![multiple devices](multiple-devices.png)

Along with increased complexity comes an increased need for tooling that can keep up with today's needs. This is where Kinvey Studio comes in.

## Why Kinvey Studio?

If you're looking for an elevator pitch for Kinvey Studio, it goes something like this:

*"Kinvey Studio enables professional developers to build cross-platform mobile, web, chat, and wearable apps, leveraging a secure and performant backend, from a shared JavaScript codebase."*

![kinvey studio](kinvey-studio-web.gif)

Let's break down that sentence:

- **"Professional Developers"** - Kinvey Studio is not necessarily geared towards the "citizen developer". This is a tool that increases productivity (and generates clean code) for pro developers. All files are stored locally, and freely editable in your IDE of choice.
- **"Cross-Platform"** - Kinvey Studio focuses on creating less code in less time, and creating it for virtually any screen size and platform.
- **"Mobile/Web/Chat/Wearable"** - And yes, this means creating cross-device experiences, enabling you to meet your customers where they are, not where you want them to be.
- **"Shared Codebase"**: Apps built with Kinvey Studio utilize modern code sharing strategies across platforms, including a common shared backend.

## Built on Solid Foundations

With a name like "Kinvey Studio", it should be no surprise that the product itself relies on the mature foundations provided by the [Progress Kinvey](https://www.progress.com/kinvey/) serverless backend. But Kinvey Studio goes far beyond the backend and builds upon on a variety of mature and performant libraries and frameworks. For instance:

- Native mobile apps are built on top of [NativeScript](https://www.nativescript.org/) (of course!) plus [Angular](https://angular.io/)
- Web apps and PWAs are built with components from [Kendo UI](https://www.telerik.com/kendo-ui) (with Angular as well)
- Chatbots are created with [Kinvey Chat](https://www.progress.com/kinvey/chat)

## Kinvey Studio Features

While the image above gives a good sense of some the visual drag-and-drop capabilities of Kinvey Studio, there is a lot more to it than that.

### Building Your Mobile UI

When approaching an app with Kinvey Studio, we want to think about the app as a set of screens, or views. Each view is a set of distinct functionality comprised of native UI elements. Maybe it's a read-only display of data, a list with a master/detail relation, a customer service chatbot, or just a blank canvas to customize as-needed.

![mobile view templates](mobile-view-templates.png)

A key component of Kinvey Studio are the (growing) set of pre-defined views that are **powered by your data**, and save a dramatic amount of time that would normally be spent tediously scaffolding.

For example, let's say I have a backend collection of data that I simply want to expose via a dynamic listview with images. In Kinvey Studio the steps would be:

1. Create a new view from an existing template;
2. Link that view to an existing set of data;
3. Preview the native app on a real device!

...illustrated in-action here:

![kinvey studio mobile view](kinvey-studio-mobile-view.gif)

### Building a Custom Mobile UI

Using the pre-defined view templates is great...when your business case matches one of those templates! For other unique scenarios we recommend using the **Blank** view option.

![kinvey studio mobile blank canvas](kinvey-studio-mobile-blank.png)

This is where the power of Kinvey Studio really shines. The blank canvas allows you to completely customize your user interface. You can drag-and-drop UI elements (like labels, text fields, sliders, switches, and so on). You can also drag-and-drop *layout containers*, which are how you *arrange* your native UI elements on the canvas.

> Since Kinvey Studio uses [NativeScript](https://www.nativescript.org/) behind the scenes, anything you need to learn about layout containers can be [found in the NativeScript docs](https://docs.nativescript.org/ui/layouts/layout-containers)!

Need a view that shows an `Image`, a `Label`, and a `Button`? To do so, I would:

1. Start with dragging a `Stack Layout` container on to my canvas
2. Then drag an `Image`, `Label`, and `Button` elements over, one at a time.

...leading to the following UI:

![kinvey studio blank canvas](kinvey-studio-blank.png)

Which, keep in mind, **took us all of five seconds!**

> While we are focusing on the *visual* aspect of Kinvey Studio today, you should also know that the generated code is stored locally. This also means that it is editable by your favorite text editor or IDE! Kinvey Studio supports *roundtrip editing* of an app, from Studio to IDE, and back again.

### Navigation Options

Clearly there are a lot of time-saving capabilities involved with creating the UI of your mobile app. Another common stumbling block is **app navigation**. Whether you want to use mobile-optimized tab- or drawer-based navigation, Kinvey Studio has you covered:

![mobile navigation](mobile-navigation.png)

By simply including, excluding, or re-ordering the array of views provided, you can quickly create a fully-functional navigation system for your app in no time.

### Themes

It's one thing to build an app. It's another to make that app **actually look good**.

Kinvey Studio mobile apps with support for the [NativeScript core theme](https://docs.nativescript.org/ui/theme) out of the box. The advantages of using this theme include:

- Access to the existing class-based application of different UI styles;
- A variety of color schemes to choose from;
- An easy way to tweak and override the theme defaults with CSS.

Within Kinvey Studio, there is a **Themes** section that allows you to edit both your web and mobile themes. You can quickly experiment with a variety of pre-built color schemes:

![mobile theme](mobile-theme.png)

> And remember, since mobile apps built with Kinvey Studio rely on the NativeScript framework, this means that apps are styled with CSS, the same CSS you and your developers have been using for years.

### Settings and Configurations

Are you a big fan of hand-editing your `info.plist` and `AndroidManifest.xml` files to configure your mobile apps? I didn't think so.

Kinvey Studio provides an intuitive UI on top of these iOS and Android configuration files, making sense out of such arcane code blocks as:

	<activity
		android:name="com.tns.NativeScriptActivity"
		android:label="@string/title_activity_kimera"
		android:configChanges="keyboardHidden|orientation|screenSize"
		android:theme="@style/LaunchScreenTheme">

		<meta-data android:name="SET_THEME_ON_LAUNCH" android:resource="@style/AppTheme" />

		<intent-filter>
			<action android:name="android.intent.action.MAIN" />
			<category android:name="android.intent.category.LAUNCHER" />
		</intent-filter>
	</activity>

For example, does your app need to access certain device permissions on Android? Instead of editing your `AndroidManifest.xml` file, you can use the interface provided to point and click your way to success:

![android permissions](android-permissions.png)