# Vue.js and NativeScript in One Minute

One of the pillars of NativeScript is the extensibility of the framework itself. There isn't another native JavaScript framework out there that is flexible enough to provide support for an established framework like [Angular](https://angular.io/), a community-driven [Vue](https://vuejs.org/) integration, a way-too-early look at [Preact support](https://github.com/staydecent/nativescript-preact), and an ongoing commitment to "vanilla" JavaScript (a.k.a. NativeScript Core).

Did you know in the early days we considered the name "Angular Native" for NativeScript? While we adore Angular, we didn't want to pigeonhole ourselves into one specific framework. This is why we are so excited about the growth in interest in merging [Vue with NativeScript](http://developer.telerik.com/products/nativescript/native-ios-android-vue-nativescript/).

A few weeks back I put together a short (like one minute short) video on developing a NativeScript app with Vue. If you missed it then, here you go:

iframe width="560" height="315" src="https://www.youtube.com/embed/jnRmXczcSdQ" frameborder="0" allowfullscreen></iframe>

**Want to try it for yourself?** Here it is, from scratch, zero to native mobile app with Vue and NativeScript:

## Using Vue with NativeScript

You haven't installed NativeScript yet? Wait, of course you have. But [just in case](https://docs.nativescript.org/):

	$ npm install -g nativescript

Next we need to create the NativeScript project:

	$ tns create vue-test (or whatever you want to call your app)

Navigate to the newly created directory:

	$ cd vue-test

Install the [nativescript-vue plugin](https://www.npmjs.com/package/nativescript-vue):

	$ npm install --save nativescript-vue

Open the `app.js` file and replace the existing contents with this:

	const Vue = require('nativescript-vue/dist/index');
	
	new Vue({
	  template: `
	    <page>
	      <stack-layout>
	        <label text="Hello Vue!" style="background-color:#41b883;color:#ffffff;padding:10;text-align:center"></label>
	      </stack-layout>
	    </page>
	  `,
	}).$start();

Before we continue, we should take a quick look at what is happening in this code:

- We are requiring the `nativescript-vue` module;
- We are creating a new Vue template, which is just {N} layouts and UI components;
- We are calling `$start()`, which calls the NativeScript `application.start()` method you may be more familiar with.

Finally, run the app on an iOS/Android connected device or simulator/emulator:

	$ tns run ios | android

**Success!**

![ios android nativescript vue](nativescript-vue.png)

## What's Next?

As we march towards a 1.0 release, we encourage you to play around with the [nativescript-vue plugin](https://www.npmjs.com/package/nativescript-vue). Try out all of the NativeScript layouts and UI components. In a giving mood? [Check for open issues](https://github.com/rigor789/nativescript-vue/issues) and squash some 🐛 with us!

