# Speed Up Your Development with NativeScript Forge Debugging

With the introduction of NativeScript Forge last week, we unveiled an entirely new set of features and services to help improve your experience developing cross-platform mobile apps with the NativeScript framework. This week is all about diving deep into some of the most valuable features of Forge, and today we are going to look at how Forge helps you debug your NativeScript apps.

Let's see how NativeScript Forge helps us squash those bugs!

## Debugging

## LiveSync

## What's Next for NativeScript Forge Debugging?




Debugging/LiveSync
•	Do you know when LiveSync will be ready on the internal builds?
There are bugs with edge cases popping up constantly. I expect that the feature will be merged no later than Tuesday but at this time it is a guestimation. The feature works consistently on the happy path for both iOS and Android, local & cloud builds, but the team wants to polish it for the release. Given that it is the heart of NS experience, I think this is the right approach.
•	What is the debugging story with Forge? Do we have any direct integration with VS or VS Code? Or is the expectation that a user just does debugging outside of Forge entirely?
Forge offers debugger based on Chrome DevTools both on Windows and Mac. The users will use it when they prefer a text editor or IDE which is not enlightened (i.e. we have no plugin for). This feature is completed and you can test it today.
For VS, VSC and WebStorm, we will offer plugins which expose the entire feature set of Forge inside the IDE
•	Any future debugging features that we can talk about?
We will expose the “Elements” tab and will update with whatever the core framework team implements.
