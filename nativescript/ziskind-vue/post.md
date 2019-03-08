# NativeScript-Vue with Class Components

You [might have heard](https://www.nativescript.org/blog/nativescript-5.2-comes-with-official-support-for-vue) that as of version 5.2, the NativeScript CLI can scaffold out Vue-based native mobile apps. This news came as a fantastic bonus to the already big release for a minor version, and I think it's a move in the right direction. 

I've also been a long time fan of TypeScript, a language that is widely used in the enterprise, and a language that is being embraced by the VueJS community, especially in 2019. So I was very happy to hear that NativeScript-Vue recently got TypeScript support as well. 

While we wait for the NativeScript CLI to offer the TypeScript option for it's Vue projects, there is still an alternate way to create NativeScript-Vue projects that will give you TypeScript support, and let you use class components.

Let's see how we can leverage class components in NativeScript-Vue.

## Creating a NativeScript-Vue With TypeScript Project

We need to create a NativeScript project that will suport Vue and TypeScript at the same time. I show you how to do this in a [recent video](https://youtu.be/Z9koujDvpBM) I posted on YouTube. Here are the steps.

> I'll assume that you already have the latest [NativeScript bits and pieces](https://docs.nativescript.org/angular/start/quick-setup) installed on your machine.

First, install the Vue CLI and the cli-init tool globally via npm

	npm install -g @vue/cli @vue/cli-init

Use the Vue CLI to generate a new NativeScript project using the cli-init tool and the nativescript-vue vue-cli-template.

	vue init nativescript-vue/vue-cli-template <project-name>

When you run this Vue CLI command, you get an interactive wizard that asks you some questions.

	- Project name
	- Project description
	- Application...

etc...

The language option you need to select is

	- Select the programming language
	  - javascript
	  - typescript <- this one

Follow the rest of the wizard to get the project set up, any options you select are fine after this point.

This will create a new NativeScript project, with the proper packages in the package.json file, a properly configured `tsconfig.json`, and here's the big one: a properly comfigured `webpack.config.js` file. There is also a `types` folder that has extra TypeScript declarations for the environment, Vue shims, and nativescript platform references.

The `app` folder has a `main.ts` file. Yes that's a `.ts` extension you see there.

And your single file Vue components have a code block that specifies TypeScript as the language. Notice the `ts` value for the `lang` attribute on the `script` tag.

	<script lang="ts">
	  export default {
	    data() {
	      return {
	        msg: 'Hello World!'
	      }
	    }
	  }
	</script>


By the way, you can omit this lang attribute and just write plain JavaScript here if you want. This is useful for projects that are transitioning from JavaScript to TypeScript, for example.

When you run the project, don't forget that you need to use the `--bundle` flag. Otherwise you'll be scratching your head, creating a bald spot, and wondering what is going on.

	tns run ios --bundle

OR

	tns run android --bundle

## Adding Class Component Support

If you are coming from an Angular background, then you'll feel right at home with class-based components with decorators. This is the next logical step to take after switching to using TypeScript and a natural one for Angular devs.

Why use class components? Simple - the syntax is cleaner. Class properties are automatically data properties. No need for strange functional syntax returned by the data property, **and** you don't have to worry about `this`.

### Install Packages

Out of the box, the new nativescript-vue cli template creates object notation TypeScript components. If you want to get class components **and** TypeScript, there are a few manual steps you need to perform. Hopefully these will be rolled into the template too at some point.

In your project's `package.json` file, add the following dependencies to support Vue class components and and decorators:

	"dependencies": {
		...
		"vue-class-component": "^6.3.2",
		"vue-property-decorator": "^7.3.0",
		...
	},

Run `npm install`

### Create Class Based Single File Component

You can start using Class Components right away. Let's try this by creating a simple throw-away component, just so we can see how these work.
Create a single file component file, let's call it `MyComp.vue` and add this code:

	<template>
	  <Label class="message" :text="msg" col="0" row="0"/>
	</template>
	
	<script lang="ts">
	import { Component, Prop, Vue } from "vue-property-decorator";
	@Component
	export default class MyComp extends Vue {
	  @Prop() private msg: string;
	}
	</script>

Notice instead of the default export being a JavaScript object, here we're exporting a class. And instead of importing `Vue` from `vue`, we're importing it from `vue-property-decorator`.

Don't forget the `@Component` decorator on the class.

## Class Component Scaffolding

Here we're going to switch gears a bit and create a new example so we can examine all the aspects of using class components in NativeScript-Vue.

Class component `.vue` files aren't that different from their object notation `.vue` files.

Here's an example we'll be working with.

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	
	}
	</script>

Two important things to note here are that we're exporting a class that inherits from `Vue` and that we've docorated the class with the `@Component` decorator, similar to how we'd do this in Angular.

## Examining Class Components in NativeScript-Vue

If you are serious about using class components in your NativeScript VueJS apps with TypeScript, you need to know these five things. I've created a [YouTube video](https://youtu.be/u8lMkgb-8iA) showing each of these in detail.

1. Props - data passed in as inputs to your Vue components
2. Data - this is local properties (or the state) of your Vue components
3. Watch - use this to spy on other properties and react to them being changed
4. Computed - don’t create and maintain another property! Use a computed to create a calculated property.
5. Methods - they do stuff! These are your event handlers and other class functions

### Thing 1: Props

By using the `@Prop` decorator from `vue-property-decorator`, we declare input properties just by decorating class properties.

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component, Prop } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	  @Prop() whatToSay: string;
	}
	</script>

