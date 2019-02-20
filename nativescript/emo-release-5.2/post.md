# NativeScript 5.2 Comes with Official Support for Vue.js

**NativeScript 5.2 is here!** Read on for a full list of features and improvements that are now part of NativeScript.

## Vue.js Official Support

[Vue.js](https://vuejs.org/) users were the fastest growing part of the NativeScript community over the past year. As most of you probably remember, the [NativeScript-Vue](https://nativescript-vue.org/) integration started as a community effort by [Igor Randjelovic](https://twitter.com/igor_randj) and turned out to be a huge success. For the past month (and a trend over the past year), NativeScript-Vue applications are the second most frequently created project types in our ecosystem, surpassing the "Pure JavaScript" and "Pure TypeScript" projects by far.

![nativescript and vue.js](nativescript-and-vue.png)

This drove the team to the decision to announce **official support for Vue.js** in NativeScript with the 5.2 release. This was the second-most voted feature in the last Community Survey from the entire NativeScript community. *This sounds great, but what does it mean in practice?*

- **Feature parity** for new functionality between Vue.js, Angular, and Core frameworks. This means that every new feature that we add, we consider Vue.js support to be as important as any other flavor.  
- **Plugin compatibility** with Vue.js. For the past couple of releases, we spent a good amount of effort to make all plugins developed and owned by NativeScript usable in NativeScript-Vue projects. All of our plugins now have a demo app that shows how the functionality is used with Vue.js. Also, to be able to provide the expected quality, every plugin is covered with Vue.js-specific tests that verify that the component behaves as expected. Finally, the marketplace has two new badges for every plugin that indicate whether the respective plugin is compatible with Vue.js or/and Angular. This is part of our effort to provide transparency to plugin users for what to expect from the components that they plan to use and an incentive for plugin authors to provide support in their plugins for both frameworks. 
- For those projects that require a **mission critical support** – the [NativeScript Enterprise Support](https://www.nativescript.org/enterprise) offering now covers NativeScript-Vue projects as well as all the other flavors. This offering complements all the other free support channels that are backed by the NativeScript comminuty: [NativeScript Community Slack](https://nativescriptcommunity.slack.com/) where the #vue channel is one of the most active in the entire workspace and [StackOverflow](https://stackoverflow.com/questions/tagged/nativescript). 
- The NativeScript Engineering Team will be **involved in the maintenance and the development of the Vue.js integration**. This means that we will address issues in the NativeScript-Vue project according to priorities of the items compared to everything else in our backlog. 
- Webinar and content creation covering Vue.js projects. You can expect an increased number of webinars, blog posts, and documentation articles that aim to help you develop your next killer NativeScript app based on Vue.js.

## Hot Module Replacement Improvements

NativeScript is continuing its effort to provide a **fast and fluent development experience**, especially when iterating over the development-test-debug cycle. For 5.2 we are bringing a series of improvements in the [Hot Module Replacement](https://www.nativescript.org/blog/nativescript-hot-module-replacement) (HMR) functionality that will make development a breeze. Here is a a full list of the changes in this release:

- HMR now handles changes in [SASS](https://sass-lang.com/) files. 
- Changes in `app.css` are applied without restarting the app. You can see how this works by watching me destroying my details template with bright colors 😂:

<iframe width="560" height="315" src="https://www.youtube.com/embed/jZytviKjHO0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- HMR works out of the box for [Angular](https://www.nativescript.org/nativescript-is-how-you-build-native-mobile-apps-with-angular) projects 
- `tns debug` now supports the `--hmr` flag and you can apply changes without interrupting the debugging session started in your [Chrome DevTools](https://docs.nativescript.org/tooling/debugging/chrome-devtools). Support for debugging with HMR in [Visual Studio Code](https://www.nativescript.org/nativescript-for-visual-studio-code) is coming in the next release. Meanwhile, check out this basic debugging session, in which I try to fix the total goal count for the players:

<iframe width="560" height="315" src="https://www.youtube.com/embed/3aKvhRPXekg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Error Logging Improvements

One of the toughest jobs after your mobile app is published in the stores and goes viral is monitoring the app’s health and troubleshooting problems that users are experiencing. This has been a pain point, as the default tooling provided in the App Store and the Google Play Store doesn’t play well with NativeScript apps, and same goes for error reporting services like [Crashlytics](http://try.crashlytics.com/) for example. 

Up to now only events that were causing crashes of the application had been reported. This was only a small subset of all exceptions that might happen in your application, and sometimes important information might not get to the error-reporting services. With this release, an exception will also be reported and it’s very easy to enable it:

	var application = require("application"); 
	
	application.on(application.discardedErrorEvent, function (args) { 
	    console.log(error.message); 
	    console.log(error.stackTrace); 
	    console.log(error.nativeException); 
	    //report the exception in your analytics solution here 
	});

## markingMode: none is Now an Officially Supported Option

As announced in the [5.1 release](https://www.nativescript.org/blog/just-in-time-for-nativescript-5.1), we are on a continued effort to provide compatibility for the `markingMode: none` option. For this this release, we were focused on the community plugins and providing support for plugin authors to make their plugins compatible with this option. You can check out the [markingMode: none](https://docs.nativescript.org/core-concepts/android-runtime/advanced-topics/marking-mode-none) article and a blog post on the matter is coming shortly. 

With this release, the `markingMode: none` option is an officially supported mode for the Android runtime. We encourage all of you to try it out and let us know if any problems arise, especially in projects that are having performance issues on Android. Feedback from users who have already tried it out has been very positive and we are looking for even more projects benefiting from the performance boost.

## Native Debugging Toolkit

In our team we have employed several practices for easier development of native code, both in plugins and applications. Recently, we decided it will be useful to share these with the whole community. Those are now packaged as a NativeScript dev plugin called [nativescript-dev-debugging](https://market.nativescript.org/plugins/nativescript-dev-debugging).

After an initial configuration, the plugin will offer you several workflows that will ease your work when it comes to developing and debugging native code. A blog post that walks through these workflows will come out shortly.

## Consuming 3rd Party Libraries Directly in iOS

One feature of the NativeScript framework that we are very proud of is the **100% access to native code**. This allows for high developer productivity and maximum reuse of existing materials and documentation for native libraries. Up to now there was one significant exception – namely consuming 3rd party native libraries (Pods) in iOS directly from the application. This was a design decision taken in the early days of the framework, which aimed to stimulate developers to create plugins and to share them with the community.

The [NativeScript Marketplace](https://market.nativescript.org/) now offers **more than 1000 plugins** and we feel that this constraint is no longer needed. That’s why with 5.2, the team decided to remove this restriction and allow for even greater flexibility and productivity when a native code development is needed. Now you can **develop against native libraries without any overhead** – just drop them in the `App_Resouces/iOS` folder and start writing your code.

## NativeScript Appium Integration

The [nativescript-dev-appium](https://market.nativescript.org/plugins/nativescript-dev-appium) package provides an integration between [Appium](https://appium.io/) and NativeScript to provide the capability to build an extensive and reliable testing infrastructure. This package has been instrumental for ensuring the quality of the NativeScript framework itself and has been used by many NativeScript users to ensure the quality of their apps.  

Recently, a new major version was released with the following improvements: 

- no need to provide capabilities configurations for local development, which should make local test development a lot easier 
- support code sharing projects and [Jasmine](https://jasmine.github.io/) (up to now only [Mocha](https://mochajs.org/) was supported)
- compatibility with latest NativeScript and latest Appium releases
- bug fixes 

Next, our efforts will be focused on preparing extensive documentation and tutorials on how to make most of this functionality.

## Other Updates

- Docs UI migration - for legacy reasons the [documentation of the NativeScript UI components](https://docs.nativescript.org/ui/professional-ui-components/overview) (like Charts, SideDrawer, RadListView and so on) was a separate website with different look and feel and structure. This caused a lot of confusion and inconvenience when working with these components. With this release we are merging the NativeScript UI documentation into the main docs.nativescript.org website. 
- [Open JDK](https://openjdk.java.net/) Support – Since January 2019, Java SE is [no longer free for commercial usage](https://java.com/en/download/release_notice.jsp). NativeScript requires the JDK as part of the Android native setup and to accommodate this change, we are switching to Open JDK - which is a free and open-source implementation. This doesn’t mean that something will break in your setup and if you have obtained a valid commercial license for the Oracle JDK you can continue to use it. However, moving forward our testing infrastructure will be focused mainly on validating that the framework is working with Open JDK. 
- [V8 engine](https://v8.dev/) updated to 7.1 
- New [Xcode](https://developer.apple.com/xcode/) build system compatibility - Xcode 10 was released with a [new build system](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_release_notes/build_system_release_notes_for_xcode_10). Up until now, NativeScript was working against the old one due to some issues. With this release, those issues were addressed and we are using the latest build system.