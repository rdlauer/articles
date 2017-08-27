# NativeScript Licensing Explained

Open source software (OSS) licensing is about as sexy as me in a bikini (hint: am a dude with a great farmer's tan). However, the *code* that OSS licenses protect is quite literally critical to most of us performing our jobs. These licenses help keep OSS projects free, customizable, and allow for lawyer-free integration with existing software across all platforms and industries.

It wasn't always so easy though. Back in the early 90's, the term "open source" was generally limited to Linux geeks and altrusitic developers who wanted to give away their software. The insanity! Luckily for them, the GNU General Public License (GPL) helped provide a basis of liability protection for them and their work. As the success of these early OSS projects became more widely known, licensing started to change and evolve as both indie developers and larger organizations crafted strategies to protect themselves and their intellectual property.

IMAGE OF GPL? GNU?

Fast forward to 2014, when work on NativeScript began. During this time, many great OSS licensing options emerged and matured, primarily MIT, Apache 2.0 (ASLv2), and BSD.

The NativeScript team realized the had an opportunity to license the software in the most friendly manner possible, while still protecting IP and the company (Telerik at that time, Progress now) sponsoring development. **Our answer then, as it still is today, is the Apache 2.0 license.**

We aren't here to spread FUD about competing frameworks and their licensing issues (whether legitimate or negligible is up to you and/or your company to decide). We do however want to focus on the ASLv2 license and what that means for you.

## First, Why We Chose ASLv2

Being a good community citizen in the OSS world means starting with a permissive license that also allows for a certain amount of legal protection (primary from patent trolls!).

IMAGE PATENT TROLL??

The ASLv2 license provides a great balance between free/permissive use of the code, while also protecting the NativeScript brand. ASLv2 prevents someone from literally cloning NativeScript (including naming/branding) and redistributing it on their own. 

ASLv2 is a lot like the equally popular MIT license. It provides most of the benefits of the MIT license, while still protecting the parent company. More on this in a bit. Needless to say, choosing ASLv2 was an easy decision.

## What are the ASLv2 Terms and Conditions?

> Let me start by stating that I'm no lawyer and you should really consult someone with OSS legal knowledge if you have question
At a high level, the two key points of ASLv2 are:

**Apache 2.0 is a permissive license.** The *permissive* aspect of ASLv2 means you may alter/edit/modify/change NativeScript to your heart's content and release said version under any license of your choice. **However, any unmodified parts must retain the original Apache License.**

**Apache 2.0 is business-friendly.** Apache 2 is *not* a [copyleft license](https://en.wikipedia.org/wiki/Copyleft). GNU GPL, which is a copyleft license, requires software that uses any GPL-licensed component to release its full source code and all rights to modify and distribute the code. This is *not* a business-friendly way to license your OSS project - if an internal project of yours uses a GPL-licensed component, you are legally required to open source your entire project.

One of my favorite legal websites (but there are so many fun ones!), tldrlegal.com, provides a [great summary of Apache 2](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)) (and other licenses):

IMAGE

## What About BSD?

The BSD license is another widely used (and permissible) license that allows you to modify and distribute software as you like.

Interestingly, early versions of the Apache license were very similar to the original BSD licenses, but ASLv2 differentiates them a bit:

- ASLv2 grants explicit patent rights while using/modifying/distributing the licensed software;
- ASLv2 is very explicit in the terms/concepts that it uses. There is very little legal ambiguity when compared to BSD;
- ASLv2 is convenient in that it can be used by other projects without rewording. You just drop it in and go!

It gets a little more murky when companies add their own clauses. But remember, we aren't going to go there 😅.

## And Why Not Use MIT?

The MIT license is probably the most permissive OSS license. It's also one of the most widely used license, especially within smaller organizations and individual developers.

Basically MIT-licensed software let's you do literally anything you want to with that software - just make sure that you add a copy of the original MIT license and copyright notice. You just can't sue the creator!

The MIT license is very attractive due to its short, easy to comprehend, license agreement (in contrast to ASLv2 and BSD, which are a bit more wordy 📓).

> The ASLv2 appendix, which explains *how* to apply the license, is longer than the entire MIT license.

## Can I License/Sell Code Created under ASLv2?

Yes! ASLv2 allows you to release your own software under the Apache license. And yes! You can sell any ASLv2-licensed software/code.

## So What Does It All Mean for Me?

In terms of your relationship to Progress and NativeScript, you can use NativeScript for your own purposes or commercially, for whatever you dream up. You even have the ability to modify the framework, redistribute it, and sublicense the framework itself. However, you cannot hold Progress liable for any damages incurred while using the software, nor can you use the NativeScript trademark without explicit permission from Progress. Easy enough!

> Remember to check out tldrlegal.com for a full run-down of the ASLv2 license!

TLDR LEGAL IMAGE FULL?

## Future Considerations

While NativeScript is available under ASLv2 today, that doesn't necessarily mean nothing will change in the future. But not in a bad way! We have considered other options like a dual license under both MIT and ASLv2, if the community requires it. Any other thoughts on NativeScript licensing? Sound off in the comments!