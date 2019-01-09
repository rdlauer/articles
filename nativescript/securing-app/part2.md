# Secure Your Mobile App - Part Two (Data at Rest)

Whether you are developing a traditional native app, a cross-compiled app from the likes of Appcelerator or Xamarin, a hybrid app with Ionic, or a JavaScript-native app with NativeScript or React Native, a common thread that runs through each is app security.

> While you're here, but sure to [register for the upcoming webinar](https://www.progress.com/campaigns/kinvey/best-practices-for-securing-your-mobile-apps?utm_medium=social-owned&utm_source=blog&utm_campaign=kinvey-webinar-secureapps) on "Best Practices for Securing Your Mobile Apps", presented on January 23rd at 11AM ET.

I like to think we developers are far more aware of the myriad security issues than ever before, which is why you may be here today. When you develop a cross-platform mobile app with NativeScript, you are developing a truly native app. But that also means the same security considerations apply as with any other native mobile app.

In [the previous article](), we dove into securing our source code via advanced obfuscation, preventing code tampering, reducing the scope of our installations, and migrating sensitive business logic to the cloud.

- **Part One:** Protecting Your Source Code (that's this one!)
- **Part Two:** Securing Data at Rest (coming Tuesday)
- **Part Three:** Ensuring Data Integrity Between Device and Server (coming Wednesday)
- **Part Four:** User Authentication and Authorization (coming Thursday)

Today our focus in on how we store (and secure) data locally. So let's get to it!

> Check out the new course from [NativeScripting.com](https://nativescripting.com/) on mobile app security and get 30% off with the code: NSSECURE.

## Simple Secure Storage

Out of the box both iOS and Android prevent data stored by one app be accessed by any other app on the system. However, as we all know the road to hell is paved with good intentions, amirite?

So it's always best to *securely store* any and all data we are saving on the device.

Luckily for all of us, the [nativescript-secure-storage](http://market.nativescript.org/plugins/nativescript-secure-storage) plugin exists!

The Secure Storage plugin allows us to securely save and retrieve key/value pairs:

	// require the plugin
	import { SecureStorage } from "nativescript-secure-storage";
	
	// instantiate the plugin
	let secureStorage = new SecureStorage();
	
	// async
	secureStorage.set({
	  key: "foo",
	  value: "I was set at " + new Date()
	}).then(success => console.log("Successfully set a value? " + success));
	
	// sync
	const success = secureStorage.setSync({
	  key: "foo",
	  value: "I was set at " + new Date()
	});
	
??? how does the above relate to Keychain ???

For most of us, the Secure Storage plugin will get us most of the way towards securing any sensitive user information on our device. However, it is only a first step, and **not nearly enough if you are developing a banking/finance/healthcare app!**

## SQLite with SQL Cipher?

## Offlin Data Encryption

If you're looking for a more robust backend solution, Progress Kinvey may be just the solution. Kinvey provides ...

online/offline data sync

## HIPAA Compliance

If you're developing an app for a healthcare provider or insurance company - or even if you are managing protected patient data - it is extremely likely your app and backend provider be HIPAA compliant.

> Read this article on what HIPAA means for mobile app development.

Did you know that Kinvey is the *only* HIPAA-compliant backend provider on the market today? With <insert compliance stuff> Kinvey is easily the most secure option you have, providing turnkey solutions and reducing the risks that companies deal with on the daily.

## Next Up: Part Three!

Today we covered storing private data elements securely in our app and even looked into some local and remote secure data storage options. Next up we are going to look into how we *securely transfer* data back and forth from the client to the server. Hint: it's not *quite* as simple as SSL.

> Don't forget to register for our mobile app security webinar coming up on January 23rd!