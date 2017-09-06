# Fast Cloud Builds with NativeScript Forge

Last week we announced the first public beta of NativeScript Forge. This is a new desktop application (for Windows, Mac, and Linux) that simplifies your NativeScript development experience. So, to celebrate, this week is all about Forge on the NativeScript blog! Every day we will do a deep dive into some of the most valuable features of NativeScript Forge.

To kick things off, today we are going to look at the time-saving (and headache-reducing) cloud build feature of Forge. Let's get started!

## What's a Cloud Build?

Normally when you create the IPA file (for iOS) or APK file (for Android), you're using the native SDKs installed on your machine. For iOS you are using the Xcode tools and for Android you are using the Android SDK. The problem with building your apps locally is the amount of time it takes to download, install, and configure these tools. Is it doable? Absolutely.

But what if you didn't have to?

> We provide a full installation guide on setting up these dependencies as part of our getting started tutorials.

With NativeScript Forge you can leave SDK management to us. You simply tell Forge to build your app in our cloud and we will deliver you the IPA/APK app package back to you. Your NativeScript assets are secure in their delivery, and we don't story your data in the cloud.

Now typically cloud builds are considered much slower than local builds. This is natural, as local builds are running on your own hardware and files don't have to be transferred over the wire. However, Forge cloud builds are fast. Ridiculously fast. In fact, there is a chance that our cloud builds end up being **faster** than your local builds. Why? Forge builds are run on the latest Mac Pros with a fast network connect to make sure your files are uploaded, built, and downloaded as fast as possible.

In fact, as a "bake off", I compared the local build of one of the NativeScript starter kits to a cloud build. Here were my results:

Local iOS: xxx seconds -- Local Android: xxx seconds

Cloud iOS: xxx seconds -- Cloud Android: xxx seconds

## Can I Still Build Locally?

Yes, definitely. If you already have the appropriate SDKs set up locally, you can simply choose to perform a "local" build within Forge.


## CI Builds?


## How Do I Create a Build with Forge?


## What's Next for Cloud Builds?




Builds
•	Is there a way to integrate cloud builds with an existing CI infrastructure? I don’t see how that is possible with Forge, but maybe I’m missing something?
There is. As the infrastructure for online help, logging in and account management is not in place yet, we need to be careful with externally communicating it. In essence, this is what you need:

npm i -g nativescript
tns extension install nativescript-cloud
tns build cloud android
tns build cloud ios --provision <path to provision> --certificate <path to certificate> --certificatePassword <pass>
tns build cloud android --release --keyStorePath <path to certificate> --keyStorePassword <pass>
tns build cloud ios --release --provision <path to provision> --certificate <path to certificate> --certificatePassword <pass>
tns build cloud ios --emulator //for ios simulator
