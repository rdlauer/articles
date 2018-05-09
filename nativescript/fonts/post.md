# Using Custom Fonts in a NativeScript App

Seasoned web and mobile developers know that an easy way to add some pizazz to an app is to switch to an alternative font face. Heck, I remember the renaisance that Verdana brought to the stage back in the late 90's!

IMAGE

The question of *how* to use a custom font is mostly handled in the NativeScript docs. However, there are some extra steps that you may need to be aware of to help smooth your journey to Comic Sans glory.

## Finding a Font

If you're looking for some ideas for a mobile-friendly font, you've come to the right place. If you already have the font you want, head on to the next section.

### System Fonts

Both iOS and Android come with a set of fonts pre-installed in the OS. The problem is the venn diagram of fonts doesn't overlap too well, so if you're picking a system font, it's likely you'll have different choices for iOS vs Android (not to mention the fact that each iteration of Android can have wildly different fonts installed).

But system fonts are a great way to experiment. The best place to look for iOS system fonts is [iosfonts.com](http://iosfonts.com/) and for Android it's not the Android docs unfortunately, but rather an [extensive post on Stack Overflow](https://stackoverflow.com/questions/19691530/valid-values-for-androidfontfamily-and-what-they-map-to).

### Font from Image

Commonly I will see a font embedded in an image that I like. Luckily our friends at Font Squirrel provide a [Font Matcherator](https://www.fontsquirrel.com/matcherator) service that is usually up to the task!

Let's start with the following image:

IMAGE

...

With the font name in hand, I'm a quick web search away from finding the source, downloading it for free, or paying a reasonable sum (usually). 

### Other Font Resources

Using the power of Google you might just be able to find a free (or paid) font that you want. My favorite font sites are [Google Fonts](https://fonts.google.com/) (because those are all free and I am cheap!) and Font Squirrel (because they have everything else, for when I'm actually willing to open the pocket book).

## TTF, OTF, WTF?

When you download a font, you may be a little overwhelmed at the quantity of files you are subjected to. I JUST WANTED ROBOTO!

IMAGE

It takes a little picking-and-choosing to determine the exact font files you need. If you're just starting out with a font, it's best to just include all of the files with either the .ttf or .otf file extension. There is no different for NativeScript, so just pick one, and delete the other files.

IMAGE

Now that we are down to just .ttf (in this case), we can further narrow down the fonts we need by variant. For instance, if I *know for a fact* that I only need a `Regular` and `Bold` variant (and not `Italics`), I only need those font files.

IMAGE

Now that I have limited the font files I need, they can simply be copied to the `app/fonts` directory in my NativeScript project.

> If you are using the NativeScript Playground...

## Which Font-Family?

Now we switch over to our app. The way we specify how to use a font is with CSS of course! And yes, it's the same CSS you've been using on the web, with a wrinkle.

iOS requires the `font-family` to be the *exact name* of the font. The only reliable way I've been able to do this is with the **Font Book** app on macOS.

> Windows developers, I'm hoping you a have a trick to share in the comments!

In our case, the font I've chosen is ???, but that's not the name that iOS expects to see. However, by double-clicking on the font file, I can open it up in Font Book to see the name is really ???:

IMAGE

Now, for Android it's generally a bit simpler. You can count on Android to look at the file name of the font. So in our case that is ???.

Our CSS is starting to come together!

	font-family: ???
	
But what about our variants? Ah, this is where we find another wrinkle! With iOS we simply have to specify the variant in CSS, like so:

	font-weight: bold;
	
...but with Android you have to explicitly reference the bold variant of the font file, like so:

	font-family: ???
	
It's not as as bad as it sounds though, as the final CSS for this project (for both regular and bold variants) would probably be something like this:

	.asdf {
		font-family: asdf, asdf;
	}
	
	.asdf-bold {
		font-family: asdf, asdf;
		font-weight: bold;
	}
	
Then in my markup I can simply reference the above classes:

	<Label ...
	
Done!

## Bonus: Icon Fonts

Another very common use of custom fonts is for icons. The most popular image icon sets out there are probably Font Awesome and Ionicons, but there are plenty more out there.

In the case of Font Awesome, the instructions are the same as the above. The only difference is with the text output. There are a few ways to tackle this:

- Use a plugin that is built to make using icon fonts, or
- Simply look up the unicode value that you want to display.

In the case of Font Awesome, we can look up an icon from the [master list](https://fontawesome.com/icons?from=io) and grab the unicode value:

IMAGE

Copy that value and paste it into the `text` property of your NativeScript UI element, like so:

	<Button text="asdf" ...
	
...which will render a nice little icon as requested.

## Wrapping Up

Hopefully this guide has been helpful for you to get up and running with custom fonts in NativeScript. Sound off in the comments if you run into trouble or if you have any tips of your own.

Happy NativeScripting!