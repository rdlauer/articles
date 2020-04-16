# Test in Production with Fiddler

The anti-pattern of anti-patterns. The king of worst practices. Testing your changes in production. Obscene!

In reality, we've all done it. We still do it. "Oh this change is so tiny it's hardly anything let's just push it up on Friday afternoon."

GIF

While we are collectively becoming more and more responsible with our test and QA practices, believe it or not, there *are* scenarios where testing a change in production is not only possible, but safe and effective.

Today we will take a quick look at how to tackle a few different "test in production" scenarios with Fiddler Everywhere.

## Our Scenarios

Remember TodoMVC? Well, let's pretend we've commercialized it (because why not) and a customer of ours is encountering numerous errors in production. We have a variety of suspects: from our frontend framework, to our build process, to some seemingly random issues that we simply can't replicate locally.

So let's look at some of these scenarios and see how each one can be effectively diagnosed with Fiddler Everywhere:

1. I'd like to see if the latest version of Vue.js magically fixes the problem and I want to see how my production app *reacts*. Pun intended?
2. Since the issue only shows up in production, I'd like to see if it's related to something in my production build process.
3. Some of the issues are intermittent. Yay! In this case, I'd like to see all traffic (including 404/500) exposed as I run through some activities my customer would perform.

## Updating a Library in Production

Nothing makes our lives easier as developers when an issue is magically resolved by updating to the latest version of a framework or library. Luckily with Fiddler Everywhere, this task is incredibly easy to perform and test, even when your app is already in production.

> While of course we will take the time to integrate the latest and greatest bits from our frontend framework of choice, the ability to *quickly* test our a specific scenario in production is nice to have.

Since the issue we are experiencing cannot be replicated in any environment other than production (yikes!), let's spin up our [TodoMVC Vue.js app](http://todomvc.com/examples/vue/) and open up Fiddler Everywhere:

IMAGE

Fiddler Everywhere provides the ability to both *inspect traffic* and create *auto responder rules* that are triggered based on certain scenarios. In our case, we want to investigate where and how the Vue.js script is loaded, and redirect that to another location (either remote or a local file) to load a newer version.

IMAGE

Now that we see where the script is loaded, we can create a rule...



## Can't Replicate a Production Bug Locally

The next step in our debugging process is going to be to look at any possible problems created during our build process. The most likely culprits are minification of our JavaScript assets or even a problem serving resources from our CDN, as those are the only real differences between our QA and production environments.

Once again, let's inspect our traffic to see where our minified libraries are loaded from our CDN:

IMAGE

...and let's then create a new auto responder rule to instead load the *unminified* (is that a word?) package from our *local machine*:

IMAGE

If this works, we can then investigate a problem with our webpack configuration or see if our CDN provider is having any issues.

## Debug All Incoming/Outgoing Traffic

Our next step is a bit of a "hail mary" attempt at finding an issue, but can actually be useful in some scenarios. The customer provided us a step-by-step walkthrough to help us replicate the issue. 

