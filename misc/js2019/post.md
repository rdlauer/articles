# "JavaScript-Native" Mobile Development

Time certainly flies when you realize it's been almost four years since the some of the most popular "JavaScript-native" frameworks were first released! Born back in 2015, React Native and NativeScript both provide a similar model for JavaScript developers to crack into the *truly native* mobile development world. By using web skills like JavaScript, Typescript, and CSS - along with established frameworks like React, Angular, and Vue.js - these development options have created a new market for traditional web developers.

While the popularity of these frameworks is growing, some high-profile companies like AirBnb have decided to scale back their usage of React Native. Where does this put JavaScript-native on the map today, where is it headed in 2019, and how does the Dart-based upstart Flutter come into play?

> (this could be in a sidebar?) **What is a JavaScript-native framework?** Hybrid app development frameworks like PhoneGap/Cordova/Ionic let you use your web skills (HTML/CSS/JavaScript) to create natively *installable* apps. However, the app runs in a WebView, which presents performance and UI limitations. JavaScript-native frameworks like NativeScript and React Native use JavaScript (and CSS) to power (and style) truly native UI on the device, without any WebViews, giving you the API reach and performance you demand.

## JavaScript-Native Today

For two years running, we've predicted a strong and steady rise in the adoption of both React Native and NativeScript, and we haven't been disappointed:

![react native and nativescript downloads](rn-and-ns.png)

Year-over-year growth in adoption of both frameworks have been impressive, especially considering how they are not necessarily competitors, but rather complimentary frameworks.

### React Native

Boosted by the rise of React over the past few years, React Native is THE way to develop native mobile apps with the React library. 73K GitHub stars and 45K questions on Stack Overflow make it clear how popular React Native has become. 2018 was another monster year for React Native, with companies like Facebook (of course), Walmart, and Bloomberg relying on it to power at least part(s) of their existing native mobile apps. However, Airbnb was extremely public in their dismissal of React Native, stating "its benefits didn’t come without significant pain points".

Undeterred, the future remains bright for React Native:

> "We're working on a large-scale rearchitecture of React Native to make the framework more flexible and integrate better with native infrastructure in hybrid JavaScript/native apps." - State of React Native, 2018

Going forward, the React Native team looks to be making RN more lightweight and better able to fit into existing iOS and Android apps. By changing the threading model to one that is more similar to NativeScript, they hope to improve performance and stability. They are also incorporating async rendering capabilities into React Native to allow multiple rendering priorities and to simplify asynchronous data handling. Direct calls between native APIs and JavaScript will also be more efficient and easier to debug.

### NativeScript

Like React Native, NativeScript lets you use your web skills to create truly native mobile apps for iOS and Android. Unlike React Native, NativeScript provides official support for the Angular and Vue.js frameworks, along with plugin-free access to 100% of native device APIs. Large companies like SAP are betting on NativeScript, and the #1 app in the US app stores over the holidays was a NativeScript-built app.

Where does NativeScript go from here?

While the core NativeScript framework has matured, there is always room for improving the developer experience, which is precisely a priority for the team in 2019. By improving capabilities such as Hot Module Replacment and adding additional support for CSS properties, the hope is to bring NativeScript as close as possible to the web-based iterative workflow that we are all used to. The NativeScript team also wants to enable developers to create beautiful apps, by offering a full Material Design library out of the box. Both Angular and Vue.js developers will rejoice when they find that the NativeScript to Web code sharing stories will be a priority in 2019 as well.

### Flutter

The new kid on the block in 2018 was certainly Flutter. A product of Google, Flutter also lets you build native apps for iOS and Android from a single codebase. Flutter's architecture is very different from both NativeScript and React Native though, as instead of leveraging platform-specific native UI, you build your app UI on top of a 2D rendering engine. This leads to great performance, smaller app sizes, but a considerably steeper learning curve than React Native or NativeScript.

*Yes, even though Flutter requires you to develop with Dart, not JavaScript, we are including it here!*

Flutter was a big hit in 2018, scoring 52K GitHub stars and 10K questions on Stack Overflow.

[NEED TO ELABORATE ON FLUTTER IN 2018/2019 HERE]

## Moving Forward with JavaScript-Native

Rising tides lift all boats, and with React Native, NativeScript, and Flutter as tools, mobile app development has become more accessible to web developers than ever before. But how are these frameworks going to build on their successful pasts?

Developing with any of the JavaScript-native options today provide table stakes functionality in terms of native iOS and Android development with some amount of shared code between the two mobile platforms. And while keeping up with the latest releases and APIs from Apple and Google will always be a priority, React Native has publicly stated their concerns with improving stability; NativeScript with focusing on developer productivity; and Flutter with targeting ease of adoption and core fundamentals.

With one could argue these frameworks are approaching a "feature-complete" state, where do we see them headed later in 2019?

### To the Web!

Wait a minute, didn't we just come from the web? Yes, but hear me out. Developing omni-channel mobile experiences is becoming a more and more critical component of what it means to create an "app" today. Responsive/mobile web, PWA, native iOS, native Android - that's arguably four distinct platforms to share a *single* experience.

This is where initiatives like React Native Web, NativeScript Schematics, and Flutter's Hummingbird come into play. With the ability to create responsive web experiences *along with* native iOS and Android apps, these frameworks are leading the way to advanced developer productivity and time-saving capabilities. We see this area booming in 2019.

[NEED TO ELABORATE ON THIS A BIT???]