If you're coming from the object notation Vue, then you're used to the code looking like so:

	export default {
	  props: {
	    whatToSay: {
	      type: String
	    }
	  }
	};

### Thing 2: Data

`data` is really simple with class components. It's just properties on the class:

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component, Prop } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	  @Prop() whatToSay: string;
	
	  //Data
	  counter = 1;
	  firstName = "Donna";
	  initialLastName = "Summer";
	  lastName = this.initialLastName;
	}
	</script>

This is what `data` looks like with object notation - not as easy to reason about, in my opinion:

	data() {
	    return {
	      counter: 1,
	      firstName: "Donna",
	      initialLastName: "Summer",
	      lastName: "Summer"
	    };
	  },


### Thing 3: Watch

Watchers are probably the most complicated part because they are defined as class methods with a `@Watch` decorator. The `@Watch` decorator has to specify what property we're spying on.

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component, Prop, Watch } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	  @Prop() whatToSay: string;
	  counter = 1;
	  firstName = "Donna";
	  initialLastName = "Summer";
	  lastName = this.initialLastName;
	
	  //Here we define a Watch
	  @Watch("firstName")
	  onFirstNameChanged() {
	    this.lastName = this.initialLastName + this.counter++;
	  }
	}
	</script>

BUT, it's still way neater than it is with object notation, which looks like this:

	watch: {
	    firstName: {
	      handler() {
	        this.lastName = this.initialLastName + this.counter++;
	        console.log("first name changed");
	      }
	    }
	  }

The triple nested object is definitely not easy to look at or reason about. Again, this is my opinion.

### Thing 4: Computed

Computeds are my favorite because they are executed exactly how they should be in a class - as getter (and setter) properties.

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	
	    <StackLayout class="input-field">
	      <Label text="Full Name" class="label font-weight-bold m-b-5"/>
	      <Label :text="fullName"/>
	    </StackLayout>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component, Prop, Watch } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	  @Prop() whatToSay: string;
	  counter = 1;
	  firstName = "Donna";
	  initialLastName = "Summer";
	  lastName = this.initialLastName;
	
	  //Computed
	  get fullName() {
	    return `${this.firstName} ${this.lastName}`;
	  }
	
	  @Watch("firstName")
	  onFirstNameChanged() {
	    this.lastName = this.initialLastName + this.counter++;
	  }
	}
	</script>

### Thing 5: Methods

Since we're using classes, guess what! Methods are just class methods! Declare an event handler in the template, and then just write a method in your class.

	<template>
	  <StackLayout class="form">
	    <StackLayout class="input-field">
	      <Label text="First Name" class="label font-weight-bold m-b-5"/>
	      <TextField v-model="firstName" hint="First Name" class="input input-border"/>
	    </StackLayout>
	
	    <StackLayout class="input-field">
	      <Label text="Full Name" class="label font-weight-bold m-b-5"/>
	      <Label :text="fullName"/>
	    </StackLayout>
	
	    <Button text="SPEAK" @tap="speak"/>
	  </StackLayout>
	</template>
	
	<script lang="ts">
	import { Vue, Component, Prop, Watch } from "vue-property-decorator";
	
	@Component
	export default class PersonClassComponent extends Vue {
	  @Prop() whatToSay: string;
	
	  counter = 1;
	  firstName = "Donna";
	  initialLastName = "Summer";
	  lastName = this.initialLastName;
	
	  get fullName() {
	    return `${this.firstName} ${this.lastName}`;
	  }
	
	  @Watch("firstName")
	  onFirstNameChanged() {
	    this.lastName = this.initialLastName + this.counter++;
	  }
	
	  //And here is the method
	  speak() {
	    alert("This is " + this.fullName + " speaking. " + this.whatToSay);
	  }
	}
	</script>

## Conclusion

I know that class components aren’t for everyone and some people really love using pure JavaScript, that’s totally fine too. This is just another way to do it, and if you **are** going to be using TypeScript in your NativeScript-vue apps, then class components in Vue is a natural fit. Especially for those people that are coming from an object oriented programmng background - .NET and Angular folks, that includes you.

Do you want more NativeScript-Vue videos? I have an [Introduction to NativeScript-Vue course](https://nativescripting.com/course/nativescript-vue-introduction) out, and Paul Halliday, another NativeScript Developer Expert, has a [brand new course all about state management with NativeScript-Vue and Vuex](https://nativescripting.com/course/nativescript-vue-and-vuex).
