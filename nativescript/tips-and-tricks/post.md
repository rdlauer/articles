# {N} Tips and Tricks

1.	Visual Studio Code snippets for NativeScript.
2.	Using the Angular CLI generate commands with nativescript/schematics. I am about to push a PR to our docs that covers this topic.

Trick 1: Enable router tracing.
It’s not really NS-specific but it helps a lot in debugging router issues and figuring out what’s wrong with you routing config:
It’s done with passing an option when calling NativeScriptRouterModule.forRoot:
NativeScriptRouterModule.forRoot(routes, { enableTracing: true})

The result is that all router events are logged in your console (including error in resolving routes):
JS: Router Event: NavigationStart
JS: NavigationStart(id: 2, url: '/home')
JS: Router Event: RoutesRecognized
JS: RoutesRecognized(id: 2, url: '/home', urlAfterRedirects: '/home', state: Route(url:'', path:'') { Route(url:'home', path:'home') } )
JS: Router Event: GuardsCheckStart
JS: GuardsCheckStart(id: 2, url: '/home', urlAfterRedirects: '/home', state: Route(url:'', path:'') { Route(url:'home', path:'home') } )
… and so on

Trick 2: Enable tracing
There are a bunch of build-in tracing categories that we use in the tns-core-modules and ns-angular for logging. Enabling them might provide insights on what is happening inside the framework, when debugging a problem in a specific area. Here are some useful categories to enable
import * as trace from "tns-core-modules/trace";

// Logs info on measure and layout passes
trace.addCategories(trace.categories.Layout); 

// Logs info when modifying the view tree (adding views, removing views, etc.)
trace.addCategories(trace.categories.ViewHierarchy);

// Logs info related to navigation
trace.addCategories(trace.categories.Navigation);

// Don't forget to enable tracing
trace.enable();

There are more “core-module” categories in trace.categories.XXX. For angular project there are also angular-specifc tracing categories in "nativescript-angular/trace" module.

Trick 3: Profiling module
There are a bunch of interesting utilities in the profiling modules ("tns-core-modules/profiling").
Some useful methods:
•	time() the most accurate (and fast) way of getting time in NS
•	start("my-timer-name") / stop("my-timer-name"). The stop method will return an TimerInfo object with some useful information like how many start/stop iterations were executed, total time, time of the last measurement etc.
•	@profile decorator. You can add it on methods and it will automatically track how many time the execution goes through this method and the total time it took. You can dump all the info collected by profile decorators with dumpProfiles(). You should call enable() to use the decorator though.

- tns resources generate
 - Code samples
 - Using the plugin seed
 - Android image optimization

fonts


1.	USE TYPESCRIPT!!!
2.	USE VISUAL STUDIO CODE!
3.	Use Visual Studio Code (with {N} plugin) to set breakpoints in JS code and inspect variable values as app runs on device/simulator
a.	Once you hit a breakpoint, you can start running arbitrary commands via the VS Code command line
b.	Good for testing code you think should work or exploring available APIs
4.	When in doubt, delete your node_modules, platform, hooks folders and rebuild/rerun your app to start “clean”
5.	When building for iOS, you can always use XCode to build, deploy your app
a.	Just be sure to open the *.xcworkspace and NOT the *.xcodeproj file
6.	You can find the raw build assets for iOS and Android in platforms > [ios/android] > build
7.	Use font icons for most of the small images you in need in an app (don’t create images at 1x, 2x, 3x for everything)
8.	Use custom data binding converters to translate data in to images (like weather icons or images for specific model numbers, etc)
9.	Make sure you have links to a privacy policy, terms of use, etc in your app if you plan to submit via iOS AppStore
10.	AND if you don’t have these assets, there are tools to help you quickly generate them. Examples:
a.	https://termly.io/privacy-policy/privacy-policy-generator
b.	https://privacypolicies.com/
c.	https://getterms.io/ 
11.	 iOS AppStore tracks app VERSION and BUILD NUMBER
a.	New VERSIONS trigger a manual review by the AppStore (even during beta testing)…which can take some time
b.	New BUILD NUMBERS are usually approved INSTANTLY during beta testing
c.	If you want to get a new beta out via TestFlight quickly, ONLY increment the build number (dirty, but works)
12.	Think about how you’re going to monitor/triage app crashes/bugs when you’re ready to ship your app
a.	If you don’t add some kind of logging, it’s a black box once you ship your app


I meant that it is possible to add {N} extensions which don’t have native components while using the Playground. It’s “hidden” behind the “Add NPM package” menu entry.