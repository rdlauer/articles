# Test in Production with Fiddler

The anti-pattern of anti-patterns. The king of worst practices. Testing your changes in production. *Obscene!*

In reality, we've all done it...and we still do it today. "Oh this change is so tiny let's just publish it on Friday afternoon and go grab some brews."

GIF

Cheers! 🍻

While we are collectively becoming more and more responsible with our testing and QA practices, believe it or not, there *are* scenarios where testing a change in production is not only possible, but also safe and effective.

Today we will take a quick look at how to tackle a few different "testing-in-production" scenarios with [Fiddler Everywhere](https://www.telerik.com/fiddler-everywhere).

> If you're not yet familiar with Fiddler Everywhere, it's a new version of Fiddler with focused functionality for macOS, Linux, and Windows.

## Our Scenarios

Remember [TodoMVC](http://todomvc.com/)? Well, let's pretend we've commercialized it (because why not?) and a customer of ours is encountering some errors in production.

IMAGE

We have a variety of suspects: our frontend framework, maybe our build process, to some seemingly random issues that we simply can't replicate locally.

Let's look at some of these scenarios and see how each one can be effectively diagnosed with Fiddler Everywhere:

1. Maybe an updated version of Vue.js magically fixes the problem and I want to see how my production app *reacts*. Pun intended? 🤔
2. Since the issue only appears in production, I'd like to see if it's related to something in my build process. 🏗️
3. Some of the issues are intermittent. Wonderful! In this case, I'd like to see *all* network traffic (including 404/500) as I run through some customer workflows. 🚦

## Updating a Library in Production

Nothing makes our lives easier as developers when an issue is magically resolved by updating to the latest version of a framework or library. Luckily with Fiddler Everywhere, this task is incredibly easy to perform and test, especially when you want to do so in production.

> While of course we will take the time to integrate and test the latest version of our library/framework, the ability to *quickly* test our a specific scenario in production is nice to have. 😍

Since the issue we are experiencing cannot be replicated in any environment other than production (yikes!), let's spin up our [TodoMVC Vue.js app](http://todomvc.com/examples/vue/) and open up Fiddler Everywhere:

IMAGE

Fiddler Everywhere provides the ability to both *inspect traffic* and create *auto responder rules* that are triggered based on defined scenarios. In our case, we want to investigate where and how Vue.js is loaded, and redirect that to another location (either remotely or locally) to load a different version.

IMAGE

Now that we see where the script is loaded, we can create a rule...



## Can't Replicate a Production Bug Locally

The next step in our debugging process is to look at any possible issues created during our build process. The most likely culprits are minification of our JavaScript assets or even a problem serving resources from our CDN, as those are the only obvious differences between our QA and production environments.

Once again, let's inspect our traffic to see where our minified libraries are loaded from our CDN:

IMAGE

...and let's then create a new auto responder rule to instead load the *unminified* (is that a word?) package from our *local machine*:

IMAGE

If this works, we can then investigate a problem with our webpack config or see if our CDN provider is having any issues.

## Inspect All Incoming/Outgoing Traffic

Next up is a bit of a "Hail Mary" attempt at finding an issue, but can actually be useful in certain scenarios. The customer provided us a step-by-step walkthrough to help us replicate the issue. But when we follow the steps locally, we can't replicate the problem. Classic "works on my machine" situation.

> NOTE: In this case, alternatively the usage of a tool like [FiddlerCap](https://www.telerik.com/fiddler/fiddlercap) can be useful!

So what we want to do is work our way through a variety of customer-driven scenarios with our app. We can look for any `404` or `500` errors in Fiddler Everywhere that may have been otherwise missed.

IMAGE

In this case we can see that sure enough, a `500` error was thrown