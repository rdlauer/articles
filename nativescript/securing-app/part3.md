# Secure Your Mobile App - Part Three (Data in Transit)

Whether you are developing a traditional native app, a cross-compiled app from the likes of Appcelerator or Xamarin, a hybrid app with Ionic, or a JavaScript-native app with NativeScript or React Native, a common thread that runs through each is app security.

> While you're here, be sure to register for the upcoming webinar on ???, presented on January 23rd at 11AM ET.

Last time, we looked at securing the data we store on the device - whether it's via a simple encrypted key/value storage, SQLite, or a robust and compliant backend like Progress Kinvey for encryption and online/offline data sync.

Preserving the integrity of your app's data as it moves back and forth to/from your backend is another critical piece of this puzzle, so today our focus in on how we data in transit.

> Check out this new course from nativescripting.com on ???

## SSL All The Things

Since iOS ??? and Android ???, leveraging SSL is required by default for any external network calls. While occasionally inconvenient for developers, it's a great thing for end users to fall back on the fact that any data in transit is being encrypted.

So Rule #1, the simplest rule of them all, is to make sure that literally every remote call you make (I don't care if it's to an image or a remote endpoint) is performed via https.

Even in iOS 12, though, you can circumvent these rules with ???:

CODE

Don't get caught in this trap! This leaves your app vulnerable AND requires an additional explanation when you submit the app for review to the app store.

## Man-in-the-Middle Attacks

Leveraging SSL is critical when transferring data, but just hitting an `https` endpoint doesn't guarantee security unfortunately. This is where the dreaded "man-in-the-middle" attack comes into play.

A man-in-the-middle attack is...

Luckily for us, there is a NativeScript plugin to address that! The ??? plugin...

ssl pining plugin?

## End-to-End Encryption

Clearly the best solution for securing your data from the device to your backend is a fully integrated solution. This is where, again, Progress Kinvey comes into play.

With a feature-complete NativeScript SDK, Kinvey encrypts data at rest on the device, guarantees the integrity of your data in transit, and encrypts data at rest in the cloud!

IMAGE

## Last, But Not Least: Part Four

Our final article in this series focuses on a very common app scenario: secure user authentication and role-based authorization. Exciting stuff I know, which is why you'll be pleased to know you can copy-and-paste your way to success!

> Don't forget to register for our mobile app security webinar coming up on January 23rd!