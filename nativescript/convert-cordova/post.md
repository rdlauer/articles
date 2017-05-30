# Converting a Cordova/PhoneGap App to NativeScript

Have you ever seen the time/money/energy...

Mobile app developers live in a world of compromises. The problem is we want it all! We want:

- Our apps to be performant, so customers don't abandon us;
- One codebase to support multiple mobile platforms;
- To re-use our skills and teams that we've been nurturing for years;
- To leverage existing libraries so we don't have to re-write code that is already proven.

And how do we get there?

We have tried going pure native. But that meant sacrificing one codebase and existing skills in favor of "siloed" code and net new languages and frameworks.

We have stuck with the web. This has worked out well for us, as the advent of Progressive Web Apps have helped web perf, and increasing numbers of native device features are exposed via new APIs. But we realize the web will always lag behind native when it comes to feature access and performance.

We found the hybrid model, Apache Cordova (PhoneGap) to be specific, which started to bring the best of both worlds together: developing one codebase for multiple platforms, with our web skills, accessing virtually any device feature we could want.

80/20 becomes 80/80

IMAGE OF HYBRID BEST OF BOTH WORLDS

Sounds great right? And hybrid can still be good. But (there is always a "but") hybrid apps still run in WebViews on devices. Meaning they are hamstrung by the performance-limited WebView running UI that can be made to look native, but only occassionally feel native (see: ListView scrolling, OTHERS EMOJI).

IMAGE OF HYBRID COMPROMISES

So here we are, stuck in our world of compromises.

## Enter NativeScript

NativeScript was introduced a little more than two years ago as an evolution of the hybrid model. Remember the four items we want from the beginning of this article? NativeScript delivers on all of them:

- Performant? NativeScript apps are truly native, meaning native UI, native performance;
- One codebase? NativeScript leads with a one shared codebase message for iOS and Android development (with platform-specific capabilities when needed);
- Re-use skills/teams? NativeScript apps are built on the foundations of the web: with an HTML-like syntax for UI, JavaScript powering the app, and everything styled with CSS;
- Using existing libraries? NativeScript supports the use of existing JavaScript libraries, native CocoaPods for iOS, and Android JARs (not to mention loads of plugins).

So what compromise do you have should you choose NativeScript? It's a new framework, so there is a learning curve. I propose that the positives outweigh the negatives more than any other alternative though.

## Have a Cordova App, Now What?

Ok, so NativeScript sounds good, and your next stop should be one of the two getting started tutorials:

- Getting Started with NativeScript Core and JavaScript
- Getting Started with NativeScript and Angular + TypeScript

But what if you already have a hybrid app and you want to convert it to NativeScript?

Let's look at some techniques to leverage as much as we can to migrate from hybrid to native. We will cover three main topics:

- Native UI and Layouts
- Code-Behind JavaScript
- Styling with CSS

> It just so happens there is an expansive guide on [Upgrading Hybrid Apps to Native with NativeScript](http://www.hybridtonative.com/) for the Angular developer that goes far beyond what we will be covering today.

Todo app mockup?

## Native UI and Layouts

Constructing your UI with a hybrid app is relatively easy, since you're just using HTML. The downside is that you are using non-native UI and losing out on the performance and experience of truly native UI.

So how do we migrate from HTML to NativeScript's markup? Let's take a look at the HTML powering our todo app:

CODE

NativeScript offers a variety of options for UI elements. A full list is available here, but for our todo app we just need the following:

CODE

It's one thing to spit some UI elements out on a screen, but its another to lay them out in a way that is nice looking and engaging. That is where NativeScript layouts come in.

IMAGE

There are five "traditional" NativeScript layouts you can choose from:

- Absolute
- blah

While they are all fairly intuitive, the power of NativeScript layout comes from the fact that they can be nested as well. There is nothing wrong with nesting a `GridLayout` inside of a `StackLayout` if that is what your UI requires.

> Flexbox

So while there is no concept of a `<div>` element in NativeScript, you do have layouts that perform a similar function.

Back to our todo app example. We can translate our HTML into truly native UI with something like this:

CODE

## Code-Behind JavaScript

The beauty of leveraging web-based technologies is at the end of the day, we're all just writing JavaScript. This doesn't change with NativeScript. It's. Just. JavaScript.

For example, a basic JavaScript function supporting our hybrid todo list app might look something like this:

CODE

That same code, supporting a native iOS/Android app in NativeScript, is going to look like this:

CODE

See any differences? I didn't think so.

Now, this isn't to say you can literally copy and paste all of your code have it "just work" with NativeScript. NativeScript doesn't have a DOM (since it's native UI, not a WebView). This means libraries like jQuery that expect a DOM or window object will not function. However, other popular libraries like moment.js, asdf, and adsf work just fine.

All this to say: if your existing JavaScript code isn't directly interacting with UI, you can probably copy and paste it into a NativeScript app.

Likewise, if you're familiar with the MVVM pattern and your hybrid app is built on that, you will be at home with NativeScript. You also may be glad to hear that TypeScript is supported out-of-the-box as well with NativeScript!

Finally, Angular is a first class citizen in NativeScript. In fact, if you have an app built with Angular 2+, you'll find the transition from hybrid to native to be incredibly simple.

> If you're interested in a true code-sharing experience from web to native mobile, check out Nathan Walker's Angular seed project.

## Styling with CSS

When you're building a hybrid app, you are using CSS to style your app, mimic native UI, tweak positioning of elements, and so on. Luckily, NativeScript supports a subset of CSS properties so you can transition your mighty styling skills over as well.

Here is a snippet of a stylesheet from our hybrid todo app:

CODE

We can see that this snippet is used to style ???. The same styling for a NativeScript app would look like this:

CODE

What changed? Not much. ???

NativeScript supports a variety of ways to apply your CSS. By default, you'll be provided with an `app.css` file. This is where you apply styles that you want applied globally to your app. If you want to apply CSS to just one view/page of an app, you simple name your CSS file the same as the corresponding view/page. For example, `main-page.css` will only be visible on `main-page.xml`. Finally, you can apply CSS inline EMOJI the same you way you do on the web:

	<Button text="Tap Me!" style="background-color: green;" />

What about CSS selectors? Same as the web!

- Select by Type: `button { background-color: blue }`
- Select by Class: `.big-button { font-size: 28 }`
- Select by ID: `#some-button { font-weight: bold }`
- Select by Hierarchical Combinations: `GridLayout Button` and `GridLayout > Button`
- Select by Attribute: `button[someAttribute]{ background-color: orange; }`
- Select by State: `button:highlighted { background-color: red; }`

> If you are a SASS or LESS person, you can install the appropriate plugins to use those technologies in NativeScript as well!

And custom fonts? Well of course you can use any font you want, provided you have the .ttf or .otf font file to include in your app. To use a custom font, drop the .ttf/.otf file in your `app/fonts` directory and reference it with some familiar CSS:

	.roboto {
		font-family: "Roboto";
	}

## Next Steps

This article is just a small taste of how easy it is to migrate from Cordova/PhoneGap to NativeScript. For a more in-depth look, be sure to read the guide provided at hybridtonative.com.

If you're new to NativeScript and would like to learn more about the framework itself, look no further than our getting started tutorials. Enjoy!