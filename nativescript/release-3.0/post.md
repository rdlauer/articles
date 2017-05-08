# NativeScript 3.0 Available Today

We are incredibly excited to announce the official release of NativeScript 3.0. Since the [3.0 Release Candidate](https://www.nativescript.org/blog/nativescript-3.0-release-candidate-available-today), we've been working hard to fix all major issues and ease the migration path to this new major version of the framework. We closed about **80 pull requests** across all repositories and **upgraded dozens of plugins and applications** to ensure a smooth migration experience for our users. We have also [prepared a document](https://docs.google.com/document/d/1sEzoE_lTK2XjrozzT87U-4qUFTzIR8vez1xxWcKlZb8/edit) to describe the need behind a major version bump and the newly introduced [breaking changes](https://github.com/NativeScript/NativeScript/blob/master/Modules30Changes.md).

You can try out the new 3.0 bits by running:

	npm install -g nativescript

...and follow our documented [upgrade instructions](https://docs.nativescript.org/releases/upgrade-instructions) for upgrading individual projects.

> Be sure to [register to attend](https://www.nativescript.org/blog/register-for-the-nativescript-3.0-webinar) the free NativeScript 3.0 webinar (coming up on May 17th!) to learn all about this release and be the first to hear exciting news about our future product plans.

## Release Highlights

*Here is an overview of what's included in NativeScript 3.0:*

### Cross-Platform Modules

- **The new Modules 3.0 implementation.** The 3.0 refactoring is based on three major pillars:
	- Performance improvements;
	- Enhanced extensibility model;
	- More consistent APIs across the entire cross-platform modules implementation.
- **A completely revamped layout measurement system.** Prior to 3.0, the layout system worked with DIP (device-independent pixels). With 3.0, everything is calculated down to DP (device pixels) and you can specify the **px** suffix to define a **1-px** wide border, for example.
- **Migration to TypeScript 2.2** and removed ambient modules in favor of explicit path resolution.

### CLI

- We made the following behavioral changes in the Command-line tool:
	- The `livesync` command is removed in favor of `tns run` which now automatically performs `livesync --watch` under the hood;
	- The `plugin find/search` command is removed as it had zero usage;
	- The `emulate` command is now deprecated and its functionality has been exposed through the `run` command and its `--emulate` option;
	- The `run --device <Device Identifier>` command now starts an emulator if one is not started already. Prior to 3.0, the `--device` option worked only with running devices.
- Numerous bug fixes and internal improvements that ensure better stability and extensibility of the codebase.

### Runtimes

- **Network Domain enabled for Chrome DevTools.** Now you can monitor your application network traffic directly from within [Chrome DevTools](https://docs.nativescript.org/runtimes/android/debug/debug-cli). This is currently available for the built-in http module. For third-party modules that do network requests, additional glue code must be implemented to populate the Network Tab.
- **Reverted static binding code generation for Android** instead of producing *.DEX files directly. The code generation approach has several major advantages that affect build speed, readability, and maintainability.
- **Updated Gradle build tool.** This will speed up the Android builds significantly.
- **Improved debugging experience in Chrome DevTools for Android.** We have made several fixes related to crashes and bugs while debugging.

> Please note that we decided to wait to release the [local snapshot builds](https://github.com/NativeScript/android-snapshot/issues/17) feature due to breaking changes in the underlying native build tool chains (i.e. the new Android SDK tools and Xcode 8.3). While the snapshot feature is important for further optimizing the loading time for Android applications, we decided that we shouldn't delay the 3.0 release and will deliver the feature as an incremental update in the near future.

## Enabling the New Bits

Besides the standard new npm packages update `npm install -g nativescript`, if your project uses TypeScript, you will need to update the [nativescript-dev-typescript](https://github.com/NativeScript/nativescript-dev-typescript) plugin and it will automatically update the `tsconfig.json` file for you. Install the new TypeScript plugin with `tns install typescript`.

For a comprehensive list of all the breaking changes and how to migrate existing code you can refer to [this wiki page](https://github.com/NativeScript/NativeScript/blob/master/Modules30Changes.md). And if you are a plugin author we have also prepared [this document](https://github.com/NativeScript/NativeScript/wiki/Migrating-Plugins-to-NativeScript-3.0) to guide you through the plugin migration steps for 3.0.

## WebPack vs. Android Snapshot

For 3.0 we are turning off the snapshot-by-default fearue for Android Angular projects. This is primarily done because for Angular projects the major benefit that affects the loading time is the AoT feature (not present in the existing snapshot architecture) that comes enabled through our [nativescript-dev-webpack](https://github.com/NativeScript/nativescript-dev-webpack) plugin.

WebPack is the recommended way to optimize your NativeScript Angular applications. Once we ship the local snapshot builds then your applications will be able to take advantage of WebPack, AoT, and snapshots for Android simultaneously. More information about enabling WebPack in a NativeScript application may be found in [this documentation article](https://docs.nativescript.org/tooling/bundling-with-webpack).

## Share Your Feedback

We have no less than 600 Jenkins jobs that run continuously to ensure the stability and quality of the framework, but no software is flawless :). Please, [let us know of any issue](https://github.com/NativeScript/NativeScript/issues/new) that you may hit with the new release so that we can fix it as soon as possible!
