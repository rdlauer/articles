# Migrating NativeScript Plugins to AndroidX 

The NativeScript 6.0 release is planned for July and with it comes [support for AndroidX](https://www.nativescript.org/blog/support-for-androidx-in-nativescript). We are also dropping support for the Android Support Library which is a **breaking change** and means code depending on it should be migrated.

This blog will provide what you, **as a plugin developer**, can do to prepare for the release. 

> **Important**: NativeScript 6 projects will build with both [`useAndroidX` and `enableJetifier`](https://developer.android.com/jetpack/androidx#using_androidx) enabled. Which means that all existing libraries (`.jar` and `.aar` files you ship with or include in your plugins) will be automatically migrated by `jetifier` at build time. 

That said, each NativeScript plugin falls in one of following categories:

1. There is **no need to migrate** the plugin. YEY!
2. Plugin can be made **compatible with both support-lib and AndroidX**.
3. There is **no compatibility path** (or it's just not worth it). You should publish an AndroidX-only version of the plugin to make it usable with NativeScript 6.0. 

<!-- 
Here is a flowchart that will help you determine which category does your plugin fall into:

![androidx-flowchart](https://i.imgur.com/iSbGieY.jpg)
-->

Let's see what is the recommended way to go forward in each case.

## No Need To Migrate

*Example*: [nativescript-localstorage](https://market.nativescript.org/plugins/nativescript-localstorage)

If your plugin (and its dependencies) does not depend on the Android Support Library in any way - you are off the hook. You can stop reading now.

Also, if the plugin uses a third-party native library that depends on support-lib, but does not use the support lib API in the JS/TS code - there is a good chance that you don't have to do anything. Jetifier will migrate the library build-time for you. Of course it is always best to test if you are not sure.

### Versioning

There is no need to release new version of the plugin in this case.

## Make it Compatible with Both Support-lib and AndroidX

*Example*: [nativescript-permissions](https://market.nativescript.org/plugins/nativescript-permissions)

In many cases it is possible to make your plugin compatible with both support-lib **and** AndroidX by performing a runtime check to know which one is available. You can check this [awesome blog by Nathanael Anderson on how to do that](http://fluentreports.com/blog/?p=720). He's got (and maintains) tons of high quality plugins so you can trust him 😉.

There are some cases when re-writing your code to be compatible will take more effort. Mainly, when you are **extending** classes from the support lib. Here is an example on how to do that:

	function useAndroidX {
	    return global.androidx && global.androidx.appcompat;
	};
	
	function getMyClass() {
	    if (useAndroidX()) {
	        // Class implementation using AndroidX
	        class MyCardView extends androidx.cardview.widget.CardView {
	            // implementation skipped for brevity 
	        }
	    
	        return MyCardView;
	    } else {
	        // Class implementation using support-lib
	        class MyCardView extends android.support.v7.widget.CardView {
	            // implementation skipped for brevity 
	        }    
	    
	        return MyCardView;
	    }
	}
	
	// Get the final class
	const MyCardViewClass = getMyClass();

### What?! But, I have so many questions!

Yup! The code above looks strange. Let's try to address some of the questions.

**1. How does the [Static Binding Generator (SBG)](https://www.nativescript.org/blog/static-binding-generator---what-is-it-good-for) know for which of the two implementations to generate bindings?**
 
SBG will try to generate bindings for both. However, it will only succeed for implementations that extends a class that it knows about at build time (libraries that you are referenced in the Android poject). So if you are building for NativeScript 5 - it will have the support-lib in its context and will manage to generate a class extending `android.support.v7.widget.CardView`.It will not generate bindings for the implementation extending `android.support.v7.widget.CardView`. The opposite is true when building for NativeScript 6. The `useAndroidX()` check will make sure you use the right implementation at runtime.

> **Important:** SBG will match the class in the `extends` directly. As it doesn't actually evaluate the JS code you are not allowed to put dynamic code there. So, the following **will not work**:

	const supportLib = global.androidx ? androidx.cardview.widget : extends android.support.v7.widget;
	class MyCardView extends supportLib.CardView { ... }

**2. Looks like a lot of duplicating code?**

You might try to share some of the code by extracting it outside the implementation of the classes. But...yeah, your code will get less pretty.

**3. What about TypeScript definitions?**

You can use the `androidx` tag of `tns-platform-declarations` to get AndroidX typings (`npm i tns-platform-declarations@androidx`). It will give you the definitions for types in AndroidX, but you won't get definitions for support-lib. You might want to use `<any>` or "stub" for one of the flavors (ex. `declare var androidx:any;`)

### Versioning

Once you're done with migration, you can safely publish these changes as a minor or patch release of the plugin. The good part is that you can do this now and your plugin will be usable with NativeScript 6 when it comes out. 

## No Compatibility Path

*Example*: [tns-core-modules](https://github.com/NativeScript/NativeScript/tree/androidx/tns-core-modules)

It might just not be possible for your plugin to be compatible with both support-lib and AndroidX. This can be due to multiple reasons: 

- Java code or third party lib depends on support-lib (and for some reason jetifier cannot handle it).
- You are extending support-lib classes in your JS code and you don't want to bloat your code with androix runtime checks.
- You have support lib configuration in `android-manifest.xml` or other resource that cannot be handled by jetifier.

There are some tools that might help you with the migration. You can use [jetifier-standalone tool](https://developer.android.com/studio/command-line/jetifier) for the native dependencies and/or [androidx-migration-tool](https://github.com/NativeScript/androidx-migration-tool) for your JS/TS/Java code. You can also check [support for AndroidX in NativeScript for tips on migration](https://www.nativescript.org/blog/support-for-androidx-in-nativescript). 

### Versioning

Here is the recommended action plan:

1. Migrate the plugin.
2. Bump a major version and publish it behind `androidx` tag in npm: `$npm publish --tag androidx`
3. (When NativeScript 6.0 is released) Publish the AndroidX version. It's a good idea to add a peerDependency to `"tns-core-modules": ">6.0.0"`.

> **Note:** Publishing behind the `androidx` tag before the official NativeScript 6 release will give a chance to other plugins and applications that depend on it to be tested.

## Test Your Plugin Now!

Regardless of which category your plugin falls into, it is highly recommended to test it now using the `androidx` builds of the android-runtime and tns-core-modules:

	npm install tns-core-modules@androidx --save-exact
	tns platform add android@androidx
	npm install tns-platform-declarations@androidx --save-exact (if you need typings)

You can then run your demo projects to assure everything is working as expected.

## Getting Help

Handling breaking changes can be tedious, but we are here to help. Reach out to us in the [NativeScript slack - #androidx-migration channel](https://nativescriptcommunity.slack.com/messages/CJ3RVMFBP) if you get stuck.

