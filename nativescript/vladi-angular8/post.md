# NativeScript Now Supports Angular 8

Last week, the Angular team released the brand new [Angular 8](https://blog.angular.io/version-8-of-angular-smaller-bundles-cli-apis-and-alignment-with-the-ecosystem-af0261112a27), and today we’re happy to announce that NativeScript now supports this latest update 🎉.

## What is New with Angular 8?

Angular 8 brings a lot of internal changes and is a stepping stone for the future Ivy implementation making it a major milestone in Angular's history. For a full list of all the changes you can read [this blog post](https://blog.angular.io/version-8-of-angular-smaller-bundles-cli-apis-and-alignment-with-the-ecosystem-af0261112a27).

## Ivy, the Future of Angular

With Angular 8.0.0 the Angular team has shipped a framework which includes Ivy behind an experimental optional flag, `enableIvy`, that you can set in your `angularCompilerOptions`. With this version the benefit of enabling Ivy is not a big one due to the current state of its implementation. Long story short, the current NativeScript plugins are not published in an appropriate format (APF), which means that Ivy's tree-shaking functionality is not possible. The NativeScript team is working closely together with the Angular team on the Ivy implementation and we expect to be able to **deliver day one support** for it when it is ready and available. Stay tuned!

> Check out [this article](https://www.telerik.com/blogs/first-look-angular-ivy) for more information on Ivy.

## Upgrading from 7.x.x to 8.0.0

Let’s look at how you can update your NativeScript-Angular apps to take advantage of these optimizations:

![nativescript angular dependency changes](https://i.imgur.com/dMZUIXE.png)

In order to update your project to the latest **Angular 8.0.0** you simply have to do the following easy steps:

* Install the latest `nativescript-angular` plugin `npm i nativescript-angular@latest --save` and run the automated dependency-updating script provided with the package `./node_modules/.bin/update-app-ng-deps`.
* Install the latest `nativescript-dev-webpack` package `npm i nativescript-dev-webpack@latest --save-dev` and run the automated dependency-updating script provided with the package `./node_modules/.bin/update-ns-webpack --deps --configs` (note that the `--configs` will update your `webpack.config.js`, it is recommended to update that config with each new version of `nativescript-dev-webpack` but you can remove it if necessary).

You are almost done! The only thing left is to check if your project needs to migrate any of the breaking changes [described in our changelog](https://github.com/NativeScript/nativescript-angular/blob/99d699c98e03354de680395372ff1b4a0c875d7b/CHANGELOG.md#breaking-changes). You can find that out by triggering a TypeScript compilation of your project using `tsc`. If you are lucky the terminal will not show any errors and you will be happy to know that you are ready to start using Angular 8.0.0. If you see any errors regarding `ViewChild` simply follow the below migration example.

Anywhere you previously had `@ViewChild` with a single param you now have to provide a second param with a `static` property set to either `true` or `false`

**Previous code:**

	import { ElementRef } from "@angular/core";
	
	@ViewChild("myElement") myElement: ElementRef;

**Migrated code:**

	import { ElementRef } from "@angular/core";
	
	@ViewChild("myElement", { static: false }) myElement: ElementRef;

## Known Issues

Currently we are aware that `@nativescript/schematics` does not work with Angular 8.0.0. We are actively working on an update!

## Final Notes

The `@angular/http` package has been deprecated and we plan to remove it completely in one of our future versions of `nativescript-angular`.

For most users the upgrade to Angular 8 should be seamless. You can refer to our [changelog](https://github.com/NativeScript/nativescript-angular/blob/99d699c98e03354de680395372ff1b4a0c875d7b/CHANGELOG.md#800-2019-05-29) for a full list of things that have changed, and if you run into issues, let us know on our [issue tracker](https://github.com/NativeScript/nativescript-angular/issues).

Feel free to let us know what you think about this latest Angular update in the comments as well.
