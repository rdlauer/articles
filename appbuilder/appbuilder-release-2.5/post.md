## Telerik AppBuilder September Release: Xcode iOS Simulator Access, Verified Plugins Marketplace Integration, and More

We have another exciting AppBuilder release to share: this time we are focusing on rounding out our native emulator story with access to the Xcode iOS Simulator (yes, even from Windows!), full integration with our new [Verified Plugins Marketplace](http://plugins.telerik.com/), Cordova plugin variable management, and numerous productivity enhancements.

For the complete details, read on! Be sure to also consult our [release notes](http://docs.telerik.com/platform/appbuilder/release-notes/v2-5) for the most detailed explanation of what is new.

### Xcode iOS Simulator Access

In our [last release](http://blogs.telerik.com/appbuilder/posts/14-07-31/telerik-appbuilder-7-31-14-release-native-emulator-support-bower-package-manager-and-new-project-templates) we provided you with the ability to test your mobile apps directly on any Android or Windows Phone 8 native emulators you have installed. This release completes the story with providing you access to the Xcode iOS Simulator from our Windows client, Visual Studio extension, and CLI on the Mac. [Consult our docs](http://docs.telerik.com/platform/appbuilder/testing-your-app/running-in-emulators/ios-emulator) for the full details.

> Be sure to check out the Telerik Developer Network and the articles we have on getting started with the [Android emulators](http://developer.telerik.com/featured/using-android-emulator-hybrid-mobile-apps-telerik-appbuilder/) and [Windows Phone 8 emulator](http://developer.telerik.com/featured/using-windows-phone-8-emulator-hybrid-mobile-apps-telerik-appbuilder/).

### Verified Plugins Marketplace Integration

If you haven't heard of it yet, a couple of months ago we released our [Verified Plugins Marketplace](http://plugins.telerik.com/). This is a curated set of Cordova/PhoneGap plugins that have been verified, tested, and documented to work with AppBuilder (and hybrid mobile apps in general). Today I'm pleased to announce that you can install these plugins directly from inside the AppBuilder IDEs!

![verified plugins marketplace integration](plugins_25.png)

We are now making custom plugins easier than ever to discover, download, and install by providing point and click methods for extending your apps with more native functionality. **Try it out today by using AppBuilder's Package Manager**, which also allows you to switch between versions of Kendo UI and download and maintain [Bower](http://bower.io/) packages.

### New "Quick Start" Tutorial

Some of you may have noticed our new quick start tutorial that helps you get up and running developing a new hybrid mobile app with AppBuilder. If you haven't used it yet, it's worth checking out (even for experienced developers) as it is quick and you will most likely learn something new about AppBuilder and our other Telerik Platform services!

> Current subscribers can access the quick start lessons from the "Getting started" menu option.

![quick start tutorial](quickstart_25.png)

### Cordova Plugin Variable Management

Speaking of Cordova plugins, in this release we are also providing an easy to use UI to manage your Cordova plugin variables. These are variables that are normally edited in the individual plugin's `plugin.xml` document. These can be difficult to discover and easy to miss, so we have pulled this functionality out for you and now provide a clean UI to manage these variables.

![cordova plugin variable management](pluginvars_25.gif)

### Device Simulator Improvements

We've also significantly enhanced the plumbing of our device simulator - and in future releases you will see how much better this amazing tool can get! In the meantime, we now allow you to create app screenshots right from within the simulator, add support for more core Cordova plugins, and watch variables in the debugging tools.

### What Else?

Another big request from our [AppBuilder extension for Visual Studio](http://www.telerik.com/appbuilder/visual-studio-extension) users is to improve our integration with the default Visual Studio themes. This release cleans up AppBuilder and now all of the themes are applied correctly. We've also provided [security updates for Cordova 3.5.1 on Android](http://blogs.telerik.com/appbuilder/posts/14-09-14/telerik-appbuilder-hotfix-for-cordova-3.5-on-android), improved the filtering of our Bower packages to only show you packages that are relevant to mobile development, and have redesigned numerous dialog boxes.

### What's Next?

Be sure to [consult our recently updated roadmap](http://www.telerik.com/support/whats-new/appbuilder/roadmap) to stay current on our plans for AppBuilder. Also, [sound off on our feedback portal](http://feedback.telerik.com/Project/129) if there are important features you think we are missing!