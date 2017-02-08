# How TypeScript Will Reshape the Enterprise Developer

JavaScript is ubiquitous. From its birth in the browser, to a re-birth on the server, to powering native mobile app development and IoT devices, it is becoming increasingly difficult to argue against the importance of JavaScript. As the most recent [Stack Overflow survey](http://stackoverflow.com/research/developer-survey-2016#technology-most-popular-technologies) attests, the popularity of JavaScript among rank-and-file developers has held steady for four years running. In fact, two of the other "most popular technologies" in the poll are *based* on JavaScript ([Angular](https://angularjs.org/) and [Node](https://nodejs.org/en/)).

However, one place in which JavaScript has only just started to make a dent is the "enterprise". Today your typical enterprise developer is focused on the .NET or Java stacks. Why? Because (typically) they are dealing with large ERP systems that expose APIs most easily consumable by said languages and frameworks.

What makes us think anything is going to change in 2017? Why would enterprise developers start to abandon their language of choice for an unfamiliar and &lt;gasp&gt; weakly typed alternative?

The answer may be [TypeScript](http://www.typescriptlang.org/).

## What is TypeScript

TypeScript is a superset of JavaScript. This is a fancy way of saying that TypeScript *extends* JavaScript and provides static typing, classes, and interfaces. One of the primary benefits of TypeScript is the ability for modern IDEs to provide an environment for features like autocompletion (a.k.a. intellisense) for JavaScript-based projects. This significantly increases developer productivity by reducing syntax mistakes and runtime code errors.

TypeScript is an open source project developed and maintained by Microsoft. Adoption of TypeScript is taking off dramatically:

![typescript downloads on npm](other-npm-stat.png)

*Total downloads for TypeScript on npm have increased from ~50K in January 2015 to over 2.4 MILLION in December 2016.*

While we can't deny TypeScript's popularity, it is fair to question the place TypeScript has in the enterprise. How could TypeScript/JavaScript start to displace C# or Java in the workplace? Let's look at some advantages of adopting JavaScript and the reasons the enterprise may need to move.

## Non-Traditional Device Access

JavaScript is already available in non-traditional "computers" like [Arduino](https://www.arduino.cc/) and the [Raspberry Pi](https://www.raspberrypi.org/). Internet of Things (IoT) devices such as sensors, home automation tools, and fitness trackers already provide JavaScript APIs. We are seeing an explosion in non-desktop devices and the lowest common denominator for programmatic access tends to be JavaScript.

What about mobile? For years now developers have been focused on Objective-C and/or Java for creating native iOS and Android apps. Today though "native JavaScript" frameworks like [NativeScript](https://www.nativescript.org/) exist to let developers re-use these skills.

If the developer can re-use his or her skills moving from platform to platform, or device to device, doesn't it make sense to standardize on one language?

## Client AND Server Development

No other language than JavaScript better addresses both client-side and server-side development. JavaScript is most widely thought of as a scripting language for web browsers. While that 90's-era view of JavaScript is not inaccurate, the last decade has seen a dramatic expansion of JavaScript as a legitimate language on the server as well. Node.js came about in 2009 as the preeminent server-side runtime for JavaScript. This opened up numerous doors for server-side web and networking-focused development. With the advent of Node's long term support (LTS) plan, Node is poised to continue its meteoric rise in the enterprise in 2017.

Even large ERP vendors such as Salesforce recognize this trend and offer a [JavaScript API](https://jsforce.github.io/) for both client and server-side development. Amazon's AWS, Microsoft Azure, SAP, PeopleSoft, are just a handful of vendors who offer a JavaScript interface for their systems.

## New Breed of Engineers

Generally speaking, younger developers are more familiar with modern JavaScript development. As this group invades the enterprise, the reliance on today's skill set will diminish for new, greenfield, application development. Make no mistake, C# and Java aren't going anywhere - there is plenty of legacy app maintenance that will be needed. However, like .NET and Java replaced COBOL and Fortran, JavaScript may be on its way to replacing today's stalwarts.

## Tooling and Language Considerations

TypeScript will never succeed in the enterprise unless developers only have to change a minimum amount of tooling. The fact that TypeScript has been included in Visual Studio since 2012 is one small step. A [plugin exists](https://marketplace.eclipse.org/content/typescript) for using TypeScript in Eclipse. Tool re-use is becoming a non-issue.

What about more fundamental roadblocks like language constructs? C# and Java developers have both gotten so used to generics for instance. In the JavaScript world there is no such thing as a generic. Until TypeScript came along that is. TypeScript adds support for modern language constructs like [generics](https://www.typescriptlang.org/docs/handbook/generics.html), [arrow functions](https://www.typescriptlang.org/docs/handbook/functions.html), [modules](https://www.typescriptlang.org/docs/handbook/modules.html), [classes](http://www.typescriptlang.org/docs/handbook/classes.html), and [interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html). It helps to make JavaScript more like the object-oriented language you've been using all these years and less like the dynamic scripting language you used to think of.

## TypeScript == Enterprise JavaScript

When you adopt TypeScript (and by virtue adopt JavaScript), you are opening your enterprise to countless possibilities and future-proofing your business.