# NativeScript Licensing Explained

Open source software licensing is about as sexy as me in a bikini (hint: am a male with a great farmer's tan). However, the *code* that our OSS licenses protect are the reasons many of us are here today. These licenses help to keep OSS projects free, customizable, and allow for lawyer-free integration with existing software packages across all platforms.

It wasn't always so easy though. Back in the early 90's, "open source" was still cast aside as a realm for Linux geeks and developers who wanted to give away their software for free. What a crazy idea! Luckily for them, the GNU General Public License (GPL) provided some great initial support of their work. Licensing started to change and evolve as developers (and corporations) crafted strategies to protect their IP (intellectual property).

IMAGE

Fast forward to 2014, when work on NativeScript began. We had an opportunity at the time to license our software in the most friendly manner possible, while still protecting our IP and the company (Telerik at that time, Progress now) who was sponsoring the development. Our answer then, as it still is today, is the Apache 2 license.

We aren't here today to spread FUD about competing frameworks and their licensing issues (whether legitimate or negligible is up to you and/or your company to decide). We do however want to focus on the licensing model NativeScript utilizes and what that means for you.

## What are the Apache 2 Terms and Conditions?

Let me start by stating that I'm no lawyer and you should really consult someone with OSS legal knowledge if you have detailed questions. However, at a high level, I can provide a summary of the Apache 2 terms:

**Apache 2 is a permissive license.** What does this mean? It means you can actually release a modified version of NativeScript under any license of your choice. However, any unmodified parts must retain the original Apache License.

**Apache 2 is business-friendly.** Apache 2 is *not* a copyleft license. The popular GNU GPL, which is a copyleft license, requires software that uses any GPL-licensed component to release its full source code and all rights to modify and distribute the code. This is not a business-friendly way to license your OSS project, since if an internal project of yours uses a GPL-licensed component, you are legally required to release your entire project as open source.

One of my favorite legal websites (there are so many fun ones!), tldrlegal.com, provides a [great summary of Apache 2](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)) (and other licenses):

IMAGE

## How Does Apache 2 Differ from BSD?

The BSD license is also a permissible license that allows you to modify and distribute software as you like.

Interestingly, early versions of the Apache license were very similar to the original BSD licenses, but Apache 2 differentiates them a bit:

- Apache 2 grants explicit patent rights while using/modifying/distributing the licensed software;
- Apache 2 is very explicit in the terms/concepts that it uses. There is very little legal ambiguity when compared to BSD;
- Apache 2 is convenient in that it can be used by other projects without rewording. You just drop it in and go!

It gets a little more murky when companies add their own clauses to licenses, such as BSD+. But remember, we aren't going to go there :).

## What about the MIT License?

The MIT license...

MIT is one of the most permissive free software licenses. Basically, you can do whatever you want with a software licensed under the MIT license - just make sure that you add a copy of the original MIT license and copyright notice to it.
The Apache License is also a permissive license. However, it has stringent terms when it comes to modifications. It requires you to explicitly list out all the modifications that you’ve done in the original software, i.e., you’re required to preserve modification notices. The Apache License also states clearly that you can’t name your product in any way that hints at the product being endorsed by Apache. So you can call your product “SuperWonderServer powered by Apache” but not “Apache SuperWonderServer”. The MIT license doesn’t impose any such terms.
The MIT license is also gaining popularity with developers due to its short and clear license agreement, in contrast to the Apache license agreement. Although the Apache License is nearly identical in terms of what you can and can't do, it is much more heavily "lawyered" and is significantly more verbose. Heck, the appendix alone, which explains how to apply the license, is longer than the entire MIT license.

## Can I License or Sell Code Created under Apache 2?

Yes. The Apache License 2.0 allows you to release your own software under the Apache license. Yes. You can sell any Apache licensed software/code.

## So What Does It Mean for Me?

You can use NativeScript for your own purposes or commercially. You have the ability to modify it, redistribute it, even sublicense the framework itself. However, you cannot hold Progress liable for any damages incurred while using the software, nor can you use the NativeScript trademark without explicit permission from Progress.

TLDR LEGAL IMAGE FULL?

## Future Considerations

•	We consider talking about whether we would dual license with the MIT license if enough people needed it