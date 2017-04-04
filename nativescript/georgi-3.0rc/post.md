# NativeScript 3.0 Release Candidate Available Today

We are super excited to announce the immediate availability of the NativeScript 3.0 Release Candidate! This is a major milestone for the project and the team - who worked hard to prototype, implement, and stabilize the new bits. As you may have [already read in this post](https://www.nativescript.org/blog/sneak-preview-of-nativescript-3.0), the 3.0 release introduces many performance improvements and some breaking changes. We believe that every single change introduced makes the NativeScript framework more stable, valuable, and performant.

To ease the migration path we have prepared a [comprehensive document](https://progresssoftware-my.sharepoint.com/personal/vakrilov_progress_com/_layouts/15/WopiFrame.aspx?docid=010adaf93ef8b412694e0cac9bf7ff12a&authkey=ATk6nC6ZsoCrTNMxE16Eb_A&action=view) to explain the changes, why they were needed, and what value they add. We've also compiled a [complete migration guide](https://github.com/NativeScript/NativeScript/blob/31063ee732256d306b172a1ad48d4c17399c809a/Modules30Changes.md) that will help you with upgrading your apps to 3.0.

## Release Highlights

*Here is an overview of what's included in the 3.0 RC.*

### Cross-Platform Modules

- **A fresh new modules 3.0 implementation!** The modules refactoring is based on three major pillars:
	- Performance improvements;
	- Easier extensibility model;
	- More consistent APIs across the entire cross-platform modules implementation.
- **A completely revamped layout measurement system.** Prior to 3.0, the layout system worked with DIP (device-independent pixels). With 3.0, everything is calculated down to DP (device pixels) and you can specify the **px** suffix to define an exact **1-px** wide border, for instance.
- **Migration to TypeScript 2.2** and removal of ambient modules in favor of explicit path resolution.
- Numerous existing issues addressed.

### NativeScript CLI

- We made the following behavioral changes to the NativeScript CLI:
	- The `livesync` command is removed in favor of `tns run` which now automatically performs `livesync --watch` under the hood;
	- The `plugin find/search` command is removed as it was not used;
	- The `emulate` command is now deprecated and its functionality has been exposed through the `run` command and its `--emulate` option;
	- The `run --device <Device Identifier>` command now starts an emulator if one is not started already. Prior to 3.0 the `--device` option worked only with physical devices.
- Many internal improvements that ensure better stability and extensibility of the codebase.
- Numerous bug fixes.

### iOS and Android Runtimes

- **Network Domain enabled for Chrome DevTools.** Now you can monitor your application network traffic directly from within [Chrome DevTools](https://docs.nativescript.org/runtimes/android/debug/debug-cli). *Note: this is currently available for the built-in [http module](https://docs.nativescript.org/cookbook/http). For third-party modules that do network requests, additional glue code must be implemented to populate the Network Tab.*
- Reverted static bindings code generation for Android instead of producing *.DEX files directly. The code generation approach has several major advantages that affect build speed, readability, and maintainability.
- Updated Gradle build tool. **This will speed up the Android builds significantly.**
- Improved debugging experience in Chrome DevTools for Android. We have made several fixes related to crashes and bugs while debugging.

## Enabling NativeScript 3.0 RC

You can install the new NativeScript package from npm (behind the RC tag):

	npm install -g nativescript@rc

### Create New Projects with NativeScript 3.0 RC

You can create a new project based on the new 3.0 templates:

- Vanilla JavaScript Application: `tns create MyApp --template tns-template-hello-world@rc` 
- TypeScript Application: `tns create MyApp --template tns-template-hello-world-ts@rc` 
- Angular Application: `tns create MyApp --template tns-template-hello-world-ng@rc`

These commands will create a project and the 3.0 RC `tns-core-modules@rc` will be added.

At this point, you may add the RC bits of the platform you are targeting:

`tns platform add android@rc` and `tns platform add ios@rc`

**That's it!** You can now start the project with with `tns run <platform-name>`.

### Update Existing Projects

In order to update existing projects, you will have to update the `tns-core-modules` and platforms for your app:

	tns plugin remove tns-core-modules 
	tns plugin add tns-core-modules@rc
	tns platform remove <platform-name>
	tns platform add <platform-name>@rc

For both TypeScript and Angular applications you should also update the TypeScript plugin:

	npm uninstall nativescript-dev-typescript --save-dev
	npm uninstall typescript --save-dev
	npm install nativescript-dev-typescript@0.4 --save-dev

Finally, for Angular projects, you need to do the following:

1) Update the `nativescript-angular` plugin:

	tns plugin remove nativescript-angular
	tns plugin add nativescript-angular@rc

2) Update to Angular 4 by opening the `package.json` file of your project and update the various Angular packages to 4.0.0. Additionally, the `zone.js` dependency should now be listed as a dependency rather than a devDependency.

> Read more about updating to Angular 4 [in this blog post](https://www.nativescript.org/blog/nativescript-supports-angular-4).

As a result your `package.json` should look like this:

    "dependencies": {
        "nativescript-theme-core": "~1.0.2",
        "nativescript-angular": "rc",
        "tns-core-modules": "rc",
        "@angular/animations": "4.0.0",
        "@angular/core": "4.0.0",
        "@angular/common": "4.0.0",
        "@angular/compiler": "4.0.0",
        "@angular/http": "4.0.0",
        "@angular/platform-browser": "4.0.0",
        "@angular/platform-browser-dynamic": "4.0.0",
        "@angular/forms": "4.0.0",
        "@angular/router": "4.0.0",
        "rxjs": "~5.2.0",
        "reflect-metadata": "~0.1.8",
        "zone.js": "~0.8.2"
    },
    "devDependencies": {
        "typescript": "~2.2.1",
        "nativescript-dev-typescript": "~0.4.0",
        "nativescript-dev-android-snapshot": "^0.*.*"
    }

## Known Limitations

There are some known limitations that the team is currently fixing. However, we wanted to release the new bits today as we believe they are stable enough to allow for early experimentation. Here are the limitations we are aware of:

- No support for Xcode 8.3
- Cannot deploy to a physical iOS device unless the `--syncAllFiles` option is specified
- No snapshot packages available for Android

**We plan on releasing a new RC version once these issues are resolved.** We may have several RC releases prior to the official release, to ensure critical issues are resolved as soon as possible and not force our users wait 4 weeks for a fix!

## Share Your Feedback!

Please [let us know of any issues](https://github.com/NativeScript/NativeScript/issues/new) that you find with this release candidate, so that we may address them prior to the official release.

While we believe only a small subset of all the existing [NativeScript plugins](http://plugins.nativescript.org/) will be affected by the changes (mainly those which provide a custom UI), we do want to make sure issues are identified and fixed as soon as possible!

## When is the Official 3.0 Release?

We are aiming for **April 26th, 2017** as the official 3.0 release date. In the four week period between the RC and the official release date, we will dedicate ourselves to fix any issues that may arise, help authors migrate their plugins to 3.0, and update the documentation and getting started materials.

## Next Steps

- Read the [comprehensive document on NativeScript 3.0 changes](https://progresssoftware-my.sharepoint.com/personal/vakrilov_progress_com/_layouts/15/WopiFrame.aspx?docid=010adaf93ef8b412694e0cac9bf7ff12a&authkey=ATk6nC6ZsoCrTNMxE16Eb_A&action=view);
- Read the [complete migration guide](https://github.com/NativeScript/NativeScript/blob/31063ee732256d306b172a1ad48d4c17399c809a/Modules30Changes.md);
- Install the NativeScript 3.0 RC (using the instructions provided above)!