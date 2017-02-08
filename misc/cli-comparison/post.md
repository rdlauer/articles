## Hybrid Mobile App Command-Line Smackdown

The proliferation of command-line tooling has exploded over the last few years - and hybrid mobile developer tools have been no exception. Where once we were downloading PhoneGap 1.2 and installing Xcode templates to develop our iOS apps, today we have platform-agnostic tools and utilities to help us fulfill the promise of cross-platform mobile app development. Today we have choices - and to help provide some guidance, I present you with a high-level comparison of three of the most popular command-line interfaces (CLIs) available today for hybrid mobile development.

### The Competitors

The most popular hybrid mobile framework today is clearly [Apache Cordova](http://cordova.apache.org/), and two of our tooling options utilize the Cordova (a.k.a. PhoneGap) framework (those being the [Cordova CLI](https://cordova.apache.org/docs/en/3.5.0/guide_cli_index.md.html#The%20Command-Line%20Interface) and the [Telerik AppBuilder CLI](http://www.telerik.com/appbuilder)). Our third option is [Steroids](http://www.appgyver.com/steroids) from AppGyver, a competing hybrid framework that maintains compatibility with the Cordova plugin architecture. You may be thinking, "why not include the PhoneGap CLI?". This is because the PhoneGap CLI and the Cordova CLI are nearly feature-identical. The main difference is that you would use the PhoneGap CLI if you want to create cloud-based builds with [PhoneGap Build](https://build.phonegap.com/).

### The Rules of the Game

I am going to break down all of the CLIs and compare them across ten different categories such as: installation experience, testing/debugging options, deployment capabilities, app store publishing, and more.

Let's get started!

*Disclaimer: I am the Product Manager for Telerik AppBuilder. However, I have used the Cordova CLI and Steroids extensively to research this post, and will do my best to give each product a fair shake!*

### Installation

*How easy is it to get up and running with the CLIs from scratch? What installation dependencies are there that may affect my experience?*

**AppBuilder CLI**

The only pre-installation dependency for the AppBuilder CLI is [Node.js 0.10.22 or later](http://nodejs.org/) (32-bit if installing on Windows). If you are deploying to a physical device you'll also want: iTunes for iOS development, drivers for your Android devices, and/or the Windows Phone 8 SDK.

With Node.js installed, simply run this command to install the AppBuilder CLI from your command prompt/terminal: `npm install appbuilder -g` (add `sudo` if installing on a Mac). The installation process will install any additional dependencies for you automatically.

**Cordova CLI**

Cordova requires you to install the appropriate SDKs for each platform you wish to develop with before installing the CLI. For example, if you are developing for iOS, you have to be on a Mac and install Xcode's command line tools (luckily this is all [well documented](https://cordova.apache.org/docs/en/3.0.0/guide_platforms_index.md.html#Platform%20Guides) and, while slightly annoying, not too difficult).

Next up you'll need Node.js and a [git client](http://git-scm.com/downloads) installed.

Finally, simply run this command to install the Cordova CLI: `npm install cordova -g` (add `sudo` if installing on a Mac).

**Steroids CLI**

AppGyver has a fantastic online installation wizard to walk you through all of the steps required to install the Steroids CLI - the downside is there are quite a few dependencies you have to take care of yourself:

- Install a git client
- Install [Python](https://www.python.org/downloads/)
- Install Xcode + the Xcode command-line tools
- Install Node.js
- Install Node Version Manager (NVM)
- Finally, install Steroids CLI using: `npm install steroids -g`

**Advantage: AppBuilder CLI.** With fewer dependencies to install manually you'll get up and running most quickly with AppBuilder.

### Mobile Platform Support

*Which mobile platforms can I target with these CLIs?*

**AppBuilder CLI**

AppBuilder supports the three most popular mobile platforms available today: iOS, Android, and Windows Phone 8.

**Cordova CLI**

Cordova supports numerous mobile platforms, including: iOS, Android, Windows Phone 8, Amazon Fire OS, Firefox OS, Tizen, Blackberry 10, Ubuntu, and Windows 8.

**Steroids CLI**

Steroids supports the iOS and Android platforms, which realistically cover the vast majority of our mobile development needs.

**Advantage: Cordova CLI.** With incredibly broad platform support, it's tough to match the Cordova CLI.

### Project Creation

*How easy is it for me to initiate a new hybrid mobile project? Are there any tricks to adding cross-platform compatibility?*

**AppBuilder CLI**

To start a new project with the AppBuilder CLI, use the `appbuilder create hybrid [project name]` command (you can optionally specify an app id in reverse domain notation - i.e. `com.mycompany.myproject`). This will create a working directory with some pre-configured project assets and you'll be ready to go.

You also have the option of using one of the AppBuilder sample apps as a starter template. Try running `appbuilder sample` to get a list of available apps and `appbuilder sample clone` to clone one of the sample apps and start a new project.

**Cordova CLI**

Similar to the AppBuilder CLI, the Cordova CLI allows you to initiate a hybrid project with a simple command: `cordova create [app id] [app name]`. This creates the directory structure for your app. Unlike AppBuilder, though, you do need to run one more command for each platform you'd like to support. Navigate to your newly-created project directory in your command prompt/terminal and run: `cordova platform add [platform]` (where "[platform]" is ios, android, wp8, etc).

**Steroids CLI**

Just like our other CLI options, the Steroids CLI allows us to run one simple command: `steroids create [app name]` to generate a new project. Like the AppBuilder CLI, it will create a working directory for you with some HTML, JS, and CSS assets ready for you to use.

**Advantage: Tie between AppBuilder and Steroids.** Both make it *slightly* more simple than the Cordova CLI to get up and running since they don't require a separate command to add individual platforms.

### IDE Integrations

*CLIs are great, but what about integrating with our current development tools? Do any of the CLIs work well with commonly used IDEs for my code editing experience?*

**AppBuilder CLI**

Available as a [separate download](http://www.telerik.com/appbuilder/sublime-text-package), the AppBuilder CLI provides an integration with Sublime Text. This extension to Sublime Text allows you to build and deploy to connected devices, enable LiveSync (allowing saved changes to automatically display on your devices), and run the app in the AppBuilder device simulator.

**Cordova CLI**

As part of Apache, the Cordova CLI doesn't officially support any single proprietary IDE or integration. However, Raymond Camden has a nice blog post on [integrating the Cordova CLI with Brackets](http://www.raymondcamden.com/2014/7/11/First-release-of-Cordova-Brackets-extension) that is certainly worth reading.

**Steroids CLI**

The Steroids CLI doesn't provide direct integration with any specific IDE. However, AppGyver recently released a tool called [Composer](http://www.appgyver.com/composer), which lets you visually construct a mobile app using the [Ionic framework](http://ionicframework.com/). You can then export your app from Composer and use the Steroids CLI to generate a build of your app.

**Advantage: Tie Between AppBuilder CLI and Steroids CLI.** AppBuilder's Sublime Text package is a direct integration while AppGyver's Composer is more of a separate design tool (as opposed to an extension of Steroids). It's an apples-to-oranges comparison, but both provide significant added value.

### Built-In Frameworks

*What is my getting started experience like? Are any JavaScript frameworks pre-installed to give me a head start on developing my mobile app?*

**AppBuilder CLI**

Since AppBuilder comes from Telerik (makers of the open source JavaScript framework [Kendo UI](http://www.telerik.com/kendo-ui)), when you create a new project from scratch with the CLI, your starter app includes a copy of Kendo UI Core. This makes your getting started experience that much easier (and if necessary it's easy enough to remove the associated Kendo UI assets and use your framework of choice).

**Cordova CLI**

The Cordova team does not officially endorse one JavaScript framework over another, so none are included by default. However, in their [getting started docs](http://cordova.apache.org/docs/en/3.5.0/guide_next_index.md.html#Best%20Practices) they do conveniently provide a list of some of the most popular SPA (single page app) frameworks for you to get started.

**Steroids CLI**

One of the biggest advantages in using the Steroids CLI is that you can easily take advantage of the Steroids JavaScript framework. The Steroids framework allows you to create an MPA (multi-page app) with true native animations and transitions. If you initiate your app using AppGyver's Composer tool, you also can take advantage of the Angular-based Ionic framework mentioned earlier.

**Advantage: Steroids CLI (by a hair).** Steroids is well worth checking out if you want to take advantage of native transitions and animations. Otherwise stick with the AppBuilder CLI + Kendo UI combination.

### Testing and Debugging

*How do I test and debug my hybrid app as I'm developing it? What options do I have as far as previewing the app without running it on a physical device?*

**AppBuilder CLI**

AppBuilder has always had a very robust and accurate device simulator. This simulator is available on Mac and Windows and provides an accurate representation of a real device for iOS, Android, and Windows Phone 8. Accessible via the CLI by running `appbuilder simulate`, the simulator also allows you to utilize your familiar Chrome dev tools to inspect elements, set breakpoints, check for network latency issues, profile your app, and examine the console.

**Cordova CLI**

While there are no testing tools or simulators that come bundled with the Cordova CLI, you can (and should) use the [Ripple emulator](https://chrome.google.com/webstore/detail/ripple-emulator-beta/geelfhphabnejjhdalkjhgipohgpdnoc?hl=en) from Blackberry. Ripple allows you to simulate a variety of devices and sizes, is open source, and available free of charge. Since you're running Ripple in Chrome, you can also take advantage of your browser's dev tools.

There is no explicit CLI command to execute the Ripple emulator, you simply start it and point it at your project. The Cordova CLI does, however, let you access any native emulators you may have running on your local machine. For example, you can run `cordova emulate ios` to run your project in the native iOS Simulator (you must also install [ios-sim](https://github.com/phonegap/ios-sim) first).

**Steroids CLI**

As with the Cordova CLI, you may use the Ripple emulator with your Steroids project. Beyond that, testing and debugging is currently limited to the native iOS Simulator on a Mac (there is no Android testing option available). To access this, simply run `steroids connect` to start the Steroids server and then `simulate`. This will open up the iOS Simulator and allow you to test your iOS app.

**Advantage: Tie between AppBuilder CLI and Cordova CLI.** The AppBuilder CLI has the most comprehensive device simulator on the market, but the ability to access native emulators is a great boost to your Cordova CLI workflow.

### Mobile Device Accessory Apps

*Is there a way for me to remotely distribute an app to my testers? How about quickly running an app on a device without going through the entire deployment process?*

**AppBuilder CLI**

The [AppBuilder Companion App](http://www.telerik.com/appbuilder/companion-app) is available for [iOS](https://itunes.apple.com/bg/app/icenium-ion/id527547398?mt=8), [Android](https://play.google.com/store/apps/details?id=com.telerik.AppBuilder&hl=en), and [Windows Phone 8](http://www.windowsphone.com/en-us/store/app/appbuilder/0171d46b-b5f2-43d9-a36b-0a78c9692aab). Available for free from all of the public app stores, the Companion App allows you to test and share your apps on real devices without worrying about the hassle of deploying to a physical device (read that: no iOS provisioning profiles required!). You can install your app via QR code (run `appbuilder build [platform] --companion` to generate a QR code to install on a device). You can also utilize LiveSync to have saved changes show up in the Companion App on any connected devices.

**Cordova CLI**

The [PhoneGap Developer App](http://app.phonegap.com/) is a mobile app for [iOS](https://itunes.apple.com/app/id843536693), [Android](https://play.google.com/store/apps/details?id=com.adobe.phonegap.app), and [Windows Phone 8](http://www.windowsphone.com/en-us/store/app/phonegap-developer/5c6a2d1e-4fad-4bf8-aaf7-71380cc84fe3) that allows you to run one or more Cordova apps inside of it. Like the AppBuilder Companion App, it provides about as close of an on-device experience as you can ask for.

To get your Cordova app running in the PhoneGap Developer app run `cordova serve`, open up your Developer App and enter the IP address of your local server. When your CLI is paired up with your Developer App you'll see your app running on the device.

**Steroids CLI**

AppGyver also has a great mobile app for [iOS](https://itunes.apple.com/us/app/appgyver-scanner/id575076515?mt=8) and [Android](https://play.google.com/store/apps/details?id=com.appgyver.android&hl=en) called Scanner. This app works in much the same way as the AppBuilder Companion Apps and the PhoneGap Developer App. To get an app into Scanner you simply scan a QR code. Very easy to use, convenient, and apps run quickly. (The only downside are compatibility issues with Android 4.4.)

Simply run the `steroids deploy` command to create a local build of your app for Scanner.

**Advantage: Tie.** Considering the similar feature sets and capabilities, it's essentially a tie between all three implementations.

### Generating Builds

*How easily can I create actual native builds of my apps using the CLIs?*

**AppBuilder CLI**

AppBuilder makes it pretty easy with the `appbuilder build [platform]` command which builds your app in the cloud (so yes, you can even create an iOS build on Windows or a Windows Phone build from a Mac). There are, of course, platform-specific dependencies to take into account. For example, with iOS, you can use the `--certificate` and `--provision` options to specify the certificate and provisioning profiles you want to use to sign your app. Run `appbuilder build` to get a full list of available features.

**Cordova CLI**

Cordova allows you to create local builds of your apps, provided you have the appropriate SDKs installed. For example, to create an iOS build, you may run: `cordova build ios`. If, however, you would like to do cloud-based builds like AppBuilder, you'll have to use the nearly-identical [PhoneGap CLI](http://phonegap.com/install/) and create your builds with [PhoneGap Build](https://build.phonegap.com/).

**Steroids CLI**

Steroids makes it easy to create cloud-based builds of your apps by issuing the `steroids deploy` command. From the [AppGyver Cloud portal](https://build.appgyver.com/) you may then download your iOS and Android builds. (This is also the same way you create a build to run in the Scanner app.)

**Advantage: AppBuilder CLI.** The ability to generate cloud-based builds of iOS, Android, and Windows Phone 8 apps - regardless of your development platform - is a huge advantage (no local SDKs are required). However, it's important to note that subscribers of PhoneGap Build can achieve a similar outcome.

### Publishing to App Stores

*I've developed and tested my app, now I'd like to deliver it to a public app store. What options do I have?*

**AppBuilder CLI**

The AppBuilder CLI requires you to publish your apps yourself to the Google Play and Windows Phone Marketplace sites (use the `appbuilder build` command to generate your builds). However, since publishing an iOS app to the app store requires a Mac, the AppBuilder CLI offers the `appbuilder appstore upload` command - which builds and delivers your app to iTunes Connect (even if you're on Windows).

**Cordova CLI**

The Cordova CLI does not provide integration with any of the public app stores. You may, however, create your builds locally (see "Generating Builds" section), and upload them manually.

**Steroids CLI**

Steroids is similar to Cordova CLI in that there are no direct integrations with the public app stores. As with the Cordova CLI, you may create your builds, download them from your AppGyver Cloud portal, and upload them to the appropriate app stores yourself.

**Advantage: AppBuilder CLI.** The ability to publish directly to iTunes Connect from Windows is a big bonus. If you're on a Mac this feature doesn't make a huge difference though.

### Open Source and Cost

*Are these tools open source? What are the costs associated with these CLI tools and associated services?*

**AppBuilder CLI**

The AppBuilder CLI itself is [open source](https://github.com/Icenium/icenium-cli) (Apache 2). The services that support the CLI (like the simulator and cloud build services) do require a valid [AppBuilder subscription](http://www.telerik.com/purchase/appbuilder). Start with a 30 day free trial and pricing starts as low as $19/month after that.

**Cordova CLI**

The Cordova CLI is [open source](https://github.com/apache/cordova-cli) (Apache 2) and completely free to use. If you want to extend the Cordova CLI to use cloud-based build services, the smartest route is to go with PhoneGap Build which offers a [variety of free and paid plans](https://build.phonegap.com/).

**Steroids CLI**

The core AppGyver Platform is free as well and Steroids is [open source](https://github.com/AppGyver/steroids) (MIT). AppGyver does offer significant [managed add-ons and cloud services](http://www.appgyver.com/steroids/pricing) that are fee-based, but can add a lot of value to your development experience.

**Advantage: Cordova CLI.** You can't beat 100% free! But you do get what you pay for - with Steroids you get some great add-ons and with AppBuilder you get access to some significant time-saving development tools and services.

### And the Winner Is...

If we total up the "winners" from each category, the Telerik AppBuilder CLI is the victor. However, there are two major caveats to this scoring model:

- Not every CLI feature has equal importance to every developer or project, so some categories may be more important to you than others.
- I have more knowledge of the AppBuilder CLI, so readers should judge my inherent bias accordingly (and sound off in the comments if I have misspoken!).

All of these CLI tools allow you to develop hybrid mobile apps faster and better than ever before. Choose the tool that best fits your style and budget. You have many choices today as a mobile developer, more than any other time before, so hopefully this helps to guide you on the best path for your hybrid mobile development experience.