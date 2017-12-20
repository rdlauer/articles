# NativeScript Observable Magic String Property Name, Be Gone!

## Introduction

A popular approach in JavaScript APIs these days is to pass a string that matches a property name as a parameter to some function. These are sometimes called "magic strings" and they are very code smelly.

And why shouldn't we use them? After all, JavaScript allows us to access object properties in either of these ways:

	obj.property
	obj[‘property’]

Using strings to denote property names is extremely error prone and not refactor-friendly. We have enough to worry about when it comes to keeping the code properties and the bound properties in our markup in sync, let’s at least eliminate this worry when it comes to our code.  

In NativeScript Core applications, a common pattern to support two-way data binding is to create a view model that extends NativeScript’s own [Observable](https://docs.nativescript.org/cookbook/data/observable). By using Observable’s `get()` and `set()` methods, we get UI updates for free. Unfortunately, these methods accept magic strings to let the API know what property name was updated.  

With the help of TypeScript, we can solve this problem using one or both of the following three methods. Pick your favorite or the most appropriate method for your situation.

## Setup

We can demonstrate this with a simple "Hello World" application. We can quickly generate a NativeScript app with the common "Hello World" template and TypeScript using this simple CLI command:

	tns create magic-strings-be-gone --tsc

Open up the project folder in a code editor and open the `main-view-model.ts` file, which defines the `HelloWorldModel` class. The `message` property that we see there with a getter and a setter is bound to a label in the markup. We won’t need a getter and a setter for this exercise, a simple public property will do. Remove the getter and setter for the `message` property and just add a public `message` property of the `string` type. You can also remove the private `_message` field if you want, but it’s not bothering anyone, right? Oh, just remove it already, you know you want to.

At this point our class should look like this:

	export class HelloWorldModel extends Observable {
	    private _counter: number;
	    constructor() {
	        super();
	        this._counter = 42;
	        this.updateMessage();
	    }
	
	    public message: string;
	    
	    public onTap() {
	        this._counter--;
	        this.updateMessage();
	    }
	
	    private updateMessage() {
	        if (this._counter <= 0) {
	            this.message = 'Hoorraaay! You unlocked the NativeScript clicker achievement!';
	        } else {
	            this.message = `${this._counter} taps left`;
	        }
	    }
	}

The problem here is that when we update the `message` property, we don’t get notifications and our UI won’t update. To get the free UI updates, we have to use Observable’s `set()` function to set the message property, which internally triggers notification. So change our `updateMessage()` method to this:

    private updateMessage() {
        if (this._counter <= 0) {
            this.set('message', 'Hoorraaay! You unlocked the NativeScript clicker achievement!');
        } else {
            this.set('message', `${this._counter} taps left`);
        }
    }

There are those magic strings! Observable’s API requires a string to be sent to the `set()` method and this is where our problems began. If we refactor the code and change the property name or the magic string (or worse, one of the magic strings) then we will have runtime problems that will be extremely hard to diagnose.

Let’s fix this, shall we?

## Method 1

TypeScript has a handy operator that allows us to create a “list” of strings that are the public property names of a class. Well, that’s not entirely accurate. We can create a `type` that is all the public properties of a class as strings.

**We can use the amazing `keyof` operator to do this.**

Just above our `HelloWorldModel` class definition, define a new type and use the `keyof` operator:

	type MessageType = keyof HelloWorldModel;

If our editor provides TypeScript intellisense as Visual Studio Code does, when we hover over the `MessageType` identifier, we’ll see that the new type we created is all the public properties of the `HelloWorldModel` class as strings.

This is perfect because now we can create a constant that will be just one of the properties, specifically the message property:

	const messageType: MessageType = 'message';

This constant looks like a string, but it’s not a string. It’s of type `MessageType`. If we try misspelling `message`, we’ll immediately see compilation errors. Since we now have a strongly typed constant, we can just use it in the `set()` method call of our `updateMessage()` method:

    private updateMessage() {
        if (this._counter <= 0) {
            this.set(messageType, 'Hoorraaay! You unlocked the NativeScript clicker achievement!');
        } else {
            this.set(messageType, `${this._counter} taps left`);
        }
    }

Now if we change the property name, we will get compilation errors, which is a whole lot better than getting a runtime error. 

## Method 2

If we want a more generic approach and don’t want to add a constant for every property we want to update, then we can use this next method, which is similar in that we still use the `keyof` operator.

Create a new file called `observable-extensions.ts` and add the following code to the file:

	import { Observable } from 'data/observable';
	
	export function getObservableProperty<T extends Observable, K extends keyof T>(obj: T, key: K) {
	    return obj.get(key);
	}
	
	export function setObservableProperty<T extends Observable, K extends keyof T>(obj: T, key: K, value: T[K]) {
	    obj.set(key, value);
	}

We’re exporting two functions here that act on `Observable`s. Take a look at the generic `setObservableProperty()` function. It specifies that the first parameter is of type `T`, which has to inherit from `Observable`, and our `HelloWorldModel` is such a class. The second parameter is of type `K` which extends the type that is `keyof T`, which is, the list of all public properties of the class that extends `Observable`. There’s a lot to unpack there, but trust me, it works.

We can now import the `setObservableProperty()` function in the `main-vew-model.ts` file:

	import { setObservableProperty } from './observable-extensions';

And modify the `updateMessage()` method once again to use the new function:

    private updateMessage() {
        if (this._counter <= 0) {
            setObservableProperty(this, 'message', 'Hoorraaay! You unlocked the NativeScript clicker achievement!');
        } else {
            setObservableProperty(this, 'message', `${this._counter} taps left`);
        }
    }

This is deceivingly simple looking. It even looks like we have our magic string back, but in reality, the `message` parameter is NOT a string at all. It is a `type`. If we try misspelling, we’ll immediately get TypeScript compilation errors, which is something we didn’t get at the start of this article.

## Method 3

The extension function approach we saw in method 2 is great, and it doesn’t allow us to mess up the property names. However, the syntax for calling the observable property extension function is not as intuitive as we would like. Wouldn’t it be great if we could just set our `message` property the way we always have done and just forget about it?

**Property Decorators to the rescue!**

Thanks to Peter Staev for providing a decorator method that can help us do just that. Here’s how we can keep our view model code even cleaner.

Create another file, let’s call it `observable-decorator.ts`, and add this code:

	import { Observable } from "data/observable";
	
	export function ObservableProperty() {
	    return (obj: Observable, key: string) => {
	        let storedValue = obj[key];
	
	        Object.defineProperty(obj, key, {
	            get: function () {
	                return storedValue;
	            },
	            set: function (value) {
	                if (storedValue === value) {
	                    return;
	                }
	                storedValue = value;
	                this.notify({
	                    eventName: Observable.propertyChangeEvent,
	                    propertyName: key,
	                    object: this,
	                    value,
	                });
	            },
	            enumerable: true,
	            configurable: true
	        });
	    };
	}

This file provides a decorator called `ObservableProperty` that can be applied to a property in your view model that you want to keep synchronized with your UI. Read more about JavaScript decorators [here](https://tc39.github.io/proposal-decorators/) and [here](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841).

As far as our example is concerned, every time you update the `message` property in your view model, you want to run a piece of code that notifies the UI. If you take a look at the `set:` function definition in the code above, you’ll notice that we store the updated value in a local field, then call the `notify` method to do just that.

In order to apply this decorator to the `message` property in our view model class, just add the decorator to the property like this:

    @ObservableProperty()
    public message: string;

Now your `updateMessage()` method can be simplified back to this:

	private updateMessage() {
        if (this._counter <= 0) {
            this.message = 'Hoorraaay! You unlocked the NativeScript clicker achievement!';
        } else {
            this.message = `${this._counter} taps left`;
        }
    }

and everything still works.

Here is the entire view model file with method 3 implemented for your reference:

	import { Observable } from 'data/observable';
	import { ObservableProperty } from './observable-decorator';
	
	export class HelloWorldModel extends Observable {
	    private _counter: number;
	
	    constructor() {
	        super();
	        this._counter = 42;
	        this.updateMessage();
	    }
	
	    @ObservableProperty()
	    public message: string;
	
	    public onTap() {
	        this._counter--;
	        this.updateMessage();
	    }
	
	    private updateMessage() {
	        if (this._counter <= 0) {
	            this.message = 'Hoorraaay! You unlocked the NativeScript clicker achievement!';
	        } else {
	            this.message = `${this._counter} taps left`;
	        }
	    }
	}

This method does give you the ability to keep your model code clean, but you lose some of the strong typing that you get with the keyof operator. So it’s up to you which method you pick, but all three methods will definitely help keep our code safer.

TypeScript provides us with some powerful tools to help us not mess up, and the `keyof` operator is a really awesome one. While these techniques are demonstrated in the context of a NativeScript Core application, we can use them in any application where an API requires a magic string as a property name. 

You might also like a video version of this article, [which is available here](https://youtu.be/lNcNYFNEaEI). If you enjoy video learning and you’re interested in more NativeScript development techniques that are beginner to advanced, check out [NativeScripting.com](https://nativescripting.com/) for video courses.

Happy coding.