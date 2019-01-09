# Secure Your Mobile App - Episode Two (Securing Data at Rest)

Whether you are developing a traditional native app, a cross-compiled app from the likes of Appcelerator or Xamarin, a hybrid app with Ionic, or a JavaScript-native app with NativeScript or React Native, a common thread that runs through each is app security.

> While you're here, but sure to [register for the upcoming webinar](https://www.progress.com/campaigns/kinvey/best-practices-for-securing-your-mobile-apps?utm_medium=social-owned&utm_source=blog&utm_campaign=kinvey-webinar-secureapps) on "Best Practices for Securing Your Mobile Apps", presented on January 23rd at 11AM ET.

More than ever before, I think we developers are far more aware of the myriad security issues we face. When you develop a cross-platform mobile app with NativeScript, you are developing a truly native app. But that also means the same security considerations apply as with any other native mobile app.

In [the previous article](), we dove into securing our source code via advanced obfuscation, preventing code tampering, reducing the scope of our installations, and migrating sensitive business logic to the cloud.

Today our focus in on how we store (and secure) data locally. So let's get to it!

- **Part One:** [Protecting Your Source Code]()
- **Part Two:** Securing Data at Rest (that's today!)
- **Part Three:** Ensuring Data Integrity Between Device and Server (coming Wednesday)
- **Part Four:** Enterprise User Authentication and Authorization (coming Thursday)

> Check out the new course from [NativeScripting.com](https://nativescripting.com/) on mobile app security and get 30% off with the code: NSSECURE.

## Simple Secure Storage

Out of the box both iOS and Android prevent data stored by one app to be accessed by any other app on the system. However, as we all know the road to hell is paved with good intentions, amirite? 🔥😰

So it's always best to encrypt any and all data we are saving to the device.

Luckily for us, the [nativescript-secure-storage](http://market.nativescript.org/plugins/nativescript-secure-storage) plugin exists!

The Secure Storage plugin allows us to encrypt, save, decrypt, and retrieve key/value pairs:

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
	
> **NOTE:** Internally, on iOS this plugin uses KeyChain via the [SAMKeychain library](https://github.com/soffes/SAMKeychain) and Android via the [Hawk library](https://github.com/orhanobut/hawk) (which in turn uses [Facebook conceal](https://github.com/facebook/conceal)).

## SQLite (with SQLCipher)

Are you a fan of [SQLite](https://www.sqlite.org/index.html)? Did you know there is a [full-featured NativeScript plugin](https://market.nativescript.org/plugins/nativescript-sqlite) that supports SQLite? Well now you do!

The free version (above) of the SQLite plugin provides all of the capabilities you've come to expect from SQLite. However, there is a [paid option](https://nativescript.tools/product/10) that also includes encrypting your SQLite database at rest. By leveraging [SQLCipher](https://www.zetetic.net/sqlcipher/), you can have transparent 256-bit AES encryption of your SQLite database on your user devices.

![sqlite and sqlcipher](2-sqlcipher.png)

## Offline Data Sync with Encryption

Many of us use mobile backend services (mBaaS) like [Firebase](https://market.nativescript.org/plugins/nativescript-plugin-firebase) or [Progress Kinvey](https://www.progress.com/kinvey) for our remote backends. And when developing mobile apps, we need to be aware of online/offline connectivity, and syncing up data for when users toggle between those states (lest the app crashes without a network connection!).

Out of the box, Kinvey comes with [online/offline data sync](https://devcenter.kinvey.com/nativescript/guides/datastore) baked in, as outlined in this example from the docs:

	// Retrieve an instance
	const dataStore = Kinvey.DataStore.collection('books', Kinvey.DataStoreType.Sync) as Kinvey.SyncStore;
	// Pull data from the backend and save it locally on the device.
	const promise = dataStore.pull()
	  .then((entities: Array<{}>) => {
	    // ...
	  })
	  .catch((error: Kinvey.BaseError) => {
	    // ...
	  });
	// Find data locally on the device.
	const subscription = dataStore.find()
	  .subscribe((data: Array<{}>) => {
	    // Called once, with local data
	  }, (error: Kinvey.BaseError) => {
	    // ...
	  }, () => {
	    // Called after the local data has been retrieved
	  });
	// Save an entity locally to the device. This will add the item to the sync table to be pushed to the backend at a later time.
	const entity = {};
	const promise = dataStore.save(entity)
	  .then((entity: {}) => {
	    // ...
	  })
	  .catch((error: Kinvey.BaseError) => {
	    // ...
	  });
	// Syncs this store with the backend. This will first push any pending changes on the device to the backend and then pull data from the backend onto the device.
	const promise = dataStore.sync()
	  .then((entities: Array<{}>) => {
	    // result will contain the results of the push to the backend and a pull from the backend
	    // result = {
	    //   push: [], // pushed entities
	    //   pull: [] // pulled entities
	    // };
	    //
	    // Each item in the array of pushed entities will look like the following
	    // { _id: '<entity id before push>', entity: <entity after push> }
	    // It could also possibly have an error property if the push failed.
	    // { _id: '<entity id before push>', entity: <entity after push>, error: <reason push failed> }
	  })
	  .catch((error: Kinvey.BaseError) => {
	    // ...
	  });

Additionally, [Kinvey provides for encryption of the data](https://devcenter.kinvey.com/nativescript/guides/encryption) at rest on the device, using SQLite and SQLCipher!

	Kinvey.init({
		appKey: '<appKey>',
		appSecret: '<appSecret>',
		encryptionKey: '<encryptionKey>'
	});

## Backend Compliance and Security

![hipaa compliance and kinvey](2-hipaa.png)

Many of us who develop apps in the enterprise are acutely aware of compliance and security regulations. Here in the US, a big one for developers creating apps for health care providers or insurance companies is [HIPAA](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act).

> If you are developing an app that deals with PHI (private health information) in the health care industry, you absolutely need to make sure the data storage and transmission of said data is HIPAA-compliant.

Kinvey reviews, affirms, and evolves security controls on an annual basis via SOC2, HIPAA, GDPR, Sarbanes-Oxley, and other compliance activities. For customers in banking focused on FFIEC or GLBA regulations, in health care focused on HIPAA, or doing business in the EU and concerned about GDPR, the Kinvey platform provides comprehensive end-to-end security with the capabilities needed to support your regulatory compliance efforts. [Read more here](https://www.progress.com/kinvey/enterprise-security) about how Kinvey can provide your organization the security and compliance coverage it requires.

## Next Up is Episode Three: Securing Data in Transit!

Today we covered storing private data elements securely in our app and even looked into some local and remote secure data storage options. Next up we are going to look into how we *securely transfer* data back and forth from the client to the server. Hint: it's not *quite* as simple as SSL. 🤔

> Don't forget to [register for our mobile app security webinar](https://www.progress.com/campaigns/kinvey/best-practices-for-securing-your-mobile-apps?utm_medium=social-owned&utm_source=blog&utm_campaign=kinvey-webinar-secureapps) coming up on January 23rd!