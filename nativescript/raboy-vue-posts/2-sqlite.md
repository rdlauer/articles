# Data Management with SQLite and Vuex in a NativeScript-Vue App

description = "Learn how to use SQLite in an Android and iOS application built with NativeScript and the Vue.js JavaScript framework."

NEEDS UPDATED LINKS!!!!

I recently wrote a tutorial about using Vuex in a NativeScript application that used the Vue.js JavaScript framework. In this tutorial titled, [Key-Value Local Storage in a Vue.js NativeScript Application with Vuex](https://www.nativescript.org/blog/key-value-local-storage-in-a-vue.js-nativescript-app-with-vuex), we saw some examples that made use of a key-value storage that used the Application Settings module for NativeScript.

Having key-value storage is great for certain scenarios, but often you'll find yourself needing something that can be queried like [SQLite](https://www.sqlite.org/index.html).

We're going to take our previous tutorial to the next level by using native SQLite within an Android and iOS application built with [NativeScript + Vue.js](https://nativescript-vue.org/) and Vuex for state management.

If you haven't seen my [previous article](https://www.nativescript.org/blog/key-value-local-storage-in-a-vue.js-nativescript-app-with-vuex), it isn't a requirement to being successful with this article, but it is worth it when learning about Vuex and storage in NativeScript.

## Creating a New NativeScript Application with Vue.js and the Dependencies

To keep things simple and easy to understand, we're going to create a new project that uses NativeScript and Vue.js. To do this, execute the following from the command line:

	vue init nativescript-vue/vue-cli-template vuex-project
	cd vuex-project
	npm install
	npm run watch:ios

The above commands will initialize a new Vue.js project with the NativeScript template, install the immediate project dependencies, and then emulate it on iOS. However, during the creation process with the Vue CLI, you'll be asked a few questions. It is important that you enable Vuex, but everything else can be left as the default.

Once the **vuex-project** project is created, we're not in the clear yet. We need to install a native plugin that will give us SQLite support for Android and iOS.

From the command line, execute the following:

	npm install nativescript-sqlite --save

The above command will install the [SQLite plugin](https://github.com/NathanaelA/nativescript-sqlite/blob/master/src/README.md) for NativeScript.

At this point in time we can start adding logic to our very simplistic application.

## Developing State Management Logic for SQLite with Vuex

If you've never used Vuex before, it is a way to manage the state of your application from a single location. When using the generator to create our project, I feel like it over complicated things in terms of Vuex. For this reason we're going to make some modifications to the project before adding our own Vuex logic.

In the project's **src/store/index.js** file, include the following JavaScript:

	import Vue from 'nativescript-vue';
	import Vuex from 'vuex';
	
	const Sqlite = require("nativescript-sqlite");
	
	Vue.use(Vuex);
	
	const store = new Vuex.Store({
	    state: {
	        database: null,
	        data: []
	    },
	    mutations: { },
	    actions: { }
	});
	
	Vue.prototype.$store = store;
	
	module.exports = store;

So what changed? Instead of referencing a module in the **src/store/index.js** file, I've included what would have been in the module, directly in the file.

Now let's figure out what we want to accomplish with Vuex. The SQLite plugin is asynchronous so we want to maintain a single open instance in our state. We also want to keep our database data loaded and ready to go at all times, and this can happen in the `data` property of the state. These naming conventions are not specific, as long as they exist in the `state` property.

Anytime we want to alter the state variables, we need to do it in a mutation. However, mutations must be synchronous, so we have to make use of actions as well, which can be asynchronous.

So what might our mutations look like? Take a look at the following:

	mutations: {
	    init(state, data) {
	        state.database = data.database;
	    },
	    load(state, data) {
	        state.data = [];
	        for(var i = 0; i < data.data.length; i++) {
	            state.data.push({
	                firstname: data.data[i][0],
	                lastname: data.data[i][1]
	            });
	        }
	    },
	    save(state, data) {
	        state.data.push({
	            firstname: data.data.firstname,
	            lastname: data.data.lastname
	        });
	    },
	}

We have three different mutations in the above code. We have an initialization which will store the state of our open database. When we wish to load our database data, we can loop through the results of a query and store it as an array of objects. It isn't so pleasant to work with SQLite data in its raw form, but it is very easy to work with as JSON. Finally, we have a way for saving data into our state.

Remember, the mutations are only for changing the `state` variables. The actual interaction with our database will happen in the actions:

	actions: {
	    init(context) {
	        (new Sqlite("my.db")).then(db => {
	            db.execSQL("CREATE TABLE IF NOT EXISTS people (id INTEGER PRIMARY KEY AUTOINCREMENT, firstname TEXT, lastname TEXT)").then(id => {
	                context.commit("init", { database: db });
	            }, error => {
	                console.log("CREATE TABLE ERROR", error);
	            });
	        }, error => {
	            console.log("OPEN DB ERROR", error);
	        });
	    },
	    insert(context, data) {
	        context.state.database.execSQL("INSERT INTO people (firstname, lastname) VALUES (?, ?)", [data.firstname, data.lastname]).then(id => {
	            context.commit("save", { data: data });
	        }, error => {
	            console.log("INSERT ERROR", error);
	        });
	    },
	    query(context) {
	        context.state.database.all("SELECT firstname, lastname FROM people", []).then(result => {
	            context.commit("load", { data: result });
	        }, error => {
	            console.log("SELECT ERROR", error);
	        });
	    }
	}

In the actions section we have three different actions, each of which calling one of our mutations in the end. The `init` action will open a database and execute a query for creating a table. When the table is created, the open database is passed to the mutation via the `commit` method. When inserting information, data is passed to the `insert` action and a query is executed. The `context.state.database` variable is our open database in the `state` variables section. The passed data is used as parameters in our query and when it finishes successfully, it is committed to our mutation and the data is saved in the state. Similarly, we can query for data in the database. The result of the query is committed to our mutation so it can be accessed from the component.

Much of the heavy lifting was actual SQL statements. Just remember that the `actions` are for asynchronous code while the `mutations` are for synchronous.

One more thing must be done.

We want to make sure our database is initialized when the application opens. To do this, add the following line to the bottom of your **src/store/index.js** file:

	store.dispatch("init");

Notice that we're using `dispatch` instead of `commit` to call our actions. With Vuex good to go, we can start using it from within any of our components.

## Creating a User Experience with a Vue.js Component

Since this is a simple application, we have two simple options towards creating our component. We can continue to use the project's **src/components/Counter.vue** file or we can rename it to something that makes a bit more sense. Since we're storing people data, I've gone ahead and renamed mine to **src/components/Person.vue**. If you decide to rename your file, just make sure you update it in the **src/main.js** file.

Open your **src/components/Person.vue** file and include the following:

	<template></template>
	
	<script>
	    export default {
	        data() {
	            return {
	                input: {
	                    firstname: "",
	                    lastname: ""
	                }
	            }
	        },
	        methods: {
	            save() {
	                this.$store.dispatch("insert", this.input);
	            },
	            load() {
	                this.$store.dispatch("query");
	            },
	            clear() {
	                this.input.firstname = "";
	                this.input.lastname = "";
	            }
	        }
	    };
	</script>

I've purposefully cleared the `<template>` block for now so we can focus on the `<script>` block. In the `<script>` block we initialize a few variables which will be bound to our HTML form. The methods will be bound to a set of buttons. When we call the `save` method we take the data from the form and send it to the Vuex action for storing into the database. Likewise when we call the `load` method, we call the appropriate action for obtaining data. Finally, the `clear` function will empty our form by clearing the variables. Nothing was too complicated here because Vuex did all the heavy lifting for our SQLite database.

Now let's take a look at the `<template>` block:

	<template>
	    <Page class="page">
	        <ActionBar class="action-bar" title="Person"></ActionBar>
	        <GridLayout rows="auto, *" columns="*">
	            <StackLayout class="form" row="0" col="0">
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
	            <ListView for="person in $store.state.data" class="list-group" row="1" col="0">
	                <v-template>
	                    <StackLayout class="list-group-item">
	                        <Label v-bind:text="person.firstname + ' ' + person.lastname" />
	                    </StackLayout>
	                </v-template>
	            </ListView>
	        </GridLayout>
	    </Page>
	</template>

The template of our page will be a grid. The first row of the grid will be our form and the second will be a list of data within our database. Let's look at the top portion first:

	<StackLayout class="form" row="0" col="0">
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

Essentially we have some input fields and some buttons. The input fields are bound to the variables we had initialized via the `v-model` attribute and the buttons are bound to our methods via the `@tap` attribute. Most of what we have here is standard markup with no real logic.

Now let's look at the second half:

	<ListView for="person in $store.state.data" class="list-group" row="1" col="0">
	    <v-template>
	        <StackLayout class="list-group-item">
	            <Label v-bind:text="person.firstname + ' ' + person.lastname" />
	        </StackLayout>
	    </v-template>
	</ListView>

We have a simple list which is populated from the data in our store. Remember, not only are we storing all our data in the database, but we're keeping it as JSON too.

## Conclusion

You just saw how to use SQLite in a [Vue.js with NativeScript](https://nativescript-vue.org/) application. Using Vuex with SQLite isn't a necessity, but it is the advised way to do things when working with data. Our example was simple in the sense that it only used a single table and some simple queries, but it could become more complex with little extra effort. If Vue.js isn't your jam, I also wrote a tutorial titled, [Using SQLite in a NativeScript Angular Mobile App](https://www.thepolyglotdeveloper.com/2016/10/using-sqlite-in-a-nativescript-angular-2-mobile-app/), that focuses on the same concepts, but with Angular.

If you'd like to use key-value storage rather than SQLite, make sure you check out my [previous article](https://www.nativescript.org/blog/key-value-local-storage-in-a-vue.js-nativescript-app-with-vuex).
