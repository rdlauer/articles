# Spring 2019 NativeScript Community Survey Results

One of the most exciting times for our team is when the Community Survey is wrapped up and we have the opportunity to take a look at the results and analyze them. We are happy to share our findings with you:

## Demographics

Our typical respondents work in a **Technology company** which is usually not bigger than 5 people, generating less than **$1M of revenue**. They most likely have some **frontend background** and are directly using the technology. On average, they have 3.3 years of experience doing mobile and have been using NativeScript for **less than an year**. Most often, they were previously developing with Cordova/Ionic (34%), React Native (24%), or Flutter (17%).

![](https://i.imgur.com/uqnuvo0.png)
![](https://i.imgur.com/jLl8ufL.png)

Vue.js usage is continuing to grow (almost 1/3 of the users), while the Angular user base is steadily our biggest group of users (1/2 from the whole). 

![](https://i.imgur.com/NqsnlY3.png)

Looking at the what OS NativeScript developers are using - macOS is the leader followed by the Windows users.

![](https://i.imgur.com/XU85Sb0.png)

More than 70% of our users have been using the framework for less than a year, which neatly shows the growth of new and engaged users that we are seeing.

![](https://i.imgur.com/YbdjYYj.png)

## General Satisfaction

Our Net Promoter Score (NPS) is at a steady level compared to the previous year of 12.94. This comes with a good amount of Promoters (1/3 of the whole) and Passives (1/2 of the whole). When we break down the NPS results by flavour - we see insignificant differences within 5%, which shows that the overall experience is comparably good. 

When asked *"What is the reason NOT to promote the framework?"* - the biggest group of people answers that they haven't spent enough time, yet. This accounts for the big part of the respondents that are still forming their opinion and/or evaluating the framework. By far, the next two biggest pain points are *"Poor Developer Experience"* and *"Performance Concerns"*. Both of them are going to be in our focus for the coming months.

![](https://i.imgur.com/SP1WKO8.png)

## Getting Started with NativeScript

The getting started experience is showing improvement of 8% compared to last year. We are happy to say that the effort we put to provide quick hands-on experience with the technology through the [NativeScript Playground](https://play.nativescript.org/) and the `tns preview` command is being appreciated. An interesting finding here is that more than half of the people found out about NativeScript through some kind of content. Word of mouth is also a strong driver of new users and those three combined account for more than 75% of the new users. Have that in mind when you wonder next time how you can make {N} even more successful!

> [Write a blog post](https://www.nativescript.org/blog/your-first-contribution-to-nativescript-a-blog-post) and tell your friends about your usage - this can really make a difference.

![](https://i.imgur.com/xQep8Y3.png)

The satisfaction from the Getting Started experience is showing improvement of 5%. The two most common ways to start with {N} is either through the Getting Started Tutorials or Jumping directly into the CLI. The data show that people find their way around much easier when starting with the tutorials. This means that we will put additional effort in the next months to make the CLI more friendly for the first-timers. 

Talking about problem areas when getting started with the technology, we are seeing a great progress. *"Installation Experience"* was a long time winner in this not-so-prestigious competition. This time it falls down to #3 and we are happy that the effort the team put in providing a smoother getting started experience when it comes to setting up the environment is showing good results. Next priorities in this area would be *"Lack of Quality Documentation"* and *"Unclear Error Messages"*.

## Feature Prioritization

Happy to say that 7 of the 8 most wanted features are already in our plans for the next several releases. Those gravitate around:
 
  - **Improved CSS Support** - our plan is to extend the CSS integration by supporting more properties, strengthen the integration with Chrome Dev Tools' "Style" tab, and provide long-wanted features like `box-shadow` support, media queries, and so on. We will evaluate switching to alternative CSS parser, which potentially would improve the performance and provide more CSS features like CSS variables.
  - **Improved Testing Story** - the unit testing story stands out as a particularly important. This is the preferred way to test mobile applications amongst our community. We plan to dedicate time to clear out the [pile of issues](https://github.com/nativescript/nativescript-cli/issues?q=is%3Aopen+is%3Aissue+label%3A%22unit+testing%22) related to unit testing. Also, the available unit testing frameworks like Mocha, Jasimne or QUnit which NativeScript supports are not particularly suited for testing mobile applications. We will complement those with Mobile-friendly selectors and asserts.
  - **New App Themes** - we all want our apps to be beautiful and stylish. The current [Core Theme](https://docs.nativescript.org/ui/theme) provides some basic primitives to help you with that. Ideally, we want to improve it by making it easier to apply across the whole application, it should control the every aspect of the look and feel of your app, and should be easier for customization. Also, you probably heard that [iOS 13 will come with a Dark Mode](https://www.nativescript.org/blog/ios-13-sign-in-with-apple-ipados-and-arkit-3-all-on-nativescript) - NativeScript will deliver on this requirement through the Core Theme as well. Pretty excited to say that the first bit of these improvements will be part from the upcoming 6.0 release.

On the other hand, we will probably put features that show limited interest on the Backlog for now, like Foldable Phones support, Angular CLI integration and Integration in NativeScript in existing native iOS/Android apps.

![](https://i.imgur.com/I7mMf17.png)

## Documentation and Self Support

The documentation is another area where we see improvement in the satisfaction with around 4%. The most frequent reasons for being dissatisfied with the docs are *"Missing concise examples"* and *"Missing important topics"*. This is where your input is extremely valuable. If you notice missing content in the docs - please, [open a new issue](https://github.com/NativeScript/docs/issues/new) in the repository. This will help us to prioritize better and put our effort where it will matter most for you.

> Help us by [opening a new issue](https://github.com/NativeScript/docs/issues/new) if you notice a problem in the docs!

We recently decided to close our forums and [redirect all such questions to Stack Overflow](https://stackoverflow.com/questions/tagged/nativescript). This is already visible in the community and Stack Overflow is the second most frequent place to look for help.

![](https://i.imgur.com/Vyvgyqs.png)

## Development Experience

Stellar Development Experience has been our focus for the past year and we will continue to invest in it. This was the main driver behind switching to Webpack entirely for the development workflow, as we see many benefits that it can bring to the table - smaller and faster apps, quicker debugging cycles and in general more reliable tooling. A good amount of the problem areas below will be improved with switching to a Webpack-only workflow and continuing to polish the development experience.

![](https://i.imgur.com/TrXR1lp.png)

## Performance

This is another area in which we see good improvement. The result is 4% better than last year. Also, what used to be the #1 pain point - *"Slow Initial App Startup"* is now #3. Our focus for the next months is going to be on *"UI Interraction delays"* and *"Slow page transitions"*.

The plan for the last two is to continue polishing the framework behavior when `markingMode: none` is enabled. In 6.0 this flag will be enabled by default which is a step towards making it the only option once we see enough stability. Another important aspect for NativeScript Angular-based applications is switching to [Angular Ivy](https://www.nativescript.org/blog/nativescript-now-supports-angular-8) and maximizing the benefits from the new renderer. The expectation is that Ivy will generate a significantly smaller object graph, which should decrease the footprint on the Garbage Collection operations - the main reason for slow page transitions and delayed UI interactions.

## Others

Although most people find about the framework through quality content - this is only the #4 way people try to contribute to the framework. #1 is building a plugin, which is a great trend and we would like to see it growing further. But, think about this - if you built a plugin, you probably have a great story for a blog post. Probably, your plugin is filling in for a missing integration, exposing a native capability in a cross platform API, or wraps some kind of self-sufficient functionality - all of these would be very valuable story to write about. If you need a tribute - be sure to get in touch with our team. We are always happy to have guest bloggers on www.nativescript.org. 

![](https://i.imgur.com/7o6HO1X.png)

## Summary

The framework is showing improvement in the satisfaction in every important area - Getting Start Experience, Performance, and Documentation. We are glad to see that some of the biggest pain points feel less like a problem and the effort the team is investing to solve those is being appreciated. 

We have a [solid roadmap](https://www.nativescript.org/roadmap-and-releases) that includes the top rated feature requests and service integrations and in fact we are already working on part of them. Actually, some of them are coming in the next 6.0 release, which will be out in a matter of weeks.

These results make us much more confident in our plans for the rest of the year and will be the keystone for building our long-term vision. The best way to be part of our success is to spread the world about the framework and tell the word about technical issues that you solved, the cool apps you are building, or simply why you like the framework. And of course - to be part of the next Community Survey in the autumn!
