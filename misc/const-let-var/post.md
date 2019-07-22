# JavaScript 101: var or let or const?

I've been spending some free time lately [updating some "old" NativeScript projects to 6.0](https://www.nativescript.org/blog/nativescript-6.0-application-migration). The process has been amazingly smooth, but I realized I haven't been very consistent in my usage of `var`, `let`, and `const` when assigning variables.

It led me to think: *I'm not even 100% sure when I should be using which method of variable assignment!* I mean, `var` and `let` *seem* to be interchangeable, right? And it's still JavaScript so it seems like I can change the value and data type of anything I want at any time. So maybe there isn't much of a difference? Or am I just overthinking all of this to a fault? Classic Rob! 🤦

It turns out there *are* some fairly significant differences between `var`, `let`, and `const`. So if you're a forever-noob JavaScript developer like me (or hey, maybe you're just getting started!), here is what I've learned:

## The Venerable `var`

In JavaScript, variables are initialized with the value of `undefined` when they are created. So if you ever wrote something like this:

	var foo;
	console.log(foo);

...it would return `undefined` in your console. Makes sense, right?

But if I assigned a value to the variable first, all will be right in the world:

	var foo = "yo";
	console.log(foo);

...you'd see the string `"yo"` in the console, since now `foo` is a string with the value of "yo".

And since it's JavaScript, variables initialized with `var` can not only have their values changed, but also their data types. So, yes, this is valid syntax:

	var foo = "yo";
	foo = 123;
	console.log(foo);

...with the resulting output being `123` in your console.

Pretty straightforward, right? Now let's look at one of the big differences of `var`, that being its *scope*.

## Function Scope vs Block Scope

The *scope* of a variable lets the compiler know where variables (and functions) are accessible inside of your app. In the case of `var`, the variable is "scoped" to the function in which it was created and is (literally) only accessible inside that function.

	function Hello() {
		var foo = "poo";
	}

	Hello();
	
	console.log(foo); // THIS NO WORK 😭

But if I moved the `console.log(foo);` line up into the `Hello()` function, all would be cool.

So let's continue on this concept of scoping as we get into `let`:

## The NKOTB: `let`

> Bonus points to anyone who is brave enough to admit they know what NKOTB stands for. Hint: 🧑‍🤝‍🧑🎤

For all practical purposes, `let` is the same as `var`. Except! (You knew I was going to say that.) Instead of being *function scoped* (see above), `let` is *block scoped*. This means that variables created with `let` are available inside the "block" in which it was created. What's a "block"? It's effectively anything inside curly braces (like a function assignment, a `for` loop, an `if` statement, etc).

If you're coming to JavaScript from another language, `let` is likely to make more sense to you than `var`. Let me give you an example:

	function Hello() {
		for (var i = 0; i <= 10; i++) {
			var foo = i;
		}
		
		console.log(foo); // 10
	}

	Hello();

...versus if we used `let`:

	function Hello() {
		for (let i = 0; i <= 10; i++) {
			let foo = i;
		}
		
		console.log(foo); // 😭
	}

	Hello();

Since in the first example, `i` is scoped to the **entire function**, it's totally cool to reference it outside of the `for` loop.

In the second example, though, `i` is *block scoped* to the `for` loop, meaning it's not available outside of the loop.

## This `const` Thing

At first glance, you're probably thinking that `const` is a way to assign immutable (unchangeable) variables. They are "constants" that now and forever will never ever ever change. **End of story!** 🛑

It so happens that `const` is almost identical to `let`. The main difference is that once you've assigned a value using `const`, you can't re-assign it to a new value. That's cool, because it makes a `const` a literal *constant*.

Ok, that's not entirely true.

> I'm going to stop you right here. If you want an in-depth look at `const` and how it's not quite as immutable as you think it might be, you should read Brian Rinaldi's article, [Confused by JavaScript's const? Me too!](https://dev.to/remotesynth/confused-by-javascript-s-const-me-too-4dfo).

By changing a *property* of an object, you aren't actually reassigning the value (even though it was declared with `const`). This means that this syntax is perfectly valid:

	const me = {
		name: "Rob";
	}
	
	me.name = "Rob Lauer";

`#JustJavaScriptThings

## So Which Should I Use???

I hear you, I haven't actually given you any guidance up to this point. It almost seems like `var`, `let`, and `const` are interchangeable in many scenarios. But here is my opinion:

1. I try to never use `var`. There is no real reason to use it other than your own muscle memory of typing it.
2. Always use `let` (yes, I even use it for constants).
3. Use `const` if you want to, knowing that it doesn't provide many explicit advantages over `let`.

In fact, I would recommend if you're truly creating constants, you name the variable appropriately. Something like:

	const __IAMACONSTANT = "indeed i am a constant";

**In summary:**

`var` is *function scoped*, meaning you can only access the value inside the function in which it was created.

`let` is *block scoped*, meaning you can only access the value inside the block (`{}`) in which it was created.

`const` is also *block scoped*, but unlike `let` and `var`, it can't be reassigned (well, with some exceptions noted above!).