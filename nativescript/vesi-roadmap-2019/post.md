# What's on the NativeScript Roadmap for 2019?

As you may have seen in Rob Lauer's tweet (below), we gathered a few weeks ago to brainstorm NativeScript's destiny in 2019. We had a one-day session with the team's managers, our awesome DevRel Advocates, and special guests 👉 [Eddy Verbruggen](https://github.com/EddyVerbruggen) and [Igor Randjelovic](https://github.com/rigor789)!

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">.<a href="https://twitter.com/NativeScript?ref_src=twsrc%5Etfw">@NativeScript</a> in 2019 ladies and gentlemen!<a href="https://twitter.com/hashtag/Vuejs?src=hash&amp;ref_src=twsrc%5Etfw">#Vuejs</a> <a href="https://twitter.com/hashtag/Angular?src=hash&amp;ref_src=twsrc%5Etfw">#Angular</a> <a href="https://twitter.com/hashtag/BeyondMobile?src=hash&amp;ref_src=twsrc%5Etfw">#BeyondMobile</a> <a href="https://t.co/vZiNOjgCkZ">pic.twitter.com/vZiNOjgCkZ</a></p>&mdash; Rob Lauer (@RobLauer) <a href="https://twitter.com/RobLauer/status/1063068666730831872?ref_src=twsrc%5Etfw">November 15, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I won't focus on the details of how the day was structured and organized but instead prefer to focus on the results and how we see the framework evolving during the next year.

> One thing I would like to note is that nothing is set in stone - and any feedback from you, our community, is more than welcome!

*So, let's start!*

## 🔭 NativeScript + Vue.js

