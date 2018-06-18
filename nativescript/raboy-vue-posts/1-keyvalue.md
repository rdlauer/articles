# Key-Value Local Storage in a Vue.js NativeScript App with Vuex

description = "Learn how to use Vuex within a NativeScript with Vue.js mobile application to manage data saved as local key-value storage with the application-settings module."

If you've ever built a Vue.js application, whether that be web or mobile, you've probably come across [Vuex](https://vuex.vuejs.org/) when searching for data storage related information. When it comes to the web, the common tutorial around Vuex includes browser local storage or similar, where Vuex handles all of the state rules between components.

With [NativeScript supporting Vue.js](https://nativescript-vue.org/) as of now, this opens the door in what we can do for mobile development, but how do we handle data under these circumstances? NativeScript doesn't use a browser so local storage isn't an option.

We're going to see how to manage data in an Android and iOS mobile application built with NativeScript and [Vue.js](https://vuejs.org/). This application will leverage the power of Vuex and NativeScript's built in [Application Settings](https://docs.nativescript.org/cookbook/application-settings) module.

Before we get into the code, let's take a step back and figure out what each of our proposed modules offer us when it comes to building a mobile application with NativeScript.

Per the [Vue.js website](https://vuex.vuejs.org/), Vuex can be described as the following:

> Vuex is a state management pattern + library for Vue.js applications. It serves as a centralized store for all the components in an application, with rules ensuring that the state can only be mutated in a predictable fashion.

There are many different ways to store data in a NativeScript application. For this example we'll be using key-value storage made possible by the [Application Settings](https://docs.nativescript.org/cookbook/application-settings) module which uses `SharePreferences` on Android and `NSUserDefaults` on iOS. In a future tutorial we'll explore how to use SQLite or similar.

This tutorial was actually inspired by a web tutorial I read titled, [Vue: Using localStorage with Vuex Store](https://www.mikestreety.co.uk/blog/vue-js-using-localstorage-with-the-vuex-store).

## Creating a NativeScript with Vue.js Project that Supports Vuex

For this tutorial to be successful, you'll need NativeScript installed and configured on your computer as well as the Vue CLI as outlined in the [NativeScript documentation](https://nativescript-vue.org/en/docs/getting-started/templates/). The commands to create our project will be as follows:

	vue init nativescript-vue/vue-cli-template vuex-project
	cd vuex-project
	npm install
	npm run watch:ios

The above commands will create a project called **vuex-project** with the necessary dependencies. When creating the project, make sure that Vuex is enabled when prompted as it will make our lives easier. The defaults are fine for all other prompts in the configuration process.

## Defining the Vuex State Logic

Now that we have a NativeScript with Vue.js project created and ready to go, we can focus on developing our Vuex logic for managing our application state. In my opinion, the generator used when creating a new project, is a bit wonky. For this reason, we're going to go against the template a little in an effort to match what we'd see for Vue.js on the web.

In the project's **src/store/index.js** file, include the following JavaScript:

	import Vue from 'nativescript-vue';
	import Vuex from 'vuex';
	import * as ApplicationSettings from "application-settings";
	
	Vue.use(Vuex);
	
	const store = new Vuex.Store({
	    state: {
	        firstname: "",
	        lastname: ""
	    },
	    mutations: {
	        load(state) {
	            if(ApplicationSettings.getString("store")) {
	                this.replaceState(
	                    Object.assign(state, JSON.parse(ApplicationSettings.getString("store")))
	                );
	            }
	        },
	        save(state, data) {
	            state.firstname = data.firstname;
	            state.lastname = data.lastname;
	        }
	    }
	});
	
	Vue.prototype.$store = store;
	
	module.exports = store;

Before we discuss what the above means, how about I mention what I've changed. Instead of referencing a module in the **src/store/index.js** file, I've included what would have been in the module, directly in the file.

*So what is happening in the code above?*

First we're defining the data that can be managed in our store. We're going to be managing first name and last name information for this example, something that is rather simple. By default, the data will be empty.

We're defining two mutations that can be used to alter the state of the application.

	load(state) {
	    if(ApplicationSettings.getString("store")) {
	        this.replaceState(
	            Object.assign(state, JSON.parse(ApplicationSettings.getString("store")))
	        );
	    }
	},

The `load` mutation will first check to see if anything was previously saved in what we're calling the `store` key. To keep our data flexible, we don't want to restrict ourselves to only what exists in our `state` variable. Instead, if data was previously saved, we are replacing the current state with a merged copy of what exists in the state and what exists in the storage. The `replaceState` function is part of the Vuex API.

	save(state, data) {
	    state.firstname = data.firstname;
	    state.lastname = data.lastname;
	}

The `save` mutation will take data committed to the mutation and save it in our state. You'll notice that we're not actually saving any data to storage in the Vuex controller, but we could. Instead, the actual saving will happen via a subscription in our component.

## Building a Vue.js Component in NativeScript with Data Requirements

We want to see the Vuex magic in action and we also want to actually persist data rather than just manage while the application is open and being used. For this reason, we need to work on our component.

When you created your project, you probably had a **src/components/Counter.vue** file. You can either rename, leave it as is, or create a new file. This is an example, so our naming conventions are not too important, but we won't be counting anything.

Open the component and include the following code:

	<template></template>
	
	<script>
	    import * as ApplicationSettings from "application-settings";
	    export default {
	        data() {
	            return {
	                input: {
	                    firstname: "",
	                    lastname: ""
	                }
	            }
	        },
	        mounted() {
	            this.$store.subscribe((mutations, state) => {
	                ApplicationSettings.setString("store", JSON.stringify(state));
	                this.input.firstname = state.firstname;
	                this.input.lastname = state.lastname;
	            });
	        },
	        methods: {
	            save() {
	                this.$store.commit("save", this.input);
	            },
	            load() {
	                this.$store.commit("load");
	            },
	            clear() {
	                this.input.firstname = "";
	                this.input.lastname = "";
	            }
	        }
	    };
	</script>

You can see that I stripped out the `<template>` content for now because we want to focus on the `<script>` content. In short, this component will have a form and three buttons. We can save the content of the form, load the content of the form, or clear the content of the form. The form will allow for first name and last name input.

First we want to initialize our variables in the `data` method. Next, when the application mounts, we want to subscribe to our Vuex store. This is where the actual saving happens. Any time our store changes, this subscription triggers which then persists our state and sets the form content. Remember, this isn't the only strategy towards saving data.

	methods: {
	    save() {
	        this.$store.commit("save", this.input);
	    },
	    load() {
	        this.$store.commit("load");
	    },
	    clear() {
	        this.input.firstname = "";
	        this.input.lastname = "";
	    }
	}

The `save` method will call the `save` mutation found in our Vuex file. We are passing the form input to this mutation. The `load` method will call the `load` mutation found in the Vuex file. Finally, the `clear` method will just wipe out our form in terms of UI, not saved data.

Now let's look at the `<template>` that I previously wiped out:

	<template>
	    <Page class="page">
	        <ActionBar class="action-bar" title="Person"></ActionBar>
	        <StackLayout class="form">
	            <StackLayout class="input-field">
	                <Label text="First Name" class="label font-weight-bold m-b-5" />
	                <TextField class="input" v-model="input.firstname" />
	                <StackLayout class="hr-light"></StackLayout>
	            </StackLayout>
	            <StackLayout class="input-field">
	                <Label text="Last Name" class="label font-weight-bold m-b-5" />
	                <TextField class="input" v-model="input.lastname" />
	                <StackLayout class="hr-light"></StackLayout>
	            </StackLayout>
	            <GridLayout rows="auto, auto" columns="*, *">
	                <Button text="Save" @tap="save" class="btn btn-primary" row="0" col="0" />
	                <Button text="Load" @tap="load" class="btn btn-primary" row="0" col="1"  />
	                <Button text="Clear" @tap="clear" class="btn btn-primary" row="1" col="0" colSpan="2"  />
	            </GridLayout>
	        </StackLayout>
	    </Page>
	</template>

In the `<template>` we have two form fields that are styled with the core NativeScript theme. They are bound using the `v-model` property to the data that we defined in our `<script>` block. Each of the `@tap` events on our buttons will call an appropriate function.

If you changed the file name or created a new file like I mentioned previously, don't forget to update the project's **src/main.js** file.

## Conclusion

You just saw how to use Vuex and a form of persisted local storage in an Android and iOS application created with [NativeScript](https://www.nativescript.org/) and Vue.js. Like I previously mentioned, there are many ways to persist data in NativeScript, but the commonality is that you'll likely be using Vuex when it comes to management.

To see another example of Vuex, I have a web example titled, [A Vue.js App using Axios with Vuex](https://www.thepolyglotdeveloper.com/2018/04/vuejs-app-using-axios-vuex/), that can easily be ported to NativeScript.