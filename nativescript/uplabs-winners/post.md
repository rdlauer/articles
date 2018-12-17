# So Who Won the NativeScript Uplabs Challenge?

A couple of months ago [NativeScript and Uplabs teamed up](https://www.uplabs.com/challenges/nativescript-uplabs-challenge/results) to challenge developers and designers to build the very best native app UIs, using NativeScript (of course) and the [NativeScript Playground](https://play.nativescript.org/).

We viewed this as a design challenge primarily. Everyone knows you can create *truly native* UIs with NativeScript, but not everyone knows that you can build *engaging* native UIs as well.

![nativescript uplabs challenge example](spacex.gif)

Like many of you, we were very pleased to see the submissions roll in! And to be honest, many of the entries blew us away. I'll admit *I didn't even know* you could build some of these UIs so easily with some NativeScript layout code and CSS.

*From:*

	<GridLayout rows="auto, *" class="intro">
	    <GridLayout class="m-t-30">
	        <StackLayout class="order" [class.order-slide-in]="opening"
	            [class.order-slide-out]="!opening">
	            <Label class="header" text="ORDER"></Label>
	        </StackLayout>
	        <StackLayout class="taker" [class.taker-slide-in]="opening"
	            [class.taker-slide-out]="!opening">
	            <Label class="header" text="TAKER"></Label>
	        </StackLayout>
	    </GridLayout>
	
	    <AbsoluteLayout class="welcome text-center" rowSpan="2">
	        <StackLayout class="overlay center">
	            <Stacklayout class="m-x-15" [class.overlay-zoom-in]="opening"
	                [class.overlay-zoom-out]="!opening"></Stacklayout>
	        </StackLayout>
	        <StackLayout class="center details">
	            <StackLayout orientation="horizontal" horizontalAlignment="center">
	                <Label text="Welcome" class="title text-center m-r-5"></Label>
	                <Image src="~/assets/happy.png" class="happy"></Image>
	            </StackLayout>
	            <Label text="Easy bill split with your friends" class="sub-title text-center m-t-20"></Label>
	
	            <GridLayout class="btn-container">
	                <Button [class.fade-out]="!opening" text="CREATE AN ORDER"
	                    class="m-x-15 welcome-btn" (tap)="newOrder()"></Button>
	                <Image [class.fade-out]="!opening" src="~/assets/plus.png"></Image>
	            </GridLayout>
	        </StackLayout>
	    </AbsoluteLayout>
	</GridLayout>

*To:*

![nativescript uplabs challenge example 2](nativescript-uplabs.png)

As part of the challenge, we offered cash prizes for the top apps. $1,000 for the app with the most votes on Uplabs.com (by the community) and $2,000 to be handed out by a committee of folks on the NativeScript team here at Progress.

**We are now are pleased to announce the winners!**

## $1,000 Community Prize

A big congrats goes to [Yassine Zanina](https://twitter.com/ZaninaYassine) for his Jukebox app, which provides a great UI for browsing artists and songs:

![jukebox app](jukebox.jpg)

Written with NativeScript + Angular, this Jukebox app provides a great starting point for the next Spotify!

You can find a runnable version of this app for both iOS and Android [on the NativeScript Playground](https://play.nativescript.org/?template=play-ng&id=ERHBrE&v=11).

## $2,000 NativeScript Prize(s)

The second prize was determined by a select few of us on the NativeScript team. **And we ended in a split decision!**

So we are pleased to award a $1,000 prize to each of the two following challengers:

Congratulations to [Lucien Noir](https://twitter.com/lucianoir_) and [Clément Roche](https://twitter.com/clementroche_) for their [City Guide](https://www.uplabs.com/posts/city-guide-app-bfc7b703-3734-4a39-a95c-9e99c2b12169) app!

![city guide app](city-guide-app.gif)

This gorgeous app uses [nativescript-vue](https://nativescript-vue.org/) with some truly inventive design patterns and animations to allow you to find some highlights from a variety of international cities.

> You can also view the [complete NativeScript Playground project](https://play.nativescript.org/?template=play-vue&id=uZMYoe&v=42).

Just as important as the design in the interactivity and animations involved to make an app as engaging as possible. This is why we are pleased to award the other $1,000 prize to Harsimran Singh (a.k.a. [Mr. Blade](https://twitter.com/iammrblade)) for his [SocialFit](https://www.uplabs.com/posts/socialfit-nativescript) app:

![socialfit](socialfit.png)

Like the City Guide app, it's another beautiful and engaging cross-platform app that showcases the rich animations and interactions available with NativeScript (with some fantastic charts and graphs!).

Built with NativeScript Core (the vanilla JavaScript version of {N}), you can also [find this app on the NativeScript Playground](https://play.nativescript.org/?template=play-js&id=7wQ8EL&v=5).

## More Templates and Code Samples for You!

These are only 2 of the 37(!) [entries we received](https://www.uplabs.com/challenges/nativescript-uplabs-challenge/results). We hope to move many of these apps over to the [sample apps section on the NativeScript Marketplace](https://market.nativescript.org/?tab=samples&framework=all_frameworks&category=all_samples) in the coming weeks.

We also plan on adding Angular, Vue.js, and JavaScript/TypeScript variations where missing, and provide additional design patterns to make it easier than ever for you to copy-and-paste your way to an engaging app UI with NativeScript. 👩‍🎨🎨