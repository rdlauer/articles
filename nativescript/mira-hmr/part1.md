# A Deep Dive into Hot Module Replacement with Webpack 😱

The following article is part one of a "deep dive" series on Hot Module Replacement with webpack.

- Part 1: The Basics
- Part 2: Handling hot updates with the `module.hot` API (coming soon)

## Part 1: The Basics

Let's imagine for a moment it's still the middle of December and I have finished this blog post on time.

Christmas is quickly approaching and it's about time you decorate that tree in the living room. You fetch all boxes with the stuffed decoration from last year. First, you get the lights out. Make sure that you're not stressed at all while untangling them. You take a step back, maybe put on some Christmas music, make yourself a cup of tea. After a few hours, you are ready to put them on the tree. Cool, the hardest part is over. Now, you can hang some garlands and cool ornaments. Finally, you place the Star™ on the top of the tree and lit the lights. It's beautiful.

But...was that the right Star™? You decide that you actually want the shiny new rainbow Star™ instead of the conservative old yellow Star™. Do you need to remove everything and start the whole process all over just to replace that piece of decoration? Obviously not! You only need to take down the old Star™ and put the new one.

If you're still reading, you're probably wondering what was all that about. Well, your Christmas tree is just like a JavaScript application. The ornaments, garlands, and lights are the modules of the application. The mechanism that allows you to change pieces of decoration without having to disassemble the tree, is called *Hot Module Replacement*. And **HMR** is what we'll be playing with today.

## Why should I care?

Because development with HMR is faster.

I work on the NativeScript framework and sometimes even build apps with it. Let's take a retrospective look at my life as a NativeScript developer *before* Hot Module Replacement: