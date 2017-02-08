## Get Ready for 64-Bit with Telerik AppBuilder

Back in October, Apple announced that [starting on February 1st, 2015](https://developer.apple.com/news/?id=10202014a), new apps submitted to the App Store must be built with the iOS 8 SDK and include 64-bit support. Also, as of June 1st, 2015, updates to existing apps will need to follow these same requirements. Rest assured AppBuilder's cloud-based build services will be ready well ahead of these dates.

### Cloud Builds

One of the biggest advantages with using AppBuilder is the fact that you don't have to manage the iOS/Android/WP8 SDKs yourself. Since our builds run in the cloud, we upgrade our build servers for you and the process is completely transparent. **It's important for you to know that 64-bit builds are our top priority for the next release!**

### Your Call to Action

There are two things you should do to make sure your iOS apps are ready for 64-bit:

- **Make sure you are using Cordova version 3.5 or greater.** (Check out the "How Do I Upgrade Cordova?" section of [this post](http://blogs.telerik.com/appbuilder/posts/14-09-17/telerik-appbuilder-hotfix-for-cordova-3.5-on-android).)
- **Verify that any custom plugins you are using also have 64-bit support included.** (You'll know if there are any non-64-bit plugins if you generate a build after we update our build servers and receive an error that the arm64 architecture is missing.)

### Key Dates

- **January, 2015** - iOS build servers upgraded to the latest version of the iOS SDK (providing you with 64-bit builds automatically).
- **February 1st, 2015** - New iOS apps must be submitted with 64-bit support.
- **June 1st, 2015** - Upgraded iOS apps must be submitted with 64-bit support.

Again, other than the two tasks above, there is nothing you need to do to take advantage of this new build support. This is why at Telerik, we love the cloud!