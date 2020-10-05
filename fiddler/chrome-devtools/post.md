# Chrome DevTools and Fiddler - The Perfect Web Debugging Combo

As web developers today, we are incredibly spoiled. Remember Firebug? Remember the days before Firebug? Now some of the best web inspection and debugging tooling comes installed with our favorite browsers.

IMAGE

The developer tooling packed into modern browsers have become critical tools during development and testing phases of our app development cycle. Chromium-based browsers (such as Google Chrome, Microsoft Edge, and Opera?) have their branded DevTools, while Firefox has ??? and Safari has ???. All perform relatively similar functions. They let you traverse the DOM, alter elements and styles on the fly, view network requests, check responsive designs, and measure performance and memory usage (among others).

IMAGE

But (and you knew this was coming) our browser-based developer tools only take us so far. There is only so much you can do today, which is why I propose today that Fiddler Everywhere is the perfect companion to our cherished DevTools.

Let's take a closer look at the Chrome DevTools, investigate what it can do, while seeing what overlap and additional capabilities Fiddler Everywhere can provide.

## Chrome DevTools in a Nutshell

While I'm guessing the vast majority of you are already familiar with at least the most rudimentary usage of the Chrome DevTools, let's make sure we're all clear on what they can provide.

The Chrome DevTools is a sidebar set of developer tooling built directly into Chromium-based browsers. Accessed via `F12`, or `Option + ⌘ + C` on macOS, or `Shift + Ctrl + C` on Windows/Linux. The first thing you'll notice are the series of tabs, or panels, that contain individual sets of developer tooling:

- **Elements** panel lets you inspect and alter the page DOM and CSS.
- **Console** panel allows you to view console messages and even execute ad hoc JavaScript.
- **Sources** panel helps you to debug JavaScript (breakpoints FTW) and save changes that you make to your site in the DevTools.
- **Network** panel gives you the ability to monitor and debug network activity.
- **Performance** panel helps you to improve the load time and overall performance of your website.
- **Memory** panel can profile your app's memory usage over time.
- **Application** panel lets you inspect other resources like embedded databases, local storage, cookies, images, and so on.
- **Security** panel can help you debug security issues like problem TLS certificates.
- **Lighthouse** panel is a great utility for measuring your site's performance.

> **NOTE:** If you'd like to see how Lighthouse and Fiddler work together to improve your site performance, check out this article.

So...that's a lot. And if you're like me, you likely don't do much beyond the Elements and Console panels. Maybe dabble in the Network panel if I'm feeling extra ambitious.

Ultimately, the Chrome DevTools helps you to inspect and edit your site in realtime, diagnose and debug issues, which all leads to you building better web apps faster than ever before.

Under what scenarios am I diving into the Chrome DevTools you might wonder? Aside from the default behaviors of inspecting the DOM, setting some JavaScript breakpoints, and tweaking my styles, there are a myriad other opportunities to improve your app dev workflow.

Did you know...

https://www.keycdn.com/blog/chrome-devtools

1) The Chrome DevTools have a "prettify" feature built in to allow you to read minimized code? Just click on the "{}" button when inspecting a minified file:

IMAGE

2) You can also search your code from the Source panel:

IMAGE

3) I've mentioned this already, but setting breakpoints for debugging JavaScript code is a critical component:

IMAGE

4) Quickly clear all of your cookies in the Application panel:

IMAGE

5) You can't overlook the device mode, which lets you test the responsive design capabilities of your website:

IMAGE

> **NOTE:** The Chrome DevTools are so flexible, that frameworks like NativeScript allow you to tap into this tooling to inspect and debug native mobile apps on iOS and Android!

We could go on and on about the capabilities built in to the Chrome DevTools, but let's take a look at Fiddler Everywhere and see how this new breed of tooling can build on what we do today in our browser.

**Quick pro tip!** If you want to auto-open the Chrome DevTools for every tab on macOS, just pass the `--auto-open-devtools-for-tabs` flag:

	/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --auto-open-devtools-for-tabs

## Fiddler Everywhere in Two Minutes

Fiddler has been a household name for over 15 years now as a robust Windows desktop app for inspecting and debugging network requests and responses. While the original Fiddler (now dubbed "Fiddler Classic") continues to provide value for Windows developers, those of us who spend more time on macOS or Linux were kind of left out in the cold.

