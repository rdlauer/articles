# Securing a NativeScript App

I don't care if you are a web developer,  mobile app developer, desktop developer, or all of the above - security of your users, your user's data, and your intellectual property is paramount.

Remember the 90's? I (mostly) do. And I also remember in my consulting days running across issues like:

- Storing client passwords in clear text;
- Sending requests with SSNs in the query string;
- Accepting credit card payments without SSL enabled.

Good times!

I like to think we developers are far more aware of these issues, which is why you may be here today. When you develop a cross-platform mobile app with NativeScript, you are developing a truly native app. But that doesn't mean there aren't security considerations to be aware of!

Let's learn about securing a NativeScript app with, who else, but our friends from Silicon Valley.

IMAGE

We will start from the inside of an app binary and work our way out (or "middle out" in Silicon Valley parlance).

IMAGE

## Secure the Code

While NativeScript apps are truly native apps, they, like Cordova-based hybrid apps, do not pre-compile the app's XML/JS/CSS assets. This means your source code is exposed by default, which could allow malicious users to look into your app's plumbing to find vulnerabilities.

> Traditional native app developers aren't immune this either as it is incredibly easy to decompile both iOS and Android apps!

What do we do? Panic!? Nah. There is actually a lot we can do to help secure our NativeScript apps.

IMAGE

Let's start with obfuscation. This is a time-honored technique to make your code unreadable to human eyes. A popular obfuscation library, Uglify, can take legible JavaScript code like this:

CODE

...and turn it into this:

CODE

So that's one step. However, there are plenty of "beautification" services out there that will take uglified code and make it a little more legible. Using one of these services on the above code provided the following:

CODE

Ok, well, it's a start. But I think we can do better.

Enter JScrambler. JScrambler is a paid service that has proven compatibility with NativeScript and goes above and beyond plain obfuscation. JScrambler does x, y, and z. JScrambler can turn the following code:

CODE

...into this:

CODE

...which, when beautified, only gives you this:

CODE

THUMBS UP

## Secure Storage

You wouldn't trust Erlich with securely storing your stuff in his house, so why would you trust your app's operating system?

IMAGE

When storing data (of any type) on a device, you should prioritize the security of that data at rest. Luckily for all of us, the [nativescript-secure-storage](http://market.nativescript.org/plugins/nativescript-secure-storage) plugin exists!

Using the Secure Storage plugin allows us to securely save and retrieve key/value pairs in JavaScript/TypeScript:

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

## Leave Authentication to the Pros

Hopefully you don't have to roll your own authentication. Or...?

IMAGE?

Leave the heavy lifting of authenticating your app users to the OAuth 2.0 standard. By using the nativescript-oauth plugin, you get to take advantage of having an OAuth abstraction that makes authentication a snap.

Authenticating a user is as simple as calling the provided `login()` method, like so:

	import * as tnsOAuthModule from 'nativescript-oauth';
	...
	tnsOAuthModule.login()
	    .then(()=>{
	        console.log('logged in');
	        console.dir("accessToken " + tnsOAuthModule.accessToken());
	    })
	    .catch((er)=>{
	        //do something with the error
	    });

...which returns the user's `accessToken`.

## Enterprise MAM/MDM Options

A developer's life in the enterprise is all about security. Not only are you worried about the previously mentioned considerations, but...

If you are part of a big enough organization, there is a good chance that your company relies on Mobile App Management (MAM) or Mobile Device Management (MDM) software to help secure your internal apps and/or devices. That's a good thing, too, as with a MAM provider, like MobileIron, you are provided with an internal "enterprise" app store, so you don't have to even worry that an unauthorized third party has the ability to download your apps.

And it just so happens that there is a NativeScript plugin for MobileIron! By using the ??? plugin, you ...

IMAGE?

## Plugins to the Rescue

touch id, keychain, privacyscreen




## SSL All the Things

Since iOS ??? and Android ???, leveraging SSL is required by default for any external network calls. While occasionally inconvenient for developers, it's a great thing for end users to fall back on the fact that any data in transit is being encrypted. However, it doesn't explicitly prevent man-in-the-middle attacks! Yikes! Sounds scary!

A man-in-the-middle attack is...

Luckily for us, there is a NativeScript plugin to address that! The ??? plugin...

ssl pining plugin?

## Keep Secrets on the Server

If the app you are developing will be used in an online (or even occasionally online) state, you should strongly consider keeping as much of your app logic as possible on the server. Why?

- Your server-side code is a billion times easier to secure (trust me, I calculated the numbers);
- It's effectively impossible for end users to extract your IP from a server (aside from app functionality);
- By offloading your business logic to the server, you effectively decrease your app code footprint.

Now, if online/offline connectivity and data sync is an issue (and it's an issue for most of us), you do have options.

mobile baas
logic server side

## Summary

