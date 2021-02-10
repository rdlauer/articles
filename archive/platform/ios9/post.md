## Telerik Platform and iOS 9

Another year has flown by and we have yet another major iOS update to contend with! On Wednesday, September 16th, Apple is expected to release iOS 9 - the next iteration of their immensely popular mobile OS. What does this mean for hybrid and native mobile developers who are leveraging the [Telerik Platform](http://www.telerik.com/platform)? Read on to find out!

![ios 9 logo](ios9-logo.png)

### NativeScript

One of the core benefits of leveraging [NativeScript](https://www.nativescript.org/) for your native mobile app development needs is the promise of day zero support for new operating systems. **This is still the case today as we support the entire set of iOS 9 APIs!**

Specifically, you can check out some existing blog posts related to [deep linking in iOS 9](https://www.nativescript.org/blog/deep-linking-your-nativescripts-apps-with-ios-9-user-activity-and-core-spotlight-apis) and another on [building a widget for the iOS notification center](https://www.nativescript.org/blog/making-a-today-widget-in-ios-with-nativescript-and-ui-for-nativescript).

### Hybrid (Cordova/PhoneGap)

Hybrid developers specifically need to know about some breaking changes that Apple introduced with iOS 9:

- Users of our [WKWebView plugin](http://plugins.telerik.com/cordova/plugin/wkwebview) should upgrade their apps to the latest version which provides full support for iOS 9
- Tried our new [Apple Watch plugin yet](http://developer.telerik.com/featured/7-steps-to-building-a-hybrid-apple-watch-app/)? We made some tweaks to ensure compatibility with iOS 9
- Users of Angular/Ionic have reported a critical bug that [requires a fix](http://stackoverflow.com/questions/31234087/ionic-issues-with-ios-9-beta)

### App Transport Security

What is this ATS thing? If you've heard the buzz about [App Transport Security](http://ste.vn/2015/06/10/configuring-app-transport-security-ios-9-osx-10-11/), what you really need to know is that iOS 9 requires that all of your remote endpoints be accessed securely with the HTTPS protocol. This is a great best practice and you should definitely implement this going forward.

In the meantime, a quick workaround is to add the following key to your info.plist file:

	<key>NSAppTransportSecurity</key> 
	<dict>
	    <key>NSAllowsArbitraryLoads</key> <true/> 
	</dict>

> Here are the [instructions](http://docs.telerik.com/platform/appbuilder/configuring-your-project/edit-configuration) for editing these configuration files in Telerik AppBuilder.

### Looking Ahead

Here at Telerik we are actively testing our cloud-based build servers with the latest SDKs from Apple and plan on officially releasing iOS 9 support in a coming release. In the meantime, rest assured if you follow the above advice, your existing apps will work just fine on this new OS!