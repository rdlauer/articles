# Make HTTP Requests to Remote Web Services in a NativeScript-Vue App

description = "Learn how to make HTTP requests to remote web services and APIs in an Android and iOS application built with NativeScript and the Vue.js JavaScript framework."

Many, not all, mobile applications act as a client for viewing remote data that might typically appear within a web application. The most common way to consume and transmit data is through HTTP requests that communicate to remote web services or RESTful APIs.

If you've been keeping up with my Vue.js content, you'll remember I wrote a tutorial titled, [Consume Remote API Data via HTTP in a Vue.js Web Application](https://www.thepolyglotdeveloper.com/2017/10/consume-api-data-http-vuejs-web-application/). In that tutorial, the web browser acted as the client and we used [axios](https://github.com/axios/axios) and [vue-resource](https://github.com/pagekit/vue-resource) to make HTTP requests.

In this tutorial, we're going to see how to make HTTP requests within a [NativeScript application built with the Vue.js JavaScript framework](https://nativescript-vue.org/).

Remember, NativeScript will create **native** Android and iOS applications. However, the web method towards making HTTP requests, as demonstrated in my [previous tutorial](https://www.thepolyglotdeveloper.com/2017/10/consume-api-data-http-vuejs-web-application/), will still work. That said, we're going to look at the NativeScript way to do business and revisit the alternatives.

## Create a Fresh NativeScript with Vue.js Project

For simplicity, we're going to make simple HTTP requests within a fresh project. This will help us rule out any existing and potentially incorrect project code. Assuming that you have the [Vue CLI](https://github.com/vuejs/vue-cli) and NativeScript CLI installed, execute the following:

	vue init nativescript-vue/vue-cli-template http-project
	cd http-project
	npm install
	npm run watch:ios

The above commands will create a new project called **http-project** with the Vue CLI using a NativeScript template. They will install the NativeScript and Vue.js dependencies and then they will start emulating the application via the iOS emulator with hot-reload. If you don't have access to the iOS simulator, you can use Android instead.

When creating a new project you'll be asked a few questions regarding the setup of your application. For this particular example, the defaults are fine.

Before we jump into the code, I want to point out that we're going to be using the open and free API for obtaining Pokemon data found at [Pokéapi](https://pokeapi.co/). You're welcome to use any API or one that you've created yourself. If you want to build an API yourself, check out a tutorial I wrote titled, [Building a RESTful API with Node.js and Hapi Framework](https://www.thepolyglotdeveloper.com/2017/10/building-restful-api-nodejs-hapi-framework/).

## Make HTTP Requests with the NativeScript HTTP Module

The first method we're going to explore, and possibly the recommend way when developing NativeScript applications, uses the NativeScript HTTP module. Open your project's **src/components/HelloWorld.vue** file and include the following to start:

	<template>
	    <Page class="page">
	        <ActionBar class="action-bar" title="Pokemon"></ActionBar>
	        <GridLayout>
	            <ListView for="p in pokemon" class="list-group">
	                <v-template>
	                    <StackLayout class="list-group-item">
	                        <Label :text="p.name" />
	                    </StackLayout>
	                </v-template>
	            </ListView>
	        </GridLayout>
	    </Page>
	</template>
	
	<script>
	    import * as http from "http";
	    export default {
	        data() {
	            return {
	                pokemon: []
	            };
	        },
	        mounted() { }
	    };
	</script>
	
	<style scoped></style>

The above code is more or less setup to what we're trying to accomplish. In the `<script>` block we are initializing an array called `pokemon` which will hold all of our HTTP response data. We're also importing the `http` module which ships with NativeScript projects.

In the `<template>` block we have a `<ListView>` that populates based on the content of our `pokemon` array. If we were to run our application right now, it would display nothing since the array is empty.

Now let's say we want to obtain a list of Pokemon when the application loads. Within the `mounted` method we could have the following:

	http.getJSON("https://pokeapi.co/api/v2/pokemon/?limit=151").then(result => {
	    this.pokemon = result.results;
	}, error => {
	    console.log(error);
	});

The above `getJSON` method would use the Pokéapi to get a list of the original 151 Pokemon. The result would be set to our `pokemon` array and it would be rendered in our list.

While not useful for this example, if we wanted to send data via a POST request we could do something like the following:

	http.request({
	    url: "https://httpbin.org/post",
	    method: "POST",
	    headers: { "Content-Type": "application/json" },
	    content: JSON.stringify({
	        username: "username",
	        password: "password"
	    })
	}).then(response => {
	    var result = response.content.toJSON();
	}, error => {
	    console.error(error);
	});

The above was taken from the [http module documentation](https://docs.nativescript.org/cookbook/http). Remember, Pokéapi is a consumption only API and you shouldn't be sending data to it.

## Make HTTP Requests in a NativeScript Application using axios

Now that we've seen how to work with remote web services using the NativeScript HTTP module, let's take a look at how to use the more widely known [axios](https://github.com/axios/axios) method, which I demonstrated in my [previous tutorial](https://www.thepolyglotdeveloper.com/2017/10/consume-api-data-http-vuejs-web-application/).

First we'll need to obtain the axios library for our project:

	npm install axios --save

With the library downloaded, now we can make use of it within our project. Most of what we had created can remain, we're just going to remove the NativeScript HTTP stuff that we added.

Open the project's **src/components/HelloWorld.vue** file and make the `<script>` block look like the following:

	<script>
	    import axios from "axios";
	    export default {
	        data() {
	            return {
	                pokemon: []
	            };
	        },
	        mounted() {
	            axios({ method: "GET", "url": "https://pokeapi.co/api/v2/pokemon/?limit=151" }).then(result => {
	                this.pokemon = result.data.results;
	            }, error => {
	                console.error(error);
	            });
	        }
	    };
	</script>

It isn't any more difficult to use the axios library in our Vue.js with NativeScript project. It may even be more heavily documented since it is a heavily used library. Just like with the NativeScript HTTP module it offers requests beyond GET requests. Just have a look at the [official documentation](https://github.com/axios/axios).

## Conclusion

You just saw how to make HTTP requests to a popular remote API within a [NativeScript-Vue](https://nativescript-vue.org/) Android and iOS application that uses [Vue.js](https://vuejs.org/). There are many ways to accomplish the task, but I believe the NativeScript HTTP module is the recommended over the alternatives because it was designed for native Android and iOS functionality.
