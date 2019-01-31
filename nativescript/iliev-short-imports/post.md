# Say Goodbye to Short Imports in NativeScript!

NativeScript uses the `tns-core-modules` plugin to provide the core developer experience through many tools including the UI elements, access to the file system, connectivity, platform and application tools, and so on. The plugin is automatically added to each newly created project, and its modules can be accessed via JavaScript's `require` or TypeScript's `import`. 

Here is an example for using `tns-core-modules` in a NativeScript application:

	// TypeScript imports (for TypeScript or Angular based projects)
	import { EventData } from "tns-core-modules/data/observable";
	import { Label } from "tns-core-modules/ui/label";
	
	export function navigatingTo(args: EventData) {
	    const label = new Label();
	}
	
	// JavaScript require (for plain JavaScript projects)
	const Label = require( "tns-core-modules/ui/label").Label;
	
	exports.navigatingTo = function(args) {
	    const label = new Label();
	}

## Deprecation of the Short Imports in NativeScript 5.2

The last few versions of NativeScript brought a ton of new features including the Preview app, extended Webpack support, Hot Module Replacement, cloud builds via CLI, and so on. The complexity of some features and the concept that Webpack should be available out-of-the-box for all NativeScript applications *required the team to deprecate short imports*! 

_Q: Wait a minute! What is a **short import**?_

A: An import that is shortened from `tns-core-modules/something` to `something`.

Example of short import:

	// deprеcated short import (TypeScript)
	import { device } from "platform"; 
	
	// deprеcated short import (JavaScript)
	let device = require("platform").device; 

The above code should be rewritten to:

	// TypeScript module import
	import { device } from "tns-core-modules/platform"; 
	
	// JavaScript module import
	let device = require("tns-core-modules/platform").device;

Historically, the short imports were a feature in NativeScript from day one. The idea was to ease the developer's experience. However, the short imports are inconvenient for some reasons:

- Technical debt - Any new feature that requires access to `tns-core-modules` should consider if the path is short or full and if the plugin is external or embedded (e.g. [using import in the Preview app](https://github.com/NativeScript/nativescript-cli/issues/3997#issuecomment-428210585)).

- Usability - NativeScript developers might be confused when using a short import. In the following example, you might think that we are importing from a standalone library when in fact, this is a module from the `tns-core-modules` plugin.

	import * as http from "http";
	// this is actually a short import for `tns-core-modules/http`

These are the reasons why its time to say goodbye to the short imports. With NativeScript 5.2 these are now deprecated.

_Q: Should I take any steps?_

A: **If you are an application developer, make sure to migrate all short imports to full imports.** If you own a NativeScript plugin, you will have to migrate the plugin imports from short to the full syntax as well.

The NativeScript teams have already migrated the plugins under the NativeScript organization to use full imports. The next step is to iterate through the community plugins and to inform the plugin authors about the needed change.


