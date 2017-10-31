# Resources for Upgrading Your Hybrid App to NativeScript

Yes, in fact, there *is* life after Telerik Platform! While the recent news that Telerik Platform is set to shut down in May has frustrated some, the good news is that Progress is in no way abandoning mobility. Over the past year the NativeScript team has been working hard to release new tools and improve the core framework that allows you to create truly native, cross-platform, apps **using the same web skills** you've been using to create hybrid apps.

> The future of mobile app development is NativeScript, and we are here to help you find you way there.

**Make no mistake: NativeScript is at the core of the Progress mobility strategy going forward.** Progress is dedicating significant resources to make sure your transition from hybrid (i.e. Cordova/PhoneGap) to native (NativeScript) is as seamless as possible.

With that being said, let's make sure we are aware of all the resources available today, for free, to help us migrate from hybrid to native:

## HybridToNative.com

With the online resource [hybridtonative.com](http://www.hybridtonative.com/), we've partnered with the community to provide a guide on upgrading your existing Angular-based hybrid apps to NativeScript.

![nativescript app showcase](nativescript-showcase.png)

This extensive guide contains everything you need to know when comparing hybrid to native. From a comparison of hybrid and native technologies, to a look at using the Ionic CLI vs the NativeScript CLI, to comparing UI implementations, and more, [hybridtonative.com](http://www.hybridtonative.com/) should be your first stop on your upgrade path.

## Migrating from Cordova to NativeScript

In the article, [*Migrating from Cordova to NativeScript*](https://developer.telerik.com/content-types/tutorials/migrating-cordova-nativescript/), we take a high level look at taking an existing hybrid app and upgrading it to NativeScript.

By breaking down an existing hybrid app and looking at the views/layouts, JavaScript business logic, and CSS styling, we see how a transition from hybrid to native isn't nearly as difficult as one might think.

For example, we can take this HTML from a hybrid app:

	<header class="header">
	    <h1>todos</h1>
	    <input class="new-todo" placeholder="What needs to be done?">
	</header>
	<section class="main">
	    <ul class="todo-list">
	        <!-- item template goes here -->
	    </ul>
	</section>

...and convert it to a truly native view with NativeScript:

	<StackLayout>
	    <Label text="todos" class="title" />
	    <TextField class="new-item" text="{{ newTodo }}" hint="What needs to be done?" />
	    <Repeater items="{{ todos }}">
	        <!-- item template goes here -->
	    </Repeater>
	</StackLayout>

Take a closer look by reading [*Migrating from Cordova to NativeScript*](https://developer.telerik.com/content-types/tutorials/migrating-cordova-nativescript/).

## Modernizing One Hybrid App at a Time

The NativeScript Developer Relations team put together an online webinar on *Modernizing One Hybrid App at a Time*. This hour-long video contains a wealth of information on the latest in NativeScript tooling, strategies on sharing code between web and mobile, and how to create an engaging user interface with NativeScript.

Watch *Modernizing One Hybrid App at a Time* on YouTube now:

iframe width="560" height="315" src="https://www.youtube.com/embed/US-eudM3gJw" frameborder="0" allowfullscreen></iframe>

## NativeScript Resources

There are plenty of places you can go next to learn more about NativeScript and the new NativeScript tooling options:

- Getting started with NativeScript ([using JavaScript](http://docs.nativescript.org/tutorial/chapter-0) or [using Angular/TypeScript](http://docs.nativescript.org/angular/tutorial/ng-chapter-0));
- Try NativeScript in the browser with the [NativeScript Playground](https://play.nativescript.org/);
- Download [NativeScript Sidekick](https://www.nativescript.org/nativescript-sidekick) and see how easy it can be to bootstrap an app and build in the cloud (all without needing a Mac!).