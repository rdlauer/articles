# Using the iOS Simulator from Windows

When NativeScript Sidekick was introduced, a door was opened for Windows developers to initiate, develop, build, and publish iOS apps without touching a Mac. With cloud build functionality and the ability to sideload an app on a connected iOS device, there really wasn't a need for a Mac.

Unless of course you wanted to test an app on the myriad of iOS devices out there. Do you still have your trusty iPhone 5 in a drawer? Are you lucky enough to have an iPad Pro on your desk? Maybe you do, but you don't want to go through the hassle of iOS device provisioning. This is where macOS developers are spoiled, as they have easy access to the iOS Simulator, installed as part of Xcode.

Why can't NativeScript developers on Windows utilize the same tools?

Let's see if we can make this work!

> Disclaimer: You will need a physical Mac on the same local network to make this work. While a Hackintosh may work (google it), what we are walking through requires a local Mac.

## Step One: Sidekick

I'm going to initiate a new NativeScript app in Sidekick and pick one of the starter app templates. Let's go with the Side Drawer template as that requires minimal UI work and frankly, I'm no designer.

IMAGE

With my template selected, I'm going to create the app on a local directory in Windows.

IMAGE

...and once the app is created we can move on to the next step.

## Step Two: Windows Share

To make this magic happen, we need to set up a new file share from Windows that our Mac can connect to. It's easy peasy folks.

Right-click on your project directory and choose "???".

IMAGE

{ instructions }

> Note: At this point you may need to check your Windows Firewall to make sure that TCP port 445 is open locally in order for the SMB protocol to work.

## Step Three: Connect from Mac

Time to switch gears and move over to your Mac.

> Tip: Make sure your Windows PC doesn't go to sleep during this process, as a sleepy PC won't work well as a file server! 😴

Using the "Go" menu, choose "Connect to Server...".

IMAGE

Enter either your Windows PC's name or its IP address, in the format of:

	smb://my-windows-box/file-share-name
	
You can find your Windows PC's name by right-clicking on the computer and selecting ???. Likewise, you can get your local IP address by running `ipcconfig` from a command prompt and grabbing the IPv4 IP address.

Hit "OK" and (drumroll 🥁) you should be provided with a username/password prompt. Enter the credentials of your local Windows account and voila, your NativeScript project directory should show up as a shared drive!

IMAGE

## Step Four: Cloud Build + LiveSync

TBD - will a local build work? Will a cloud build work and deploy to a local simulator?