# NativeScript Remote Builds

Have you ever wondered how to:

* build NativeScript apps for iOS, on Windows or Linux?
* setup a stable CI for your NativeScript apps?
* avoid iOS code signing management?

If so, the [NativeScript Remote Builds Plugin](https://github.com/NativeScript/nativescript-remote-builds) is just for you! 🚀

## How does it work?

The plugin uses several [NativeScript CLI hooks](https://www.nativescript.org/blog/migrating-cli-hooks-to-nativescript-6.0) and modifies its behavior by:

* **Skipping** the local [native tooling<sup>[1]</sup>](#tooling) requirements.
* **Skipping the native resources** handling during the CLI `prepare` phase - they will be handled remotely.
* **Replacing the CLI local build** with the build method of the selected remote.

> At the time of writing, the plugin supports a [Circle CI](https://circleci.com/) remote build.

The **rest of the CLI logic works as usual**, for example, the `tns debug` command prepares the JavaScript files, uploading to the connected devices, showing logs, opening debug sockets, showing a URL for debugging and so on.

The video below demonstrates how to run a NativeScript app on iOS from a Windows machine:

<iframe width="640" height="360" src="https://www.youtube.com/embed/PA5h8V-0QUU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Why did we choose Circle CI?

Implementing a *remote* requires a few methods - mainly handling environment variables and downloading the build result. Circle CI has **a simple API** for these operations and offers a good number of free Linux and macOS machines for open-source projects.

However, the **plugin abstractions don't depend on Circle CI** and implementing a new remote should take just a few days. For example, we already have initial POC remotes with another cloud solution - Travis CI and local ssh connections to a macOS machine in the same network. Feel free to open feature requests and even pull requests for implementing new remotes in the [plugin GitHub repository](https://github.com/NativeScript/nativescript-remote-builds).

## How does it differ from the NativeScript CLI Cloud extension?

Let's compare the cloud builds that are part of the [NativeScript Cloud extension](https://github.com/NativeScript/nativescript-cloud) and this NativeScript Remote Builds plugin, as they look similar.

The main differences come from the fact that the NativeScript Remote Builds plugin enables the existing NativeScript CLI commands on environments without any [native tooling<sup>[1]</sup>](#tooling) requirements instead of providing additional commands like the `tns cloud` ones. The Remote Builds plugin is also designed to be stable and reliable in a CI environement.

Here's a comparison table between the NativeScript Cloud Extension and the NativeScript Remote Builds plugin:

|                                                                        |                 Cloud Extension               | Remote Builds Plugin |
| :---                                                                   |                      :---:                    |         :---:        |
| Build, Run and Publish without [native tooling<sup>[1]</sup>](#tooling)|               ![](https://i.imgur.com/v9VEBbf.png)              |  ![](https://i.imgur.com/v9VEBbf.png)  |
| Debug without [native tooling<sup>[1]</sup>](#tooling)                 |&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/88WLiNS.png)[<sup>[2]</sup>](#cloudDebug)|  ![](https://i.imgur.com/v9VEBbf.png)  |
| Run Unit Tests without [native tooling<sup>[1]</sup>](#tooling)                  |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| [Automatic iOS Signing Management<sup>[3]</sup>](#signing)                       |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| [Full CI Support<sup>[4]</sup>](#ci)                              |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| [Full Environment Information<sup>[5]</sup>](#envInfo)                 |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| [Full Environment Control<sup>[6]</sup>](#envControl)                     |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| Just the default NativeScript CLI commands                 |                       ![](https://i.imgur.com/JcfimjC.png)                     |  ![](https://i.imgur.com/v9VEBbf.png)  |
| Free                                                       |&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/88WLiNS.png)[<sup>[7]</sup>](#cloudPrice)|&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/88WLiNS.png)[<sup>[8]</sup>](#pluginPrice)|

> *The comparison is based on the CircleCI remote of the NativeScript Remote Builds Plugin*

<dl>
  <dt><span id="tooling">native tooling<sup>[1]</sup></span></dt>
  <dd>Any native environment requirements like Java, Android SDK, Android Studio, macOS, Xcode, Cocoapods, Docker and so on.</dd>

  <dt><span id="cloudDebug">cloud extension debug support<sup>[2]</sup></span></dt>
  <dd>It is available only through NativeScript Sidekick.</dd>

  <dt><span id="signing">iOS signing management<sup>[3]</sup></span></dt>
  <dd>If the end-user is responsible for passing the `--provision` flag and picking the proper certificate based on the current build configuration. In the Remote Builds plugin, this is handled out of the box by the <a href="https://docs.fastlane.tools/actions/match/">Fastlane Match service</a>.</dd>

  <dt><span id="ci">full CI support<sup>[4]</sup></span></dt>
  <dd>If the user can build and run tests on pull requests or commits. In other words, if it is supported to build multiple versions of the same app in parallel.</dd>

  <dt><span id="envInfo">full environment information<sup>[5]</sup></span></dt>
  <dd>If the full environment information is available to the users (e.g. the versions of the OS and the <a href="#tooling">native tooling<sup>[1]</sup></a>.</dd>

  <dt><span id="envControl">full environment control<sup>[6]</sup></span></dt>
  <dd>If the environment setup can be controlled by the user (e.g. change the versions of the os and the <a href="#tooling">native tooling<sup>[1]</sup></a>.</dd>

  <dt><span id="cloudPrice">cloud extension price<sup>[7]</sup></span></dt>
  <dd>The NativeScript Cloud extensions is providing a limited number of free builds.</dd>

  <dt><span id="pluginPrice">remote builds plugin price<sup>[8]</sup></span></dt>
  <dd>The Circle CI provider of the NativeScript Remote Builds plugin is just depending on the <a href="https://circleci.com/pricing/">Circle CI pricing</a>. It provides a limited number of free Android Builds for everyone and a limited number of <a href="https://circleci.com/open-source/">free iOS builds for open-source projects</a>.</dd>
</dl>

## How Do I Get Started?

### Step 1: Update to NativeScript 6.4 or later

During plugin development, we had to expose a few more NativeScript CLI hooks and edit the iOS native project template in our iOS Runtime. The required changes are available in NativeScript CLI and iOS Runtime 6.4.

### Step 2: Follow the plugin installation and setup guide

The up-to-date installation info, setup guide, and available remotes are available in the [plugin's README](https://github.com/NativeScript/nativescript-remote-builds).

### Step 3: We love your feedback

Leave a comment or open an issue with your feedback. Your comments and suggestions are very important to us!