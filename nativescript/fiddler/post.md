# View Your NativeScript App's Network Traffic with Fiddler

Fiddler is arguably the most popular network debugging proxy available today. And traditionally it's known as a proxy exclusively for desktop and web applications. However, in this tutorial, we will look at how you can set up a mobile device to use Fiddler as a proxy to capture network traffic.

Oh, did I mention that Fiddler is also completely free?

FIDDLER IMAGE

Let's take a look at how we can configure both iOS and Android devices to work alongside Fiddler, in much the same way as you would use it with a web or desktop app.

> **NOTE:** At the time of this writing, the latest version of iOS is 12 and Android is N (9.0). Future versions of these platforms may not work exactly the same as you see here!

## The Setup

It should go without saying that you'll need to have Fiddler installed on your desktop machine. Luckily Fiddler is now available for both macOS and Windows!

Next up, you'll want to make sure your desktop and mobile device can communicate with each other on the same network. Unfortunately this may or may not mean they just need to be on the same wifi network. Before you proceed to the next step, make sure you at least verify they can reach each other. The easiest way to do this is to discover the IP of your mobile device:

iOS

Android

Now, from your desktop machine, ping this IP address:

macOS

Windows

Success? Great! Proceed to the next section.

No luck? That's ok, you're not a failure. The next best solution is to disconnect your mobile device from the wifi network and enable a hotspot. Then connect your desktop to that hotspot.

> If you're on a desktop PC with no wifi, you may have luck connecting via USB. Check out [these instructions for iOS](https://www.gottabemobile.com/iphone-personal-hotspot-usb/) and [these for Android](https://www.makeuseof.com/tag/tethering-use-mobile-internet-pc/).

Again, you'll want to ping the device to make sure the two can communicate with each other.

## Fiddler Configuration

First, you should enable the Allow remote computers to connect setting in Fiddler

Open Fiddler and select Tools -> Options
Choose the Connections tab
Select the Allow remote computers to connect checkbox to enable the setting
Restart Fiddler in order the changes to take effect
Fiddler is now listening on port 8888 (this is the default port, you can change it from the setting above).



https://www.telerik.com/blogs/how-to-capture-android-traffic-with-fiddler

https://textslashplain.com/2016/07/27/using-fiddler-with-ios-10-and-android-7/