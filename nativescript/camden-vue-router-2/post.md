# Getting Your Route On with NativeScript-Vue: Episode Two

In my [last article on NativeScript-Vue routing](https://www.nativescript.org/blog/getting-your-route-on-with-nativescript-vue-episode-one), I discussed how to do basic routing in [NativeScript-Vue](https://nativescript-vue.org/) applications. I described the simple baked-in routing available via the [manual routing](https://nativescript-vue.org/en/docs/routing/manual-routing/) feature and how it let you move to, and from, different pages in your application. 

In this article, I'm going to describe a different technique. Instead of changing the entire page (i.e. from a "List" page to a "Detail" page), I'm going to describe a tab-like interface where a navigation UI of some sort (think tabs at the bottom of the screen) load different components above them.

As before, I want to thank [Jen Looper](https://www.jenlooper.com/) for her help!

## The "Is Component" Method

One of the more interesting things Vue can do is work with [dynamic components](https://vuejs.org/v2/guide/components-dynamic-async.html). This works via the `:is` declaration. So for example:

	<component :is="someComponent" />

In this example, the basic `component` tag will be rendered as whatever the value of `someComponent` is. You can even switch this dynamically. If `someComponent` pointed to a `Cat` component, but then later to a `Dog`, Vue will handle updating the display for the new component automatically. I wrote up an [example](https://www.raymondcamden.com/2018/10/31/working-with-dynamic-components-in-vuejs) of this a few months ago using dynamic form fields.

This technique gives us an interesting way of doing navigation as well. If we consider the `<component>` tag as our "view", we can then "navigate" by simply swapping out the current value of the `is` property. 

Let's look at an example of this in action.

First, our main "home" component:

	<template>
	    <Page class="page">
	        <ActionBar :title="currentComponent" class="action-bar" />
			<GridLayout rows="2*, auto" columns="*, *, *">
				<component v-for="component in viewsArray" v-show="component === currentComponent" :is="component" 
	            row="0" col="0" colSpan="3"/>
				<Button text='Info' @tap="currentComponent = 'CatInfo'" row="1" col="0" />
	            <Button text='Pic' @tap="currentComponent = 'CatPic'" row="1" col="1" />
	            <Button text='Log' @tap="currentComponent = 'CatLog'" row="1" col="2" />
		    </GridLayout>
	    </Page>
	</template>
	
	<script>
	import CatInfo from './CatInfo';
	import CatPic from './CatPic';
	import CatLog from './CatLog';
	
	export default {
	    data () {
	        return {
	            viewsArray: [ 'CatInfo', 'CatPic', 'CatLog' ],
	            currentComponent: 'CatInfo'
	        };
	    },
	    components: {
	        CatInfo, CatPic, CatLog
	    }
	}
	</script>

Let's start with the markup. First, note the use of a `GridLayout`. The idea here is to set the top portion of the app for our "view" and the bottm portion for navigation. The first element in the `GridLayout ` is the `component` tag. Technically multiple tags as you can see a `v-for` in use there. We don't want three views at once, so how does this work? First note that a `v-show` is being used to ensure we only see one at a time. Then note that the same position information (`row` and `col`) such that the components basically stack on one another. Finally, the three buttons at the botton handle the navigation.

Now let's look at the code. The set of possible views are defined in an array called `viewsArray`. This is what is bound to the `component` tag above. Then there's a simple string value called `currentComponent`. If you go back to the buttons you'll see that the logic there just changes the value of the string. 

The net result is that you can tap each button to swap out what the current, visible, view is. While the views are nothing special, here's `CatInfo`:

	<template>
	    <Label text="cat info!" />
	</template>
	
	<script>
	export default {
	    data() {
		    return {
		    };
	    }
	}
	</script>

And here's one more example, `CatPic`:

	<template>
	    <Image src="https://placekitten.com/400/500" />
	</template>
	
	<script>
	export default {
	    data() {
		    return {
		    };
	    }
	}
	</script>

And finally, one screenshot of the application in action:

![Screen shot of the application with the pic tab](shot1.png)

You can see the entire application, and test it out on your device, in [this NativeScript Playground project](https://play.nativescript.org/?id=9fC1ZQ&template=play-vue&v=5). For another example of this technique, check out Jen Looper's example [here](https://play.nativescript.org/?id=pzaaDo).

## Using Frames

For the next example, we're going to do something a bit more low level than previous examples. Each NativeScript application uses a core UI element called a `Frame`. A `Frame` is where your `Page` components are rendered. You may not have even noticed this before. In a default NativeScript Vue application, your `app.js` file will look like this:

	import Vue from 'nativescript-vue';
	
	import HelloWorld from './components/HelloWorld';
	
	// Uncommment the following to see NativeScript-Vue output logs
	// Vue.config.silent = false;
	
	new Vue({
	
	    template: `
	        <Frame>
	            <HelloWorld />
	        </Frame>`,
	
	    components: {
	        HelloWorld
	    }
	}).$start();

Note the `template` attribute. It defines the core layout to be wrapped by a `Frame` tag. The `HelloWorld` component itself is wrapped in a `Page`. However, you are allowed to modify this layout to create more advanced layouts and navigation schemes. So for example, you could have multiple frames, each with their own page, and use navigation for either of them. Each frame will have it's own navigation history and you can go back and forward in each. 

For the next version of our tabbed application, we're still going to use one frame, but we're going to modify the core layout such that the frame is setup in the first component of the application. 

First, here's the `app.js` file:

	import Vue from 'nativescript-vue';
	
	import Home from './components/Home';
	
	// Uncommment the following to see NativeScript-Vue output logs
	//Vue.config.silent = false;
	
	new Vue({
	    render: h => h(Home)
	}).$start();

Notice there is no template, but instead we just render our main component, `Home`. Now let's look at that.

	<template>
	    <GridLayout rows="*, auto">
	        <ContentView row="0">
	            <Frame>
	                <CatInfo />
	            </Frame>
	        </ContentView>
	
	        <Nav row="1" />
	    </GridLayout>
	</template>
	
	<script>
	import CatInfo from './CatInfo';
	import Nav from './Nav';
	
	export default {
	    data() {
	        return {};
	    },
	    components: {
	        CatInfo, Nav
	    }
	};
	</script>

Like before, we want to end up with a "view" on top and a navigation bar on bottom. (And to be clear, that's arbitrary. Your tabs could be on top, the right, whatever you want.) Note that the use of `ContentView` and `Frame` for the top portion of the grid. This is going to be where our content is rendered. Since the first "page" of our application is hard coded, it's just typed directly in there. Our navigation is now broken out into it's own component, `Nav`:

	<template>
	
	    <GridLayout rows="auto" columns="*, *, *">
	        <Button text='Information' @tap="goTo('info')" row="1" col="0" />
	        <Button text='Picture' @tap="goTo('pic')" row="1" col="1" />
	        <Button text='Log' @tap="goTo('log')" row="1" col="2" />
	    </GridLayout>
	
	</template>
	
	<script>
	import CatInfo from "./CatInfo";
	import CatPic from "./CatPic";
	import CatLog from "./CatLog";
	
	export default {
	    data() {
	        return {
	            routes: {
	                info: CatInfo,
	                pic: CatPic,
	                log: CatLog
	            }
	        };
	    },
	    methods: {
	        goTo(s) {
	            this.$navigateTo(this.routes[s]);
	        }
	    }
	};
	</script>

As with the previous example, the tab bar is defined as a set of buttons. This time we're using a method, `goTo`, and we pass the name of a route to navigate to. The `$navigateTo` method is used to handle loading our view and since our application only has one `Frame` tag, it will automatically load the component within it. One of the arguments you can use with `$navigateTo` lets you specify a frame if you have more than one. 

As before, each page is pretty much the same, but note that we now need to make use of `Page` components. Here's the `CatInfo` component:

	<template>
	
	    <Page class="page">
	        <ActionBar title="Cat Information" />
	        <Label text="Information about the cat. Lorem ipsum meow." />
	    </Page>
	
	</template>
	
	<script>
	export default {
	    data() {
		    return {
		    };
	    }
	}
	</script>

And here's the `CatPic` one:

	<template>
	
	    <Page class="page">
	        <ActionBar title="Cat Picture" />
	        <Image src="https://placekitten.com/400/500" />
	    </Page>
	
	</template>
	
	<script>
	    export default {
	        data() {
	            return {};
	        }
	    };
	</script>

As before, you can see the entire application in [this NativeScript Playground project](https://play.nativescript.org/?id=XKyouQ&template=play-vue&v=4). Also, you can see Jen Looper's cooler example [here](https://play.nativescript.org/?id=albgKn). One thing I like in particular about her example is that the "routes" are further broken out making the navigation component a bit more flexible.

## Going Further

I hope these examples (and those in the [previous article](https://www.nativescript.org/blog/getting-your-route-on-with-nativescript-vue-episode-one)) help you build more robust NativeScript-Vue applications.

If you want an even more advanced example, you should check out the [nativescript-vue-navigator](https://github.com/nativescript-vue/nativescript-vue-navigator) plugin. It provies an experience much closer to "traditional" Vue routing and could be easier to pick up. (And as before, Jen has a [great example](https://play.nativescript.org/?id=jYhwRU) of it in action!)