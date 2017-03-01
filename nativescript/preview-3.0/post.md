# Sneak Preview of NativeScript 3.0

Now that [NativeScript 2.5](https://www.nativescript.org/blog/nativescript-25-is-now-available) is out, we'd like to share more information about our next release - version 3.0 - which is expected to land mid-April. This is a major release with significant performance improvements, numerous fixes, and important new features that we believe our community will love. But to get there, there are a few important changes we want to share today.

> Disclaimer: This post is not intended to describe the 3.0 release in technical detail, but rather align expectations in the community. We are planning to release early alpha bits which will help us to gather feedback and ensure a smooth upgrade process.

**Why 3.0 instead of 2.6?**

According to the [Semver Specification](http://semver.org/), "Major version X (X.y.z | X > 0) MUST be incremented if any backwards incompatible changes are introduced to the public API". So, we are incrementing the major version because of some changes in the public API of the JavaScript cross-platform layer.

<hr />

**Are there any breaking changes and how might they affect me?**

Yes, there are some breaking changes related to the extensibility points of the cross-platform JavaScript Abstract Visual Tree (similar to what other frameworks call Shadow DOM), its property model, and the way the CSS styling mechanism is resolved and applied. Generally, these changes affect plugins that use certain groups of functionality. **We researched publicly available plugins and only expect a small percentage to be affected by these breaking changes.** Changes coming in NativeScript 3.0 should be completely transparent to the application logic in NativeScript applications. This will be carefully validated with plugin authors.

<hr />

**Why does there need to be breaking changes in 3.0?**

It all started when we were deeply profiling the JavaScript layer. We tried to fix various issues when applying changes to existing code but this approach was:

- error-prone;
- inefficient (meaning it did not allow us to achieve maximum performance);
- and made the code hard to read and maintain.

We take code quality and framework stability very seriously. To ensure we are on the right track, we implemented a working prototype which validated our new approach. We then undertook an analysis of the publicly available NativeScript projects and plugins to see how this would affect our community. Based on that study, we believe the impact is manageable, so we are moving forward with this new approach. 

We will prepare a detailed whitepaper to describe the changes in depth and provide a migration path to NativeScript 3.0. We'll also share with you the new performance-related improvements and features that come from NativeScript 3.0. For example, the visual tree hierarchies are now more encapsulated and more easily extended. Also, declaring a custom CSS property is now greatly simplified, leading to simpler property definition and less arbitrary code. We believe plugin authors will definitely appreciate even this single improvement.

<hr />

**What are the performance improvements? Numbers, please!**

So far we've conducted granular "synthetic" tests. Synthetic tests demonstrate relative performance changes by calling "method X" multiple times against 2.5 and 3.0 and comparing times. **For most of the tested methods the improvement is somewhere between a 50% to 400% increase in speed!** 

Our next step is to profile integrated applications to determine the overall effect on NativeScript app performance, and the initial results are very good. We believe you will be very happy with the improved runtime performance and faster loading screen UIs. We will share more results once we integrate everything in the master branches.

<hr />

**I am a plugin author, how do I verify my plugins against 3.0?**

Starting around the end of February, we will organize a series of meetings with community experts and plugin authors to discuss the changes, gather feedback, and see how we can help with the migration. ***The changes are expected to affect only a certain amount of UI plugins only.*** If your plugins are UI-agnostic, likely there is no additional work required to make your plugins 3.0-compatible. If your plugin is impacted, we are committed to working alongside the community to help create the necessary patches.

<hr />

**Can I experiment with 3.0 prior to the official release?**

Yes! In fact, **we are planning to introduce a release candidate build** as part of our ongoing releases, starting with 3.0. Our goal is to have 3.0 RC available to the community somewhere around March 22nd, allowing for 2 weeks of testing and gathering feedback. Prior to the RC, we plan to release several alpha builds to help plugin authors with early testing and experimentation. Naturally, we will take as much time as necessary to get a stable release, but our current working date is mid-April. It is important to us to handle this release with the professionalism and quality you deserve, so we can have a release our community loves and finds valuable.

<hr />

**You mentioned something about "master branches". Wouldn’t that affect `@next` builds?**

To prevent potential immediate breaking changes and confusion, we've temporarily disabled `@next` builds. We are currently running the 3.0 bits through our internal CI infrastructure and applying fixes where necessary. Once 3.0 is released, we will resume the regular `@next` builds for anyone that wants to grab the latest bits.

<hr />

**I understand there are breaking changes and improvements but what about new features?**

Yes, definitely! There are several new features that will be part of the 3.0 release:

- **Local snapshot builds for Android (macOS only):** This feature will improve the performance benefits of the current "snapshot" feature. Instead of serializing the entire core modules and Angular JavaScript, we will serialize only the AoT compiled, bundled, and minified output of WebPack. We expect this improvement alone to bring additional improvement in app loading time.
- **Continued expansion of Chrome DevTools features:** We are aiming to add the *Page Agent* and *Network* tabs.
- **Graphical installer for Windows:** This new installer will improve the local machine setup for first-time users of NativeScript on Windows.

## Conclusion

We hope that you are excited for the upcoming major release! This will be an important milestone for the entire project as the engineering team resolves two major pain points isolated during the past three years: a better property and styling system and improved extensibility. Each software product goes through different levels as it matures, and sometimes breaking changes are the only possible way to move to the next level. We are committed to do our best and ease the migration path. Stay tuned for more information in the coming weeks!

