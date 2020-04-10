# Migrating from Fiddler Classic to Fiddler Everywhere

In the more than 15 years of Fiddler's history, it's become one of the most popular Telerik products. It's also become a victim of it's own success, as it has A LOT of distinct features for A LOT of different type of users!

IMAGE

Long-time users of Fiddler are ok with this, should we say, "cluttered" UI. They know where their features are and how to use them. And you know what, Fiddler is popular because what it does, it does very well.

There are some things that Fiddler "Classic" (what we term the current Windows-only version of Fiddler) doesn't do well. It's not especially inviting to new users due to the aforementioned UI. It also isn't easily accessible to the growing base web and desktop developers on macOS and Linux.

## Fiddler Everywhere FTW

It's primarily out of these concerns that Fiddler Everywhere was born. Fiddler Everywhere is a more focused implementation of Fiddler that provides access to the most-adored features of Fiddler Classic. And it works across all platforms equally well: macOS, Linux, and Windows.

> Suffice it to say, Fiddler Classic isn't going anywhere. You know it, you love it, and we will continue to support it.

Those of you who have been using Fiddler for a long time might be curious about exactly which features are migrating to Fiddler Everywhere, where they are, and how you use them.

## Traffic Inspector

One of the primary uses of Fiddler is to inspect HTTP/HTTPS traffic to and from browsers and desktop apps. This is a critically important feature for Fiddler users, so it was a no-brainer feature to add to Fiddler Everywhere.

*Inspector in Fiddler Classic*

IMAGE

Fiddler (Classic and Everywhere) can log all traffic between your device and any internet-based endpoint. Any application on your computer that supports a system network proxy (all modern browsers and likely any desktop app you are developing) can have its network traffic monitored and debugged.

Fiddler Classic allows you to dive into the details of any specific network request, including:

- Headers: 
- TextView:
- SyntaxView:
- WebForms:
- HexView:
- Auth:
- Cookies:
- Raw:
- JSON and XML:

Likewise, Fiddler Everywhere provides access to a similar set of details:

- TBD
- TBD

*Inspector in Fiddler Everywhere*

IMAGE

## API Composer

Modern apps are usually powered by separately-hosted APIs. Tools such as Postman are widely used to help test and debug these remote API endpoints.

Fiddler Classic provides what is now a bit of a rudimentary API composer:

*API Composer in Fiddler Classic*

IMAGE

With Fiddler Everywhere, we've seen how critical this piece of functionality is in your debugging workflow. So not only have we added this feature from Fiddler Classic, but we have dramatically improved upon it:

*API Composer in Fiddler Everywhere*

IMAGE

With Fiddler Everywhere, you can now edit headers, and ???

## Auto Responder

One of the most useful features of Fiddler (in this writer's humble opinion) is the Auto Responder. Using Auto Responder, you can set up rules to alter specific requests and test out a myriad of scenarios. You can use local file assets instead of transmitting requests to the server and answer questions like: 

- What happens when my CDN goes down?
- Does my app break if the remote API returns a 500 error?
- If images aren't served properly from a server, does it break the layout of my app?
- What about if there is significant latency with my remote APIs?
- And on and on...

*Auto Responder in Fiddler Classic*

IMAGE

Auto Responder is back (with a vengence) in Fiddler Everywhere! Not only can you construct your rules in a more elegant and intuitive manner, but you can also...?

*Auto Responder in Fiddler Everywhere*

IMAGE

## But [Insert Feature Name] is Missing!?!

Clearly Fiddler Classic is chock-full of features it has accumulated over its 15 year lifespan.

![fiddler classic features](classic-features.png)

And not all of them are in Fiddler Everywhere, nor will all of them *ever* be in Fiddler Everywhere. Does this mean you should stop using Fiddler Classic? No! In fact, you can use the two tools in parallel if you need to, especially as Fiddler Everywhere features ramp-up.

## Try Fiddler Everywhere Today