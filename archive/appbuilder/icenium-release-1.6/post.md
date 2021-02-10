##Icenium 1.6 is Here!

We are very excited about the new features in this release! Especially the fact that Icenium now supports the use of custom Cordova plugins! The key highlights I want to briefly cover from version 1.6 are:

* Cordova 2.7.0 Support
* Custom Cordova Plugins
* UI Designer (*Experimental Feature*)

###Cordova 2.7 Support
![](./cordova_bot.png)

Cordova 2.7.0 was released last month (May) - and contains a number of bug fixes as well as some new features for InAppBrowser, etc. For specifics on what the 2.7.0 release entailed, here are few posts worth checking out:

* [What’s new in Cordova iOS 2.7.0](http://shazronatadobe.wordpress.com/2013/05/03/whats-new-in-cordova-ios-2-7-0/)
* [What’s new for Cordova 2.7.0 Android](http://webcache.googleusercontent.com/search?q=cache:lIrWprVfXV8J:www.infil00p.org/whats-new-for-cordova-2-7-0/+&cd=1&hl=en&ct=clnk&gl=us)

### Custom Cordova Plugins

![](./applause.gif)

Say it with me again: **Custom. Cordova. Plugins**. The beauty of developing hybrid mobile applications using Cordova is the fact that if Cordova doesn't already expose a native API to which you need access, you can write your own plugin to bridge that 'gap'. With custom plugins, you can extend native API access as much as you need and unleash the full native potential of the device. While Icenium has supported a handful of stock plugins for a while, is hasn't been possible to upload custom plugins until now. This has been our [most requested feature](http://feedback.telerik.com/Project/87/Feedback/Details/879-generic-phonegap-plugin-support) – now the wait is over! Yes, that sounds you hear *is* actually angels singing. We won't let those angels keeps us from taking a brief look at using custom plugins, though.

####Using the Canvas2Image Plugin
[Tommy Williams](https://twitter.com/therealdevgeeks) created the [Canvas2Image Cordova plugin](https://github.com/devgeeks/Canvas2ImagePlugin) for iOS devices as an exercise in saving the contents of an HTML canvas to the camera roll of the device. So - what would it take for us to utilize this plugin in Icenium and deploy it to a device for testing?

#####First - We Download the Plugin

First thing's first, let's grab a zip of the plugin from github (by clicking on the zip download option):

![](./c2img1.jpg)

#####Second - We Upload the Plugin To Our Icenium Project
I'm using Mist (our browser-based IDE). You'll notice a new folder in our project, named "Plugins". All we need to do is upload our zip to the plugins folder (right-click, choose Add -> From Archive):

![](./UploadCustomPlugin.jpg)

Once our zip is uploaded, we'll see a folder for our plugin - something along these lines:

![](CustomPluginFolder.jpg)

Now here's an *important caveat*: for a custom Cordova plugin to work with Icenium, it needs to follow the [plugman spec](https://github.com/apache/cordova-plugman/blob/master/plugin_spec.md). You'll notice below that Canvas2Image plugin included a `plugin.xml` file at the root of the folder. Let's open that up and check it out:

![](./plugin_xml.jpeg)

This file provides the metadata we need to include the plugin in the build. The `asset` element (line 8) and the `platform` element (line 11) are of particular importance. The `asset` element provides the path to our JavaScript library which exposes the native APIs this plugin makes available. The `platform` element provides the platform-specific assets that should be installed. You can see, in this case, the plugin supports iOS, and includes `header-file` and `source-file` elements that point to native source code that should be compiled and included in the build.

<div>
	<table style="width:60%; margin-left:auto;margin-right:auto;">
 		<tr>
 			<td width="150"><img src="./olderschema.jpg"/></td>
 			<td><em>AN IMPORTANT NOTE ON COMPATIBILITY:</em><br/>
The plugman spec is evolving - and it's important to recognize that the plugin.xml schema often differs between versions of Cordova. The `plugin.xml` above was modified to work with Cordova 2.7.0 - whereas the one that's in the actual github repository is using an older schema for Cordova 2.2.0.</td>
 		</tr>
	</table>
</div>

#####Third - Create Your App!
I put together a [basic sample app](https://github.com/ifandelse/Canvas2ImageIceniumDemo) – inspired by the demo used by Jonathan Creamer [here](
http://tech.pro/tutorial/1383/javascript-one-language-to-rule-them-all) – that utilizes the Canvas2Image plugin - allowing you to import a photo from your camera roll, add a caption to it (all of which is done on an HTML5 canvas) and then save it back to your camera roll as a new image. Once we've uploaded the zip archive (as we saw above), we need to add the script include to our `index.html` file. Remember how our `plugin.xml` file had an `asset` element which pointed to the `Canvas2Image.js` JavaScript file? Thanks to that, all we have to do is include a script tag like this in our `index.html`:

![](./scriptinclude1.jpg)

At this point, I can simply write my app as needed, calling the `Canvas2Image.js` code wherever I need. You can see how the `saveCanvasToImage` method uses the Canvas2Image plugin [here](https://github.com/ifandelse/Canvas2ImageIceniumDemo/blob/master/CustomPlugins/scripts/app.js#L119-L136).

#####Fourth - Run the App

In Mist, I chose "Run -> Build -> iOS -> Build (with Provision)" to generate the .ipa installer, which I then downloaded and installed to my device via iTunes.

Let's check out some screenshots:

<div>
<table style="margin-left:auto;margin-right:auto;">
	<tr>
		<td style="text-align: center;"><span style="font-weight: bold">Upon opening the app:</span></td>
		<td style="text-align: center;"><span style="font-weight: bold">Selected Image, Added Caption:</span></td>
	</tr>
	<tr>
		<td><img src="./c2i_app1.png" width="250" /></td>
		<td><img src="./c2i_app2.png" width="250" /></td>
	</tr>
</table>
</div>
<span style="clear:both;"></span>

<div style="margin-left:auto;margin-right:auto;text-align:center;">
	And proof that Tommy's plugin works as advertised, here's the actual image exported by the app:<br/>
	<img src="./c2i_app3.png" />
</div>

#####Two Important Things to Be Aware Of:

* Custom plugins will not work in the simulator. You could provide your own mock of the plugin's API, if you really needed to – but running the application on an actual device is the best way to ensure that it functions properly.
* You won't be able to use Icenium Ion to download the app for testing on your iOS device (thanks to Apple's restrictions preventing us from compiling the native part(s) of your plugins inside Ion).

So there you have it - your first tour through using custom Cordova plugins in Icenium! Before we finish, though, I want to mention a new experimental feature we've added.


###UI Designer
Our engineering team has included a UI Designer, which currently only supports Kendo UI Mobile (though we may expand it to support other UI frameworks in the future). As I mentioned above, it's an *experimental* feature at the moment, but we'd love your feedback as we work over the coming weeks to mature it. You can access the designer when you open an HTML file in project that's using Kendo UI Mobile. For example, if I open the `index.html` file from our sample application above, you'll see "Code", "Designer" and "Split" tabs at the bottom of the IDE (I'm using Mist in the example below):

![](./kendodesigner1.jpg)

The "Code" view is what you've been seeing up until now (and it's still the default view). As you can probably guess, clicking on "Designer" will open WYSIWYG style editor,  and clicking "Split" will show "Designer" and "Code" side-by-side:

<div style="margin-left:auto;margin-right:auto;text-align:center;">
	The Designer View:<br/>
	<img src="./kendodesigner2.jpg" />
</div>

You'll notice that we've included some of the basic Kendo UI Mobile widgets in the "UI Library" tool pane on the left - these are drag-and-droppable.

When you choose the "Split" view, you have the option to split vertically or horizontally, as well as swap which pane holds the design vs code view. These options expand next to the "Split" on the tab menu at the bottom:

<div style="margin-left:auto;margin-right:auto;text-align:center;">
	Splitting Vertically:<br/>
	<img src="./kendodesigner3.jpg" />
</div>

<div style="margin-left:auto;margin-right:auto;text-align:center;">
	Splitting Horizontally:<br/>
	<img src="./kendodesigner4.jpg" />
</div>

When you're working with the designer, you'll notice that "Outline Inspector" and "Property Inspector" panes will appear docked to the right of the design area. The "Outline Inspector" allows you to quickly traverse the DOM hierarchy of the view you are editing, and "Property Inspector" provides visibility into Kendo-UI-centric attribute and event values on the selected element. In this screenshot, I have the top level view of our sample app selected:

<div style="margin-left:auto;margin-right:auto;text-align:center;">
	<img src="./kendodesigner5.jpg" />
</div>

##Conclusion
Icenium 1.6 is live, so grab your custom plugins, and start custom-plugging-away on your favorite new mobile app idea. We value your feedback, so please let us know what you think!

<div style="padding: 20px 0px; margin-top: 20px; border-top-color: #999999; border-top-width: 1px; border-top-style: dashed;" class="author"> <img alt="Jim Cowart" style="float: left;" src="http://icenium.com/iceniumImages/default-source/blog-images/jamescowart_180.jpeg?sfvrsn=0" title="Jim Cowart" width="141" height="115" sfref="[images]3bccfef6-a963-6e48-9015-ff00002e69be" />
<p style="margin-left: 150px; margin-top: 0px;"><strong>About the Author<br />
</strong><a target="_blank" href="http://freshbrewedcode.com/jimcowart" rel="author">Jim Cowart</a> is an architect, developer, open source author, and overall web/hybrid mobile development geek. He is an active speaker and writer, with a passion for elevating developer knowledge of patterns and helpful frameworks. Jim works for Telerik as a Developer Advocate and is <a target="_blank" href="http://twitter.com/ifandelse">@ifandelse on Twitter</a>.</p>
</div>
