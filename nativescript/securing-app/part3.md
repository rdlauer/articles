# Secure Your Mobile App - Episode Three (Securing Data in Transit)

Whether you are developing a traditional native app, a cross-compiled app from the likes of Appcelerator or Xamarin, a hybrid app with Ionic, or a JavaScript-native app with NativeScript or React Native, a common thread that runs through each is app security.

> While you're here, but sure to [register for the upcoming webinar](https://www.progress.com/campaigns/kinvey/best-practices-for-securing-your-mobile-apps?utm_medium=social-owned&utm_source=blog&utm_campaign=kinvey-webinar-secureapps) on "Best Practices for Securing Your Mobile Apps", presented on January 23rd at 11AM ET.

Last time, we looked at [securing the data we store on the device]() - whether it's via encrypted key/value storage, SQLite + SQLCipher, or a robust and compliant backend like [Progress Kinvey](https://www.progress.com/kinvey) for encryption and online/offline data sync.

Preserving the integrity of your app's data as it moves back and forth to and from your backend is another critical piece of this puzzle, so today our focus in on how we protect and secure data while in transit.

- **Part One:** [Protecting Your Source Code]()
- **Part Two:** [Securing Data at Rest]()
- **Part Three:** Ensuring Data Integrity Between Device and Server (that's today!)
- **Part Four:** Enterprise User Authentication and Authorization (coming Thursday)

> Check out the new course from [NativeScripting.com](https://nativescripting.com/) on mobile app security and get 30% off with the code: NSSECURE.

## SSL Everywhere

![encrypt all the things](3-encrypt-all.png)

### iOS

Introduced with iOS 9, App Transport Security (ATS) is a default feature that enforces increased security within iOS apps. When your iOS app makes an external connection, that connection *must* meet the following requirements:

- The server must support at least Transport Layer Security (TLS) protocol version 1.2;
- Connection ciphers are limited to those that provide forward secrecy;
- Certs must be signed using a SHA256 (or greater) signature hash algorithm;
- Invalid certificates result in a hard failure and no connection.

This is great for developers, as we are forced into our iOS apps communicating over secure channels by default. However, there is still a way around this, which I'm pointing out here as something you should **not** add to your `info.plist`:

	<key>NSAppTransportSecurity</key>
	<dict>
	    <key>NSAllowsArbitraryLoads</key>
	    <true/>
	    <key>NSExceptionDomains</key>
	    <dict>
	        <key>example.com</key>
	        <dict>
	            <key>NSExceptionAllowsInsecureHTTPLoads</key>
	            <true/>
	            <key>NSIncludesSubdomains</key>
	            <true/>
	        </dict>
	    </dict>
	</dict>

Setting `NSAllowsArbitraryLoads` to true allows for loading any remote resources, regardless of the means of transfer. Again, please don't do this. 😀

### Android

The most recent version of Android ([Pie or 9.0](https://www.android.com/versions/pie-9-0/)) is a bit behind Apple, but does default to blocking HTTP traffic in apps by default.

This requirement will apply to all apps that target Android 9, but, like with iOS, will require a specific declaration in the app's `AndroidManifest.xml` file if any non-secure HTTP connections are needed thought the [network security configuration](https://developer.android.com/training/articles/security-config) options. Like with iOS, please don't do this 😀.

So Rule #1 today, the simplest rule of them all, is to make sure that literally every remote call you make (I don't care if it's to an image or a remote endpoint) is performed over SSL.

## Preventing Man-in-the-Middle Attacks

Leveraging SSL is critical when transferring data, but just hitting an `https` endpoint doesn't necessarily guarantee security. This is where the dreaded "man-in-the-middle" attack comes into play.

A man-in-the-middle attack is a situation where someone secretly and transparently relays and possibly alters the communication between two parties who believe they are directly communicating with each other.

![man-in-the-middle attack](3-man-in-the-middle-attack.png)

Clearly this is a major issue when we are talking about ensuring the integrity of data in transit - and the solution to this is to use a concept known as SSL pinning.

Luckily for us, there is a NativeScript plugin to address just this scenario! The [nativescript-https](https://market.nativescript.org/plugins/nativescript-https) plugin is a drop-in replacement for the [http module](https://docs.nativescript.org/ns-framework-modules/http).

To enable SSL pinning with this plugin, you'll want to [install the SSL certificate](https://market.nativescript.org/plugins/nativescript-https#installing-your-ssl-certificate) and enable pinning in code:

	import { knownFolders } from 'file-system'
	import * as Https from 'nativescript-https'
	let dir = knownFolders.currentApp().getFolder('certs')
	let certificate = dir.getFile('wegossipapp.com.cer').path
	Https.enableSSLPinning({ host: 'wegossipapp.com', certificate })

## End-to-End Encryption

Clearly the best solution for securing your data from the device to your backend is a fully integrated solution. This is where, again, [Progress Kinvey](https://www.progress.com/kinvey) comes into play.

![progress kinvey logo](3-kinvey-logo.png)

As noted in the [previous article](), with a feature-complete [NativeScript SDK](https://devcenter.kinvey.com/nativescript), Kinvey can encrypt data at rest on the device, protect the integrity of your data in transit, and secure your data in the cloud!

## Last, But Not Least, is Episode Four: Secure Identity Management

Our final article in this series focuses on a very common app scenario: securely authenticating and authorizing your users with existing providers and biometric security options.

> Don't forget to [register for our mobile app security webinar](https://www.progress.com/campaigns/kinvey/best-practices-for-securing-your-mobile-apps?utm_medium=social-owned&utm_source=blog&utm_campaign=kinvey-webinar-secureapps) coming up on January 23rd!