This is why Fiddler Everywhere was born. The vision of Fiddler Everywhere is to provide the same feature set of Fiddler Classic, but in a cross-platform package (with additional team collaboration features baked in).

> **NOTE:** Are you an existing Fiddler Classic user? Have no fear, Fiddler Classic isn't going anywhere! You can still download it here.

Fiddler Everywhere is a web debugging proxy that lets you monitor all HTTP/S traffic between your computer and the Internet. This means you can inspect network traffic, save and share traffic sessions, compose API requests, and create auto-responding rules that are triggered under certain user-specified conditions. All wrapped up in a friendly cross-platform desktop app that works just as well on macOS, Linux, and Windows.

IMAGE

Some of the key feature of Fiddler Everywhere include:

- **Traffic Inspector:** Save and reply network sessions, filter traffic, and dive into traffic metrics.
- **Auto Responder:** Mock responses to network requests and simulate network latency.
- **API Composer:** Copy a traffic session and manually tweak the headers/parameters for inspection in the composer.
- **Team Collaboration:** Share saved sessions with your team for easy (and secure) collaboration on issues.

Now some of this sounds a little like with your Chrome DevTools are already doing. It's true, but only to an extent. Let's take a look at how the two tools combined can provide more value than you might first expect.

## Chrome DevTools + Fiddler = Heart





Chrome DevTools looks fairly similar to a web debugging proxy like Fiddler, showing a list of sessions and providing detailed information about their contents. Not only that, but it also gives a huge wealth of additional information about the webpage you’re currently viewing, it’s built directly into a very commonly-used browser, and it’s totally free. You don’t even need to download anything, just learn where the button is or what the keyboard shortcut is to bring it up. (It’s Ctrl-Shift-I, or Cmd-Shift-I on Macs.) All of this makes it an incredibly useful and ubiquitous tool for web developers, and you’d think that its existence would make separate proxy programs unnecessary.

However, there is an important difference between what a proxy like Fiddler does and what Chrome DevTools does. To find out what it is, and what it means for which one of them to use, we have to look a little deeper at what, exactly, the data that each of them display means.





What does that mean for Chrome DevTools vs. Fiddler?
The important thing to note here is that the information you can get from Chrome DevTools is not the same as the information Fiddler gives you. It’s a little hard to see because there’s just so much of it, but the Chrome DevTools info is all very specific to the webpage you’re viewing. You can see the DOM for the webpage, the JavaScript for the webpage, the requests the webpage made while it was loading and their timing data, the cookies on the webpage, and so on; but there’s nothing at all about anything besides this particular page. Fiddler, on the other hand, doesn’t show nearly as much detail per webpage, but it isn’t limited to one page at a time. You can see all the Internet traffic for your machine, write requests to any website you can think of, and change around server responses in a whole load of ways. Fiddler is essentially an expanded, broader-scope version of Chrome DevTools’s Network tab.

Because Chrome DevTools essentially operates on a level below Fiddler—the webpage level rather than the computer level—the question of “which one is better” doesn’t apply, because they’re not in competition with each other. They do jobs with a slight but very important difference. Fiddler is good for seeing how a page reacts to outside pressures, like bad requests and jumbled responses, and for monitoring how it interacts with the rest of the Internet. Chrome DevTools, on the other hand, is all about looking inside a page, seeing what makes it tick, and checking if there are any screws loose. They both provide valuable information at different stages of the web development process, which means that they work best when used in tandem. In fact, there are quite a few ways in which they interact directly. For example, one of my favorite things about Fiddler's Auto Responder is that you can set up a rule that replaces a picture on a webpage with a picture of your choice. To construct this rule, you need to find the part of the page’s DOM that contains the image you want to replace, so you can put that code in the conditional part of the rule. Wouldn’t you know it, the DevTools Elements tab has the DOM right there, and shows you directly what parts of the code correspond to what parts of the page. It’s like it was meant to be.

I know it’s tempting to think that there’s one method you can use for everything, but the truth is that the more different answers you have, the more of the bigger picture you’ll see. Debugging a website is like a jigsaw puzzle; Fiddler can only give you some of the pieces. Chrome DevTools might give you some of the same ones, but it’ll give you a bunch of its own that you can’t get anywhere else. Use them both, learn how their answers fit together, and you’d be surprised at what you’ll be able to see.