[Nativescript-Vue](https://nativescript-vue.org/) gained speed in 2018 and we will continue to support Igor and all the contributors. The core team will pay special attention to the NativeScript UI plugins support for Vue and will make sure to remove any blockers to the Vue.js integration. What current plugin authors can do to strengthen Vue.js support is to review their plugins and make sure they are NativeScript-Vue compatible. More details will be available with the next releases of {N} and {N}-Vue.

This all provides a pathway for NativeScript-Vue to be *officially supported* as part of the core NativeScript framework in early 2019!

## ⚒ Improved Developer Experience

The team will continue working on improving the developer experience with [HMR and Webpack](https://www.nativescript.org/blog/nativescript-hot-module-replacement) with the goal for Webpack to become the **only way to develop** with NativeScript. No fear, it will happen gradually:

- Enable webpack by default – executing `tns run` without the need to provide `--bundle` flag 
- Deprecate development without Webpack 
- Remove the possibility for development without Webpack

We already showed that development with Webpack enables HMR, provides better build times of the app during development, and optimizes the app in terms of startup time, so we think that this should be available to all of you out of the box without the need of using different options like `--bundle`or `--hmr`. We expect it to be a huge win for all the {N} lovers out there.

And since according to the NativeScript survey results, "Unclear error messages" is stated as the top issue while developing NativeScript apps, we will review the current error handling mechanism and improve the way errors are presented in the CLI. 

In addition, we'll pay increased attention to the [current VS Code extension](https://www.nativescript.org/nativescript-for-visual-studio-code) in terms of **Intellisense** and **stabilization** and make it the preferred tool to use during development.

## 📲 Code-Sharing between Web and Mobile

We introduced code sharing capabilities with Angular in 2018 and we're dedicated to continuing to evolve this option while providing all the tooling needed for a larger percentage of code shared. Our goal is to enable you to share more than you can now and to do it in an easy and automated way with provided tooling.

We will invest time to **build a better starting template** that will allow you to:

- share the styling between your web and mobile applications 
- arrange shared and platform specific modules

We will also work on a **style guide** that will provide you with strong recommendations on:

- where to put shared or platform specific modules/components 
- how to handle platform specific TypeScript code 
- how to handle various scenarios like: *"I want to use a SideDrawer for navigation of my mobile app, but in the web app I want the navigation to be in the ActionBar"*

> A comprehensive code sharing story with NativeScript and Vue.js will appear in 2019 as well!

## 🕸️ nativescript-web

Why *share* code between web and mobile, when we can just *use* that same code on web?

This is the idea behind a **very experimental** idea we have called nativescript-web. The concept is to actually generate 100% of your web code (specifically PWA) from your existing NativeScript app. This would provide 100% code sharing across all platforms, and even provide a path towards macOS, Windows, and Linux desktop support via Electron!

> Keep in mind this is, by far, the most experimental and least well-defined feature on our roadmap! We will have to see what 🎁 2019 brings...

## 📊 Analytics & Crashlytics

Currently, [analytics](https://www.nativescript.org/blog/how-to-add-firebase-analytics-to-your-nativescript-mobile-app) and Crashlytics can be used but there isn't much documentation or content covering the topic. We also realize that the errors logged are not always helpful and clear enough. First, the team will focus on providing a meaningful native stack as opposed to memory pointers and line numbers.

The second area we will improve is exposing errors in the JavaScript code to the error reporting services – they usually contain the business logic of the app and currently those are completely hidden. Errors, while not crashes, are still important for the health of your application. They will also have to find their way to your dashboard! In addition, we'll better document the integration with existing analytics and Crashlytics services.

## 📟 Achieve a Rich and Beautiful UI

The team will invest time in enabling some of the **most requested CSS properties** during the past year. We'll open a discussion in GitHub soon to identify which ones to start with. And since there are a lot of CSS properties that might be enabled, we would **embrace any community help** in this task!  

We also know most of you have web experience and love the Chrome Dev Tools, so we'll add support for **Chrome Dev Tools CSS Inspection** as well. 

Last but not least, we'll collaborate with the community to **enable Material Design** to its fullest extent.

## 🤹 What Еlse?

We'll invest in improving the **getting started experience** by providing more samples, improved documentation content, and better error messages during environment setup. 

We also realize that the **unit testing** approach the framework currently provides has some [issues](https://github.com/NativeScript/nativescript-cli/issues?q=is%3Aopen+is%3Aissue+label%3A%22unit+testing%22) so, we'll work on stabilizing it. 

**Performance** is an aspect which can always be improved and is a task that never ends. We expect that all the Webpack integrations and optimizations mentioned previously will contribute to performance of both app startup times and in-app experience.

## How Can You Help?

Glad you asked!

👀 Have you identified an area from the above mentioned where you already made some progress - or just want to collaborate with the team and do a PR? Just [post a GitHub issue](https://github.com/NativeScript/NativeScript/issues/new/choose) about it to contact us and discuss your future contributions.

🎉 Have you built a cool tab bar, action bar, or other eye-catching UI? **Celebrate your achievement** by [sharing it with us](https://github.com/NativeScript/code-samples/blob/master/CONTRIBUTING.md) and we'll add to the [NativeScript samples](https://market.nativescript.org/?tab=samples&framework=all_frameworks&category=all_samples)! 

📊 Have you already integrated an analytics service and/or Crashlytics? Have extensive unit testing in your app? Share your experience in a **blog post** and let us know about it! 

📢 **Spread the word** about NativeScript in your local area.

📝 Having **100% access to native APIs** out of the box adds infinite opportunities for integrations and usage of the framework. **Write a blog post** to share your experience with NativeScript ([reach out to Rob Lauer](https://twitter.com/RobLauer) and we can feature it on the NativeScript blog!)  

🔌 Contribute to the ecosystem by [building a plugin](https://docs.nativescript.org/angular/plugins/plugin-reference) or a nice [sample app](https://github.com/NativeScript/code-samples/blob/master/CONTRIBUTING.md).

💡 Post and answer [issues in Github](https://github.com/NativeScript/NativeScript/issues)!
 
👫 Help community members in [NativeScript Community Slack](https://www.nativescript.org/slack-invitation-form) or [Stack Overflow](https://stackoverflow.com/questions/tagged/nativescript).

🎲 Want to contribute but you don't know where to start? Challenge yourself and pick any of the [good first issues](https://github.com/NativeScript/NativeScript/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) to give yourself a try, follow the corresponding repository [contributing guidelines](https://github.com/NativeScript/NativeScript/blob/master/CONTRIBUTING.md) to setup your environment for development and find instructions on how to do a pull request. 
  
📚 Improve the [documentation](https://github.com/NativeScript/docs)!

🎨 Have you built a nice-looking app with {N}? Show it off and [submit it](https://www.nativescript.org/showcases/submit) to our [Showcases](https://www.nativescript.org/showcases).

## Thank You for a Fantastic 2018

This is the output of the collective effort of our Engineering team, DevRel team, and our community (including **YOU**). It's a solid and ambitious plan that will make NativeScript even better!

*Happy NativeScripting and let 🎆2019 be even more successful than 2018!*

