# Debugging with Fiddler Everywhere: Diagnosing a Remote API Failure

For years [Fiddler](https://www.telerik.com/fiddler) has been *the* gold-standard tool 🥇 for diagnosing and debugging network issues for both web and desktop applications.

[Fiddler Everywhere](https://www.telerik.com/fiddler-everywhere) represents the *next* generation of Fiddler tooling: the same core features of Fiddler wrapped up in a more engaging, cross-platform interface.

In this series, we are looking at a set of specific scenarios we regularly encounter during app development. Today we are going to look at that dreaded issue of remote APIs failing on us. These are remote resources that are completely out of our control, thus far more difficult to debug.

> **NOTE:** Fiddler Classic (the original Fiddler) isn't going anywhere! You can still [download Fiddler](https://www.telerik.com/download/fiddler) and use it like you always have on Windows.

So let's get started!

## Our Scenario: Diagnosing a Failing Remote API

As a developer, I'm utilizing a remote third-party API for my app. While it had been working just fine for days, now every time I generate a request, it fails without any context! I would just copy and paste the endpoint URL into my browser or another tool, but there are authentication tokens and parameters added to the request that make this a difficult request to generate manually.

Frankly I just need to quickly re-generate the request and inspect/debug the failed response, based on real world usage within my app.

## Fiddler Everywhere's Solution

Using Fiddler Everywhere, we can easily view, record, play back, and inspect individual network requests made by any desktop apps running locally (including any modern web browser).

To diagnose and debug this issue I'm going to:

1) Open Fiddler Everywhere and toggle the **Live Traffic** option to **Capturing**:

![fiddler everywhere capture traffic](2-capturing.png)

2) Open the app you are developing (be it a desktop or a web app). Navigate to where the failed request occurs and use whatever functionality necessary in the app to fire the remote request you are trying to debug.

3) Back in Fiddler Everywhere, toggle the **Live Traffic** option to **Paused** so as to limit the flood of information coming in.

4) Find the specific session you are interested in. In my case, I'll use the **Host filter** to only show traffic from a specific remote host, making it far easier to identify and work with the issue at hand:

![fiddler everywhere host filtering](2-host-filtering.png)

5) Look for the failed request out of all your captured sessions. When you find it, inspect the request - look for any improper parameters in the header or body of the request.

![fiddler inspect request](2-inspect-request.png)

6) Next, load the session in Fiddler Everywhere's **API Composer**. Do this by right-clicking on the specific request and choose **Copy --> URL**.

7) In the **API Composer**, alter whatever possible parameters or mistakes you think you've found. Re-issue the request in the API Composer without going to a third party tool as many times as you'd like.

![1-api-composer.png

8) Once you've diagnosed the issue, replace the problem code in your app, and try running the process again.

## Summary

Today we took a quick look at how Fiddler Everywhere can be used to quickly diagnose and fix issues found when remote APIs fail. Start your journey with Fiddler Everywhere by downloading it today on either macOS, Linux, or Windows.