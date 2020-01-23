# First Class Kotlin Support Now Available in Progress Kinvey Android SDK

When I think about [Progress Kinvey](https://www.progress.com/kinvey), I tend to skip over its standard backend capabilities and instead focus on the value-added features it provides to me, as an app developer (going above and beyond traditional serverless providers):

- 🔐 Compliance and [security](https://www.progress.com/kinvey/enterprise-security) out of the box;
- 🔌 What feels like a million [data connectors](https://www.progress.com/kinvey/data-pipeline);
- 👤 Easy enterprise [authentication](https://devcenter.kinvey.com/rest/guides/mobile-identity-connect);
- ...and a mighty collection of [client SDKs](https://devcenter.kinvey.com).

Speaking of those client SDKs, instead of providing one or two SDKs to cover "most" app development scenarios, Kinvey goes above and beyond to provide tailored SDKs for a variety of frameworks and platforms:

- iOS
- Android
- NativeScript
- HTML5
- REST API
- Xamarin
- Angular
- PhoneGap/Cordova
- Node.js
- .NET

## So Why Kotlin?

[Kotlin](https://kotlinlang.org/) first appeared on the scene in 2011 and within a few years was widely embraced as a language evolution for Java developers. Features of Kotlin enable more legible code without any sacrifice in performance.

In fact, last year, Google announced that Kotlin is now its *preferred* language for Android app development!

*Here is some Java code I don't understand:* 😬

	static MyFragment newInstance(String arg1, String arg2) {
	  MyFragment fragment = new MyFragment();
	  Bundle arguments = new Bundle();
	  arguments.putString(ARG_1_KEY, arg1);
	  arguments.putString(ARG_2_KEY, arg2);
	  fragment.setArguments(arguments);
	  return fragment;
	}

*And here is the more legible equivalent in Kotlin:* 🤩

	companion object {
	  fun newInstance(arg1: String, arg2: String): MyFragment {
	    return MyFragment().apply {
	      arguments = Bundle().apply {
	        putString(ARG_1_KEY, arg1)
	        putString(ARG_2_KEY, arg2)
	      }
	    }
	  }
	}

## Android SDK Gets a Revamp

The [Kinvey Android SDK](https://github.com/Kinvey/android-sdk) has been with us for some time, nothing new about that. However, with the rising popularity of Kotlin, it became clear the team needed to modernize Kinvey's Java-based Android SDK to enable Kotlin developers to feel comfortable as well.

With the SDK codebase migrated to Kotlin, this also meant migrating existing Android sample apps to Kotlin. As of today, there are a wide variety of Android sample apps for you to choose from:

- [github.com/KinveyApps/GeoTag-Android](https://github.com/KinveyApps/GeoTag-Android)
- [github.com/KinveyApps/Kitchensink-Android](https://github.com/KinveyApps/Kitchensink-Android)
- [github.com/KinveyApps/MICFlow-Android](https://github.com/KinveyApps/MICFlow-Android)
- [github.com/KinveyApps/StatusShare-Android](https://github.com/KinveyApps/StatusShare-Android)
- [github.com/KinveyApps/TestDrive-Android](https://github.com/KinveyApps/TestDrive-Android)

## Get Started with Kinvey (and Kotlin!)

You can find the complete [Android SDK on Github](https://github.com/Kinvey/android-sdk), and to get started using it with Kinvey I'd suggest consulting the [Kinvey DevCenter](https://devcenter.kinvey.com/android/guides).

**What's next?** Well, we'd love to hear your feedback on the new Android SDK! Feel free to [submit a pull request](https://github.com/Kinvey/android-sdk) and/or tell us how we can improve the SDK via the [Kinvey developer forum](https://support.kinvey.com/support/discussions).

And if you haven't already, be sure to sign up for your [free 30-day Progress Kinvey trial today!](https://console.kinvey.com/sign-up)