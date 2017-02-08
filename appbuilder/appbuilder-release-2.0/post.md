# AppBuilder March Release: A Full CLI, Windows Phone 8 Support, Sublime Text Integration, and More

Fresh off the [release of the Telerik Platform](http://blogs.telerik.com/blogs/14-02-06/telerik-platform-101), we have an epic release of [Telerik AppBuilder](http://www.telerik.com/appbuilder) to discuss - packed full of several highly-requested features (including Windows Phone 8 support!). There's a lot to talk about, so let's dive right in.

#### Overview of This Release

* [A Fully-Featured CLI](#cli)
* [Sublime Text Integration](#sublime)
* [Windows Phone 8 Support](#windows-phone)
* [Advanced Version Control Support](#version-control)

<h3 id="cli">A Fully-Featured CLI</h3>

We're happy to announce the beta release of a command-line interface for AppBuilder. The CLI is open-source and [available on GitHub](https://github.com/Icenium/icenium-cli/) for manual installation and hacking.

In fact, the CLI is already available on npm, so you can install it right now.

```
> npm install -g appbuilder
```

> Don't have Node.js installed? You can get an installer for Windows and OS X at <http://nodejs.org/>.

Run `appbuilder help` to get a list of the commands available.

<pre style="height: 200px; overflow: auto;">
> appbuilder help
Usage:
    $ appbuilder <command> [command parameters] [--command <options>]

General commands:
    help <command>                  Shows additional information about the commands in this list.
    login                           Logs you into the Telerik Platform.
    logout                          Logs you out from the Telerik Platform.
    user                            Prints information about the currently logged in user.

Project development commands:
    create                          Creates a new project from template.
    init                            Creates a project in the current folder.
    build                           Builds the project and produces a QR code for deploying the application package.
    deploy                          Deploys the project to a connected device.
    cloud-sync                      Synchronizes the project with the cloud to enable LiveSync for remote devices.
    live-sync                       Synchronizes the latest changes in your project to connected devices.
    simulate                        Runs the current project in the device simulator.
    find-plugins                    Searches for plugins in the Apache Cordova Plugin Registry by keyword.
    fetch-plugin                    Imports an Apache Cordova plugin into your project.
    edit-configuration              Opens configuration file for editing.
    prop-add                        Adds more values to a list project property.
    prop-set                        Sets a project property.
    prop-remove                     Removes values from a list project property.
    prop-print                      Prints the value of a project property.
    list-projects                   Lists all projects of your account.
    export-project                  Exports one of your remote projects into a local AppBuilder project.

Device commands:
    list-devices                    Lists all recognized connected devices.
    open-device-log-stream          Opens the log stream for given device.

Certificate management and publishing commands:
    list-certificates               Lists all configured certificates for code signing
                                    iOS applications and Android applications.
    create-self-signed-certificate  Creates self signed certificate for code signing Android applications.
    remove-certificate              Removes certificate from the server.
    export-certificate              Exports certificate from the server.
    import-certificate              Imports certificate to the server.
    list-provisions                 Lists all configured provisioning profiles for code signing iOS applications.
    import-provision                Imports a provisioning profile from file.
    remove-provision                Removes a registered provisioning profile.
    appstore-list                   Lists all applications in iTunes Connect that are ready for upload.
    appstore-upload                 Builds the project and uploads the app to iTunes Connect.

Global Options:
    --help                Prints help about the command given in the rest of the command line.
    --path <path>         Specifies the file path to the project. If not specified, the project is searched for
                          in the current directory and all directories above it.
    --version             Prints the client's version and exits.

</pre>

There's a lot here, and we'll have subsequent articles to go into more detail, but for now let's look at what it takes to get a project up and running on a device.

We'll start by running running `appbuilder create`, which scaffolds a new project for us.

```
> appbuilder create
```

This new project uses [Kendo UI Mobile](http://www.telerik.com/kendo-ui-mobile), with the directory structure shown below.

```
.
└── app
    ├── App_Resources
    │   └── ...
    ├── cordova.android.js
    ├── cordova.ios.js
    ├── index.html
    ├── kendo
    │   ├── js
    │   │   ├── ...
    │   │   └── kendo.mobile.min.js
    │   └── styles
    │       ├── ...
    │       └── kendo.mobile.all.min.css
    ├── scripts
    │   ├── ...
    │   └── app.js
    └── styles
        ├── ...
        └── main.css
```

> Don't want to use Kendo UI Mobile? `appbuilder create` can build jQuery Mobile, Kendo UI Mobile, Kendo UI DataViz, or blank projects. To switch from Kendo UI Mobile, set the `--template` option. Run `appbuilder create --help` for details.

We now have a project, but we want to want to see what this looks like on a device, so let's build it with `appbuilder build`.

```
> appbuilder build android
```

Building requires you to be logged into a Telerik Platform account, so the `build` command first prompts you to login via a new browser before performing the build. (If you don't have a Telerik Platform account you can [create](http://platform.telerik.com/) one for free.) You can also login at any time using `appbuilder login`. After the login, the `build` command performs an Android build, and opens a browser with a QR code, as shown below.

![A browser open with a QR code displaying](2.0-qr-code.png?sfvrsn=2)

You can scan this code to download the built .apk file and install it on your device - which is cool, but we can make the process even easier by [connecting an Android device](http://docs.telerik.com/platform/appbuilder/testing-your-app/running-on-devices/running-on-connected-devices/google-android-devices/how-to-configure-usb-device-drivers-for-your-android-device) and using the `deploy` command.

```
> appbuilder deploy android
```

This command starts by building the project, but when the build completes, instead of showing a QR code, it actually opens the project on the connected Android device.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-deploy.gif?sfvrsn=2" alt="An Android phone screenshot showing the project open">

It gets even cooler with [LiveSync](http://docs.telerik.com/platform/appbuilder/testing-your-app/livesync/using-livesync), or our way of pushing code changes directly into built native applications.

For example let's say we decide that our template's heading isn't nearly exciting enough, and we want to change `<h1>Welcome!</h1>` to `<h1>OMG Welcome!!!</h1>`. We can go to our text editor of choice, make the change, and then run `appbuilder live-sync` to push the change directly to our device - no re-build required! This workflow is shown below.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-live-sync.gif?sfvrsn=2" alt="An Android phone screenshot showing the project open" style="height: 400px;">

There's a lot more to the CLI than this. You can build, deploy, and live sync iOS projects; you can even upload apps to iTunes Connect directly from the command line (`appbuilder appstore-upload`), but we'll leave those topics to future blog posts. For now, let's talk about another of my favorite projects: Sublime Text.

> There's also a more thorough walkthrough of the CLI basics available at [its GitHub repo](https://github.com/Icenium/icenium-cli/blob/docs/README.md).

<h3 id="sublime">Sublime Text Integration</h3>

I'll start with a confession: as much as I love the tooling in the AppBuilder [in-browser client](http://www.telerik.com/appbuilder/in-browser-client), I had a hard time giving up [Sublime Text](http://www.sublimetext.com/). Luckily, I no longer have to be conflicted, because in addition to the CLI, AppBuilder now offers direct Sublime Text integration through a [dedicated Sublime package](https://github.com/Icenium/appbuilder-sublime-package).

There are two ways to install the package. The easiest is through Sublime Text's [Package Control plugin](https://sublime.wbond.net/docs/usage).

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-sublime-package.png?sfvrsn=2" alt="Image of installing ST in package control">

If you're not not a Package Control user, or if manual installation and tinkering is your thing, you can also [grab the code from GitHub](https://github.com/Icenium/appbuilder-sublime-package). (Like the CLI it's also open source.)

After installing the package you need to restart Sublime Text, and then you'll see a shiny new AppBuilder menu under Tools.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-sublime-menu.png?sfvrsn=2" alt="Image of AppBuilder menu in Sublime Text">

> Windows-based Sublime Text users also have a *Run in Simulator* option that opens the application in [AppBuilder Windows client](http://www.telerik.com/appbuilder/windows-client). OS X users will be getting a similar option in our next release.

Personally I'm a huge fan of the *LiveSync On Save* command. With *LiveSync On Save* set, changes made in Sublime Text are instantly rendered on a connected device. This is shown below.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-sublime.gif?sfvrsn=2" alt="An Android phone screenshot showing the project open" style="height: 400px;">

What's really exciting to me is that the Sublime package is a fairly lightweight wrapper around CLI commands. The Sublime Text package's *LiveSync Application* command is a shorthand for running `appbuilder live-sync` from the command line. So if your favorite IDE isn't Sublime Text (or [Visual Studio which we also integrate with](http://www.telerik.com/appbuilder/visual-studio-extension)), let us know! Or better yet, since it's all open source, you can write one yourself!

> For more on the Sublime package, including documentation on each menu option, and information on contributing to the package, see [the package's GitHub repo](https://github.com/Icenium/appbuilder-sublime-package).

<h3 id="windows-phone">Windows Phone 8 Support</h3>

If a CLI and Sublime Text integration weren't enough, we're also happy to announce beta support for Windows Phone. With this release you can design, develop, simulate, and build Windows Phone applications directly within AppBuilder. Let's look at each.

To start, our UI Designer has been updated to support designing Windows Phone apps with Kendo UI Mobile controls.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-ui-designer.png?sfvrsn=2" alt="Image of in-browser editor showing Windows Phone controls">

Next, our list of simulators has a new Windows Phone option to emulate the experience without needing to use an actual device.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-windows-simulator.png?sfvrsn=2" alt="Menu showing a new option for Windows Phone simulation">

And when you are ready for testing on a device, you can perform now perform Windows Phone builds in the cloud.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-windows-build.png?sfvrsn=2" alt="Menu processes for performing a Windows Phone build in AppBuilder">

The build gives you a XAP file that you can [deploy and run](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402565(v=vs.105).aspx) on a [registered Windows Phone device](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff769508(v=vs.105).aspx). Look for more details on this in a coming blog post.

Windows Phone support is in beta, and we have a lot more features on the way - including QR code-based deployment, CLI support, and more. Stay tuned!

<h3 id="version-control">Advanced Version Control Support</h3>

AppBuilder has always had [robust support for git-based version control](http://blogs.telerik.com/appbuilder/posts/13-12-19/git-started-with-github-in-mist), but with this release we've made it even better by supporting creating, deleting, checking out, and merging branches directly within our in-browser and Windows clients.

For instance, here are just some of the things you can do at our in-browser client's version control screen.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-version-control.png?sfvrsn=2" alt="View of the version control screen in AppBuilder in-browser client">

When merge conflicts occur we have a new conflicts view to help you resolve them.

<img src="http://blogs.telerik.com/images/default-source/appbuilder/2-0-conflicts-view.png?sfvrsn=2" alt="View of the version control screen in AppBuilder in-browser client">

With full branching support, large teams can work on multiple features at once - and they never have to leave AppBuilder to do it.

<h3>Wrapping Up</h3>

There's a lot more to talk about - including Android 4.4 support, [faster builds](http://blogs.telerik.com/blogs/14-03-11/telerik-speeds-up-cordova-plugman-26-second-faster-ios-builds!), and LESS file support - so look for more information over the coming weeks. For a full list of all updates and changes check out our [release notes](http://docs.telerik.com/platform/appbuilder/release-notes/v2-0).
