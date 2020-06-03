# Debugging with Fiddler Everywhere: Collaborative Debugging

Fiddler has been used (and loved) for years as the go-to tool for diagnosing and debugging network issues on both web and desktop apps. Fiddler Everywhere is Fiddler vNext - a new cross-platform version of Fiddler that shares the same core engine, but with a modern UI and improved experience.

In this blog series, we are looking into a variety of real world network debugging scenarios that can be effectively addressed with Fiddler Everywhere. Today we are going to look at something *slightly* different - how we might use some of the approaches we've already discussed, but in a team environment.

Let's get going!

> **NOTE:** Fiddler Classic (the original Fiddler) isn't going anywhere! You can still download Fiddler and use it like you always have on Windows.

## Today's Scenario

As a developer, I work as part of an engineering team. Individually we use Fiddler Everywhere to inspect network traffic, but there are times when we want to share pre-recorded sessions with each other. Maybe we have a QA team that records problems and wants to comment on their saved sessions and send them to us for resolution.

Let's see exactly how Fiddler Everywhere can handle this situation.

## Fiddler Everywhere's Solution

Previously with Fiddler Classic we were able to "share" sessions via a relatively tedious inspect -> save -> share -> load -> re-run workflow across multiple desktops. Not fun. But by using Fiddler Everywhere, we can utilize all the same request/response inspection features we've been using for years. One thing Fiddler Everywhere does well that Fiddler Classic can't do at all, is easily share recorded sessions in the app itself.

1) Open Fiddler Everywhere and toggle the **Live Traffic** option to **capturing**:

IMAGE

2) Open the web/desktop app and follow whatever workflow in the app that you want to record and share.

3) Back in Fiddler Everywhere, toggle the **Live Traffic** option to **paused** so as to limit new sessions coming in.

4) In the sessions panel, we can right-click and choose to share selected/un-selected/all sessions:

IMAGE

IMAGE

5) Enter the email addresses of your team members in the dialog provided (along with any comments/context you'd like to add).

IMAGE

6) Your teammate(s) will receive an email notifying them of the shared session. They can now inspect and reply the session...even comment on it so you can collaborate on the issue!

IMAGE

## Summary

We took a quick look at the journey you might take to use Fiddler Everywhere to quickly share recorded sessions with your teammates. By leveraging this feature of Fiddler Everywhere, you enable your team to more easily collaborate on network debugging sessions - often saving numerous cycles for both your developers and QA team!

Start your journey with Fiddler Everywhere by downloading it today on either macOS, Linux, or Windows!