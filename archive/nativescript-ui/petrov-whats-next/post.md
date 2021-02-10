# NativeScript UI - What's Next

In case you missed it, ever since [NativeScript Developer Day](https://www.nativescript.org/blog/nativescript-developer-day-2017-recap), the professional components in NativeScript UI are [completely free](https://www.nativescript.org/blog/nativescript-ui-is-now-free-here-s-how-to-get-started) (published in npm as [nativescript-pro-ui](https://www.npmjs.com/package/nativescript-pro-ui)).

*See NativeScript UI in action for yourself:*

iframe width="560" height="315" src="https://www.youtube.com/embed/4JJVOxybR4E" frameborder="0" allowfullscreen></iframe>

[Feedback](https://github.com/telerik/nativescript-ui-feedback/issues) for the components started pouring in as soon as we made the announcement, and we hope this means that it has fulfilled its purpose - the components are helping more and more people along their journey in building [success stories](https://www.nativescript.org/showcases) with NativeScript.

## What's Next

**Our next milestone is to enable usage of each one of the components without having to install the entire package.** This will make it easier for you to add only the components that you need for your apps, minimizing app size and the required references. One of the biggest inconveniences right now comes from the fact that, since iOS 10, Apple requires an explicit description when one is trying to access device events. We have such code in our calendar component, but as the code for all components is included in one big framework, any app using NativeScript UI [needs to have](https://github.com/telerik/nativescript-ui-feedback/issues/264) this description.

What we will do next is separate our code in a way that every component will contain only its own code - so the calendar's requirements will be necessary *only if you are actually using the calendar*. After we are done splitting our NativeScript Pro UI package, you will only have to make a minimal change to your NativeScript project by replacing the component you use with the new counterparts.

> In the long term, we are also considering the option to make the NativeScript Pro UI open source!

**We believe that this is an important step we need to make and have made it a top priority for the current development.** Don't let this discourage you from reporting any bugs you may find. They will not be forgotten even though we may need a little more time to provide the fixes.